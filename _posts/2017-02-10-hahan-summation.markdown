---
layout: post
title:  "Kahan 求和算法"
date:   2017-02-10 Fri 15:56:48
categories: algorithm
---

[https://en.wikipedia.org/wiki/Kahan_summation_algorithm](https://en.wikipedia.org/wiki/Kahan_summation_algorithm)

**目的**：用于降低有限精度浮点数组成的数列的求和误差

<p>
<b>普通算法</b>：最坏情形误差正比于 \(n\) 增长，随机输入情形均方误差正比于 \(\sqrt{n}\)。
</p>

<p>
<b>补偿求和</b>：最坏情形误差随 \(n\) 的增长要慢许多。
</p>

### Python 代码

{% highlight python linenos=table %}
def kahanSum(vals):
    s = 0.0
    c = 0.0
    for v in vals:
        # y = v - err_of_previous_step
        y = v - c
        # t = s + v - err_of_previous_step + err_of_this_step
        t = s + y
        # c = err_of_this_step
        c = (t - s) - y
        # s = v0 + v1 + ε1 + v2 + ε2 - ε1 + v3 + ε3 - ε2 + ...
        #   = v0 + v1 + v2 + v3 + ... + vn + εn
        s = t
    return s
{% endhighlight %}

#### 试验
{% highlight python linenos=table %}
vals = [100000, 2.8, 2.7]
# 在只有六位有效数字的情况下，简单求和的过程是
s0 = 100000
s1 = 100002
s2 = 100004
# 使用补偿算法后的过程是
y0 = 100000
t0 = 100000
c0 = 0
s0 = 100000
y1 = 2.8
t1 = 100002
c1 = -0.8
s1 = 100002
y2 = 3.5
t2 = 100005
c2 = -0.5
s2 = 100005
# 这个当然更接近精确结果
{% endhighlight %}



### 误差界
<p>
对于求和
\[
    S_n = \sum_{i=1}^n x_i,
\]
其误差为
\[
    |E_n| \le \left[2\varepsilon + O(n\varepsilon^2)\right] \sum_{i=1}^n|x_i|.
\]
</p>

#### Condition number

<p>
定义为
\[
    \frac{\sum_{i=1}^n \left|x_i\right|}{\left|\sum_{i=1}^n x_i\right|}.
\]
</p>

<p>
给定条件数的情形，由于 \(n\) 通常远小于 \(1/\varepsilon\)，误差实质上与 \(n\) 无关。
</p>

计算**数列方差**时也应该避免这种舍入误差。

### 其它方法: 成对求和

[https://en.wikipedia.org/wiki/Pairwise_summation](https://en.wikipedia.org/wiki/Pairwise_summation)

**优点**：加法次数与普通的简单求和一样; 应用于快速傅立叶变换

<p>
<b>误差</b>: \(O(\varepsilon\sqrt{\log n})\) (假定舍入误差的正负号随机); 最坏情形 \(O(\varepsilon{\log n})\)
</p>

<p>
<b>直觉理解</b>: 为什么分治求和可以降低误差？
</p>

{% highlight python linenos=table %}
def pairwiseSum(vals):
    if len(vals) <= N:
        s = 0.0
        for v in vals:
            s += v
    else:
        m = floor(len(vals) / 2)
        s = pairwiseSum(vals[0:m]) + pairwiseSum(vals[m:])
    return s
{% endhighlight %}
