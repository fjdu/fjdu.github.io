---
layout: post
title:  "随记：Python 里的一些小东西"
date:   2016-07-13 Wed 00:05:19
categories: python
---

# 使用系统自带的随机数源
{% highlight python linenos=table %}
import os
os.urandom(10)
# 使用操作系统的随机数源产生 10 个随机字节
# urandom 不是 os 定义的，而是内置模块 posix 定义的
# 也可以通过 cat /dev/urandom 获取随机字节
# 不过这会淹没终端
# 当然 head /dev/urandom 也行
# 而 tail /dev/urandom 当然是不行的，会一直等待下去
{% endhighlight %}

# 使用 inspect 获得模块/函数/对象的一些信息 (比如源文件内容)

# 安装 dlib 时遇到的一些问题

`import dlib` 时提示加载 `libpng` 以及 `libmkl_rt` 的动态链接库出错。

解决办法简单而坑爹：

`export DYLD_LIBRARY_PATH="~/anaconda/envs/opencv/lib:~/anaconda/lib"`

要在 `ipython` 下正确使用，需要在 `~/anaconda/envs/opencv/lib` 目录下建立一个软链接：`"libmkl_rt.dylib" -> "../../../lib/libmkl_rt.dylib"`

