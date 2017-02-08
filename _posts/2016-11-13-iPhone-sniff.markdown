---
layout: post
title:  "iPhone 抓包"
date:   2016-11-13 Sun 16:38:14
categories: network
---

抓包过程

1. 用 USB 线将 iPhone 与电脑连接，从 iTunes 上获得 iPhone 的 UDID
2. 在 Mac 上执行命令 `rvictl -s <UDID>`
3. 在 Mac 上执行命令 `ifconfig -l`，会看到出现了 `rvi0` 这个虚拟网络界面
4. 在 Mac 上执行命令 `sudo tcpdump -i rvi0 -w ./output.pcap` 开始抓包
5. `control-C` 停止抓包
6. 在 Mac 上执行命令 `rvictl -x <UDID>` 移除虚拟网络界面
7. 可以用 Wireshark 打开文件 output.pcap 进行分析

微信与服务器端的通信

1. 先在电脑上用 Chrome 打开公众号历史记录的链接 (像 [http://mp.weixin.qq.com/mp/getmasssendmsg?__biz=MzI4MjAwOTY3NA==#wechat_webview_type=1&wechat_redirect](http://mp.weixin.qq.com/mp/getmasssendmsg?__biz=MzI4MjAwOTY3NA==#wechat_webview_type=1&wechat_redirect) 这样的)，此时是打不开的 (提示“请在微信客户端打开链接。”)
2. 用手机微信扫描这个链接的二维码，同时开始抓包
3. 手机微信打开历史记录页面后停止抓包
4. 会看到，手机扫二维码后立即有个 DNS 查询的过程，查询的域名是 `mp.weixin.qq.com` (这正是公众号历史记录的域名)。返回了两个 IP 地址：180.163.26.36 和 180.163.21.166；后续使用的是第一个。此过程耗时为：3.055376 - 3.064436
5. 接下来是一个连续的长时间过程，耗时为：3.066440 - 3.318681；这个过程分为多个步骤
    1. 手机与服务器进行了简单沟通，貌似此过程没有传递任何消息，大概只是为了检验是否能连接
    2. 手机对服务器用 TLS 打招呼；涉及到椭圆曲线加密什么的
    3. 服务器确认
    4. 服务器对手机发来 TLS 招呼，里面包含 Random Bytes: cf63712a69ed44f9d806d8766ee8edc019535f4cca575ed1..., Session ID: 19:ff:ca:5c:58:01:00:00:03:6a:17:a9:ba:9a:1f:a7:f2:6a:58:3c:52:2e:1c:c8:26:18:9c:c3:a0:11:32:38，Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f), Compression Method: null (0).  附带的数据中可看到 GeoTrust SSL CA - ...Guangdong1.0 Shenzhen1:08...Tencent Computer Systems Company Limited1.0...R&D...mp.weixin.qq.com0...game.weixin.com... 等字样
    5. 接下来 4 个 frame 用于 key 交换
    6. 接下来又是一个打招呼的过程
    7. 客户端公钥交换
    8. 客户端发过去一个加密过的握手信息
    9. 服务器确认收到
    10. 服务器发来一个加密过的握手信息
    11. 客户端确认
    12. 客户端通过两个 frame 发送加密数据
    13. 服务器确认
    14. 服务器通过 9 个 frame 发来加密数据
    15. 客户端确认 (每个 frame)
6. 上述过程结束 0.3 秒后客户端发起 DNS 查询；这个时间间隔相当长，表明客户端进行了运算量较大的解密过程
    - 查询的域名是 `mmbiz.qpic.tc.qq.com` 和 `isdspeed.qq.com`
    - 前者是存储图片的服务器，后者不清楚，貌似与检查登陆状态有关
    - 图片是通过 http 下载的

解密过程

1. 模拟微信与服务器的通信
2. 解密微信发出去的数据包和接收到的数据包
    - 然而这是不可能的：解密发出去的包需要服务器的私钥，而解密接收到的包需要客户端的私钥
3. 既然不可能破解数据包，那么就只有想办法破解 App 的源代码了
4. 或者用其它办法抓取文章，比如通过模拟点击


{% highlight bash %}

{% endhighlight %}
