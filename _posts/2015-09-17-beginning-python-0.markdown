---
layout: post
title:  "Beginning Python --- Lesson 0"
date:   2015-09-17 Thu 13:28:12
categories: Python
---

This is written for beginners.

# 0. 程序的运行方式

<ul>
<li> 命令行交互 (REPL: Read-Eval-Print-Loop) </li>
<li> 脚本文件 </li>
{% highlight python linenos=table %}
    > python script.py
{% endhighlight %}
<li> iPython 以及 iPython Notebook </li>
</ul>

# 1. 简单的单行代码

{% highlight python linenos=table %}

# 在命令行交互式运行时，下面的print都不必写。

print 232345  # 整数
print 23.2345  # 浮点数
print 2.12E33  # 科学计数法
print 1.2 + 0.345j  # 复数
print "Hello World!"  # 字符串
print 1, 2.3, 4343, 'adwd3'  # 一下print多个

print 1 + 1  # 代码风格：运算符前后的空格
print 1.23 - 32.4345  # 减法
print 123456 * 789012312**3  # **表示指数运算
print "Hello " + "World!"  # 字符串拼接

print 1 == 1
print 1 == 2
print True == (1 == 1)
print False == (1 == 1)
print (1 < 2) and (1 == 1)
print (1 < 2) or (1 == 1)
print not (1 < 2)

print abs(-123.453)  # 绝对值
print max(4, 5, 2, 1, 6, 3)  # 求最大值
print min(4, 5, 2, 1, 6, 3)  # 求最小值

import math  # 更复杂的数学运算需要专门的包
print math.sin(3.14159)  # 正弦函数

# 或者
from math import *  # 导入数学函数库里的所有函数
print sin(3.14159)  # 正弦函数可以直接使用了
# 坏处：污染名字空间

print [0,1,2,3]  # List
print range(4)   # Also a list
print (0,1,2,3)  # Tuple
print {'email': 'xxyy@gmail.com', 'address': 'AA, MI'}  # Dictionary

{% endhighlight %}

# 2. 一个计算机语言一般包含哪些部分
<ol>
<li> 基本的语法结构
<ul>
<li> 变量 </li>
下划线或字母开头，后面跟任意多个下划线、字母、或数字
{% highlight python linenos=table %}
a = 1
print id(a), id(1)

srwe = 1.23
b = srwe
srwe_ = 1.23
print id(srwe), id(srwe_), id(b)

srwe = 1.2E33
srwe_ = 1.2E33
print id(srwe), id(srwe_), srwe == srwe_

_a = 1
_ab_2 = 'asda'

print id(1.23)
a = 1.23
print id(a), id(1.23)
b = a
print id(b)
b = 1.23
print id(b)
print id(1.23)

print id(1)
a = 1
print id(a), id(1)
b = a
print id(b)
b = 1
print id(b)
print id(1)
{% endhighlight %}
<li> Python使用基于缩进(也就是对齐)的语法结构; 用空格，不要用Tab! </li>
<li> 条件判断 </li>
{% highlight python linenos=table %}
a = 1; b = 2  # 分号只在把两个语句写在同一行的时候需要  
if a == b:  
    print 'Do something'  
elif a > b:
    print 'Do nothing'
else:
    print 'Nothing to do'
{% endhighlight %}
<li> 循环结构 </li>
{% highlight python linenos=table %}
a = [1,2,3]
for i in a:
    print i*i

for i in 'fjsjk2jri3':
    print i

i = 10
while i > 0:
    print i
    i = i - 1
{% endhighlight %}
<li> 函数 </li>
{% highlight python linenos=table %}
def f(x):
    return x * x * x
def g(x, y, z):
    return x * y * z
{% endhighlight %}
</ul>
</li>
<li> 基本数据结构
<ul>
<li> String </li>
{% highlight python linenos=table %}
print ''
print ""
print '' == ""
s = 'weew1%^$'
print len(s)
s = 'weew\n1%\n\n^$'
print s
s = r'weew\n1%\n\n^$'
print s
s = '你好！'
print s
{% endhighlight %}
<li> List </li>
{% highlight python linenos=table %}
[]
[1,2,3]
a = [1, 'aasd', 3.14]
print len(a)
print a
a.append('QQQ')
print a
a[0] = 233
print a
for i in a:
    print i
for i in xrange(len(a)):
    print a[i]
{% endhighlight %}
<li> Tuple </li>
{% highlight python linenos=table %}
()
(1,2,3)
a = (1, 'aasd', 3.14)
print a[0], a[1]
a[0] = 3  # Error!
{% endhighlight %}
<li> Dictionary </li>
{% highlight python linenos=table %}
{}
{1: 'a', 2: 'b'}
{(1,2): 'abab', (3,4): '2e22r', 5: '2342fe', 'aa': [1, 2, 3.5]}
d = {'email': 'xxyy@gmail.com', 'address': 'AA, MI'}
print d['email'], d['address']
{% endhighlight %}
<li> Set </li>
{% highlight python linenos=table %}
s = {1, 1, 2, 2, 'a', 'qqq', 3.14, 3.14}
print s
{% endhighlight %}
<li> Class </li>
{% highlight python linenos=table %}
class AA():
    def __init__(self):
        self.name = 'AA'
        print 'Class ', self.name, ' is created.'
    def show(self):
        print self.name
    def __del__(self):
        print 'Class ', self.name, ' is destructed.'

a = AA()
print a.name
del a
{% endhighlight %}
</ul>
</li>
<li> 标准库
<ol>
<li> math </li>
<li> os </li>
<li> glob </li>
<li> sys </li>
<li> time </li>
<li> re </li>
<li> itertools </li>
<li> functools </li>
<li> multiprocessing </li>
<li> subprocess </li>
<li> random </li>
</ol>
</li>
<li> 外部库
<ol>
<li> numpy </li>
<li> matplotlib </li>
<li> pandas </li>
<li> scikit-learn </li>
<li> PIL </li>
</ol>
</li>
<li> 高级语法结构
<ol>
<li> map, reduce, filter </li>
<li> decorator </li>
<li> metaclass </li>
<li> 垃圾回收 </li>
<li> 如何自己写library </li>
<li> 如何用其它语言(C, C++, Fortran)来扩展Python </li>
<li> 用Python处理Python自身的语法元素 </li>
</ol>
</li>
</ol>

# References

1. [The official tutorial](https://docs.python.org/3/tutorial/)
1. [A Byte of Python](http://www.swaroopch.com/notes/python/)
1. [Learn Python the Hard Way](http://learnpythonthehardway.org/book/)
1. [Dive Into Python](http://www.diveinto.org/python3/)
1. [Python Practice Book](http://anandology.com/python-practice-book/index.html)
1. [Five mini programming projects for the Python beginner](https://medium.com/learning-journalism-tech/five-mini-programming-projects-for-the-python-beginner-21492f6ce0f3)
1. [Think Python: How to Think Like a Computer Scientist](http://greenteapress.com/thinkpython/html/index.html)
1. [Learning Python for non-developers](http://www.mattmakai.com/learning-python-for-non-developers.html)
1. [Python教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)
1. [Python爬虫](http://www.zhihu.com/question/20899988/answer/24923424)
1. [Python Style Guide](https://www.python.org/dev/peps/pep-0008/)
1. [Python Topics Tree](http://www.visualizationportfolio.com/visualization/python-topics-tree/)
1. Lutz, Learning Python
1. Lutz, Programming Python
1. Chun, Core Python Programming
1. Chun, Core Python Applications Programming
1. Goodrich, Data structures and algorithms in Python
1. 陈儒, Python源码剖析
1. 雨痕, Python学习笔记
1. Bird, Natural Language Processing with Python

*Other related material*

1. [The Unix Philosophy](http://www.catb.org/~esr/writings/taoup/html/ch01s06.html#id2878742)
1. [How to Build a Blog](https://www.udacity.com/course/web-development--cs253)
1. [TCP-IP协议](http://www.code123.cc/category/tcp-ip)
1. [互联网协议入门](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
1. [HTTP协议](http://www.cnblogs.com/TankXiao/archive/2012/09/26/2695955.html)
