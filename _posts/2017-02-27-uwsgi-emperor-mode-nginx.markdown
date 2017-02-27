---
layout: post
title:  "使用 uwsgi 的 emperor 模式支持多个 Flask 应用"
date:   2017-02-27 Mon 22:15:56
categories: coding
---

当在同一台服务器上使用 `uwsgi` 将多个 `Flask` 应用与 `nginx` 连接时，需要使用 `uwsgi` 的 `emperor` 模式。具体做法 (之一) 是这样的：

1\. 新建一个目录，用来存放 `uwsgi` 的配置文件。现在假设这个目录是 `/opt/uwsgi_config/`。

2\. 在这个目录下新建两个文件:

{% highlight sh %}
    文件一：uwsgi_A.yaml
    内容如下：
{% endhighlight %}

{% highlight python linenos=table %}
    uwsgi:
        uwsgi-socket: /tmp/uwsgi_A.sock
        wsgi: serve_A:app
        chmod-socket: 666
        enable-threads: 1
        master: 1
        workers: 4
        threads: 1
        chdir: /data/www/A.com
{% endhighlight %}

{% highlight sh %}
    文件二：uwsgi_B.yaml
    内容如下：
{% endhighlight %}

{% highlight python linenos=table %}
    uwsgi:
        uwsgi-socket: /tmp/uwsgi_B.sock
        wsgi: serve_B:app
        chmod-socket: 666
        enable-threads: 1
        master: 1
        workers: 8
        threads: 1
        chdir: /data/www/B.com
{% endhighlight %}

3\. 启动 `uwsgi`
{% highlight sh %}
    uwsgi --emperor /opt/uwsgi_config/
{% endhighlight %}

4\. `nginx` 参考配置

{% highlight sh linenos=table %}
    server {

        # ...

        location / {
            root   html;
            index  index.html index.htm;
            include uwsgi_params;
            uwsgi_pass unix:/tmp/uwsgi_A.sock;
        }
        location /B_serve_path {
            root   html;
            index  index.html index.htm;
            include uwsgi_params;
            uwsgi_pass unix:/tmp/uwsgi_B.sock;
        }

        # ...
    }
{% endhighlight %}

5\. 以下命令可让 `uwsgi` 自动重启
{% highlight sh %}
    touch /opt/uwsgi_config/uwsgi_*.yaml
{% endhighlight %}

6\. nginx 的配置文件位于 `/usr/local/etc/nginx`, 启动 `nginx` 需要 `sudo`；重启 `nginx` 的命令是 `sudo nginx -s reload`；停止 nginx 的命令是 `sudo nginx -s stop`；需要注意 `user` 的设置。

7\. 注意，在 `Flask` 中使用 `MongoDB` 时，不能使用全局 `mongo_client`，而必须在每个函数内单独开启 `mongoDB` 连接，否则会出错，因为 `mongoDB` 不是线程安全的。
