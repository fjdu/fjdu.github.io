---
layout: post
title:  "在Python里使用系统安装的Shadowsocks穿墙"
date:   2018-12-16 Sun 21:08:56
categories: computer
---

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
