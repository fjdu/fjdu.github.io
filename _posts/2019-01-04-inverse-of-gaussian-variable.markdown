---
layout: post
title:  "随机变量的倒数的分布函数"
date:   2019-01-04 Fri 18:31:36
categories: math
---

<p>
给定正态分布随机变量\(x\)
\[p_X(x) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}},\]
定义\(y = 1/x\)，则\(y\)的分布函数为
\[p_Y(y) = p_X(x(y)) \frac{dx}{dy} = \frac{1}{y^2\sqrt{2\pi}\sigma} e^{-\frac{(1/y-\mu)^2}{2\sigma^2}}.\]
</p>


<p>
事实上，对于任何分布\(p_X(x)\)，只要当\(x\to 0\)时趋近于非零常数值，则当\(y\to\infty\)时,
\[p_Y(y) \propto y^{-2}.\]
注意到，对于这样的分布，期望值和方差都不存在，因为积分发散。
</p>
