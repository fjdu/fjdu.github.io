---
layout: post
title:  "技术解构 Stack Overflow"
date:   2017-03-06 Mon 21:46:05
categories: coding
---

# 0. 技术解构

[来源](https://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)

作者说在 Stack Overflow 几乎所有事情都是<del>允许</del>鼓励公开讨论的 (除了与公司财务有关的少数方面)。作者希望通过分享达到下面的目的：

<ul>
  <li>让读者学到新的炫酷玩意儿</li>
  <li>找到自己做得不好的方面</li>
  <li>共同找到更好的途径</li>
  <li>消除“大公司总是做得比较好”的认识；他们也犯错</li>
</ul>

大家都会犯错。我们能做的是：生活，学习，向前看，然后下次做得好一点。

# 1. 架构 - 2016 版

[来源](https://nickcraver.com/blog/2016/02/17/stack-overflow-the-architecture-2016-edition/)

普通的一天 (以 2016 年 2 月 9 日为例；括号里的数字是与 2013 年 11 月 12 日的数据的差)：

- 209,420,973 (+61,336,090) 次 HTTP 负载均衡请求
- 66,294,789 (+30,199,477) 其中这么多次页面载入
- 1,240,266,346,053 (+406,273,363,426) bytes (1.24 TB) HTTP 发送的字节数
- 569,449,470,023 (+282,874,825,991) bytes (569 GB) 收到的字节数
- 3,084,303,599,266 (+1,958,311,041,954) bytes (3.08 TB) 总发送量
- 504,816,843 (+170,244,740) SQL Queries (仅限 HTTP 请求)
- 5,831,683,114 (+5,418,818,063) Redis hits
- 17,158,874 (not tracked in 2013) Elastic searches
- 3,661,134 (+57,716) Tag Engine requests
- 607,073,066 (+48,848,481) ms (168 hours) spent running SQL queries
- 10,396,073 (-88,950,843) ms (2.8 hours) spent on Redis hits
- 147,018,571 (+14,634,512) ms (40.8 hours) 标签引擎请求所花费的时间
- 1,609,944,301 (-1,118,232,744) ms (447 hours) ASP.Net 所花费的处理时间
- 22.71 (-5.29) ms average (19.12 ms in ASP.Net) for 49,180,275 question page renders
- 11.80 (-53.2) ms average (8.81 ms in ASP.Net) for 6,370,076 home page renders

其中 ASP.Net 处理时间的缩短是由于硬件升级和软件优化。

硬件配置 (括号里是相对 2013 年的变化)：

- 4 Microsoft SQL Servers (new hardware for 2 of them)
- 11 IIS Web Servers (new hardware)
- 2 Redis Servers (new hardware)
- 3 Tag Engine servers (new hardware for 2 of the 3)
- 3 Elasticsearch servers (same)
- 4 HAProxy Load Balancers (added 2 to support CloudFlare)
- 2 Networks (each a Nexus 5596 Core + 2232TM Fabric Extenders, upgraded to 10Gbps everywhere)
- 2 Fortinet 800C Firewalls (replaced Cisco 5525-X ASAs)
- 2 Cisco ASR-1001 Routers (replaced Cisco 3945 Routers)
- 2 Cisco ASR-1001-x Routers (new!)

最少需要几台服务器能跑 Stack Overflow? 自 2013 年以来变化不大。但由于硬件和软件的优化，最少一台就够了。我们曾经无意中测试过这个，并且不止一次。注意：我只是说这是可行的，没说这么做是对的，虽说每次都挺好玩。

<p><b>硬件外观</b><br>
<img src="https://i.imgur.com/TEb0jiPh.jpg" width='300px'></img><img src="https://i.imgur.com/ZFvRqgkh.jpg" width='300px'></img>
</p>
<p><b>架构图</b><br>
<img src="https://nickcraver.com/blog/content/SO-Architecture-Overview-Logical.svg" width='600px'></img>
</p>
<p><b>基本原则</b><br>
<ul>
  <li>每个东西都有冗余设计</li>
  <li>每个服务器和网络设备都有至少 2x 10Gbps 的带宽</li>
  <li>每个服务器有两个由 UPS 供电的电源，以及两个发电机和两个别的什么电源</li>
  <li>每个服务器有冗余伴侣</li>
  <li>每个服务器和服务在另一个数据中心 (科罗拉多) 有冗余备件；这里主要谈纽约的数据中心</li>
  <li>每个东西都有冗余设计</li>
</ul>
</p>
<p><b>联网</b><br>
采用了 CloudFlare 的 DNS 服务，通过 API 更新 DNS 记录。同时也做了自己的 DNS 服务器，在异常情况下使用。
<br>
采用 BGP 与 四个 ISP 连接。
<br>
</p>

两个数据中心之间有 10Gbps 带宽的 [MPLS](https://en.wikipedia.org/wiki/Multiprotocol_Label_Switching) 连接，用来进行数据复制和快速恢复。数据中心之间也通过 ISP 连接。

负载均衡采用的是 HAProxy 1.5.15 on CentOS 7。不少于 64 GB 的内存。

...

L1 缓存：服务器的 HTTP 缓存；L2 缓存：Redis；使用了 Protobuf 格式。

机器学习跑在两台服务器上，用来在首页推荐问题，以及工作匹配。

Redis 也用于发布和订阅，还用于 Websockets；文件句柄不够的问题。

...

搜索采用的是 Elasticsearch，StackExchange.Elastic 客户端。搜索、相关问题、提问建议，这些都用了 ES。

每个数据中心有一个 ES cluster，每个 ES cluster 有三个节点，每个站有自己的 index。求职部分有自己的一些 index。全部是 SSD 存储，192 GB 内存，10 Gbps 双模网络连接。

标签引擎也不断往 ES 里索引数据。

采用 ES 的原因是 scalability and better allocation of money。不用 SQL 全文搜索的原因是后者的 CPU 开销较大。不用 Solr 的原因是为了能同时搜索多个 index。不升级到 2.x 版本的原因是 ES 对 type 做了剧烈变动。

数据库采用 SQL Server，作为 single source of truth。所有 ES 和 Redis 的数据来自 SQL Server。两个 SQL Server cluster；主从，异步复制到另一个数据中心。第一个 cluster：每台服务器 384 GB 内存，4 TB SSD，2x12 cores；第二个 cluster：768 GB，6 TB，2x8 cores。CPU 占用率比较低。

对 SQL 的使用很简单。简单就快。新的开发采用了 Dapper。

# 硬件 - 2016 版

# 部署 - 2016 版
