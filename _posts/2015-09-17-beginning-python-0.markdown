---
layout: post
title:  "Beginning Python --- Lesson 0"
date:   2015-09-17 Thu 13:28:12
categories: Python
---

# 1. 简单的单行代码

{% highlight python linenos=table %}

# 在命令行交互式运行时，下面的print都不必写。

print 232345  # 整数
print 23.2345  # 浮点数
print 2.12E33  # 科学计数法
print "Hello World!"  # 字符串

print 1 + 1  # 代码风格：运算符前后的空格
print 1.23 - 32.4345  # 减法
print 123456 * 789012312**3  # **表示指数运算
print "Hello " + "World!"  # 字符串拼接

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
下划线或字母开头，后面跟下划线、字母、或数字
{% highlight python linenos=table %}
a = 1
_a = 1
_ab_2 = 'asda'
{% endhighlight %}
<li> Python使用基于锁进(也就是对齐)的语法结构 </li>
<li> 条件 </li>
{% highlight python linenos=table %}
a = 1; b = 2  # 分号只在把两个语句写在同一行的时候需要  
if a == b:  
    print 'Do something'  
{% endhighlight %}
<li> 循环 </li>
{% highlight python linenos=table %}
a = [1,2,3]
for i in a:
    print i*i
{% endhighlight %}
<li> 函数 </li>
{% highlight python linenos=table %}
def f(x):
    return x * x * x
{% endhighlight %}
</ul>
</li>
<li> 基本的数据结构
<ul>
<li> List </li>
{% highlight python linenos=table %}
[]
[1,2,3]
[1, 'aasd', 3.14]
{% endhighlight %}
<li> Tuple </li>
{% highlight python linenos=table %}
()
(1,2,3)
(1, 'aasd', 3.14)
{% endhighlight %}
<li> Dictionary </li>
{% highlight python linenos=table %}
{}
{1: 'a', 2: 'b'}
{(1,2): 'abab', (3,4): '2e22r', 5: '2342fe', 'aa': [1, 2, 3.5]}
{'email': 'xxyy@gmail.com', 'address': 'AA, MI'}
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
{% endhighlight %}
</ul>
</li>
<li> 标准库
</li>
<li> 外部库
</li>
</ol>
