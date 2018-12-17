---
layout: post
title:  "使用JPL提供的HORIZONS服务查询太阳系天体轨道参数"
date:   2018-12-16 Sun 21:08:56
categories: computer
---

需要对网络做一些设置，也需要对源代码做一些改变。

# 在Python里使用系统安装的Shadowsocks穿墙

系统的ShadowsocksX在运行时，会自动在本机开启代理服务，端口号是1080。

通过下面的代码可以让`urllib`通过这个代理连接外网：
{% highlight python linenos=table %}
import socks
import socket
socks.set_default_proxy(socks.PROXY_TYPE_SOCKS5, '127.0.0.1', 1080)
socket.socket = socks.socksocket
{% endhighlight %}

测试：
{% highlight python linenos=table %}
import urllib
res = urllib.request.urlopen(
    'https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/de432s.bsp',
    timeout=50)
r = res.read()
{% endhighlight %}

在命令行使用ShadowsocksX的代理，测试ShadowsocksX有没有起作用：
{% highlight python linenos=table %}
curl --socks5 127.0.0.1:1080 http://cip.cc
{% endhighlight %}
可以与下面命令的结果比较：
{% highlight python linenos=table %}
curl http://cip.cc
{% endhighlight %}
对比上面两个命令的结果应该可以看出差别；第一个命令的输出应该出现ShadowsocksX服务器的地址。

# 修改astropy源代码

在使用`astropy`连接JPL的HORIZONS服务时，需要对源代码做少量修改。

`astropy/coordinates/solar_system.py`:
把
{% highlight python linenos=table %}
        value = ('http://naif.jpl.nasa.gov/pub/naif/generic_kernels'
                 '/spk/planets/{:s}.bsp'.format(value.lower()))
{% endhighlight %}
里的`http`改成`https`。

`astropy/utils/iers/iers.py`:
把
{% highlight python linenos=table %}
        IERS_A_URL = 'http://maia.usno.navy.mil/ser7/finals2000A.all'
{% endhighlight %}
里的`maia.usno.navy.mil`改成`199.211.133.23`；这是由于域名解析问题。

# 查询HORIZONS并画图

{% highlight python linenos=table %}
pmo_dlh = {'lon': 97.56,
           'lat': 37.37,
           'elevation': 3.2}

obj = Horizons(id='900543', # Comet 46P
               location=pmo_dlh,
               epochs={'start': '2018-12-14',
                       'stop': '2018-12-18',
                       'step': '10m'})

eph = obj.ephemerides()

fig = plt.figure(figsize=(8, 8))
ax = fig.add_axes((0.1, 0.57, 0.85, 0.4))
ax.plot(eph['datetime_jd'], eph['delta_rate'])
ax.set_ylabel(r'Velocity$_{\rm obs}$ (km/s)')
ax.yaxis.set_minor_locator(ticker.AutoMinorLocator())
ax.xaxis.set_minor_locator(ticker.AutoMinorLocator(n=4))

days = ['2018-12-'+str(d) for d in range(14, 31)]

xticks = [Time(t).jd for t in days]
ax.set_xticks(xticks)
ax.set_xticklabels([])
ax.grid(axis='both', which='major', color=(0,0,0))
ax.grid(axis='both', which='minor', color=(0.6,0.6,0.6))

ax = fig.add_axes((0.1, 0.15, 0.85, 0.4))
ax.plot(eph['datetime_jd'], eph['delta'])
ax.set_xlabel('Date')
ax.set_ylabel(r'Distance (AU)')

xticks = [Time(t).jd for t in days]
ax.set_xticks(xticks)
ax.set_xticklabels(days)
ax.xaxis.set_minor_locator(ticker.AutoMinorLocator(n=4))
for tick in ax.xaxis.get_ticklabels():
    tick.set_rotation(60)
ax.grid(axis='both', which='major', color=(0,0,0))
ax.grid(axis='both', which='minor', color=(0.6,0.6,0.6))
{% endhighlight %}

<img src="{{ site.url }}/pictures/2018-12-16-shadowsocks-python_jpl_horizons_comet_46P.png" id="Fig1">
<p class="image-caption">彗星46P相对观测站的视线方向速度和距离的变化。</p>
