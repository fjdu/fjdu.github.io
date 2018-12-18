---
layout: post
title:  "多次测量取平均时的权重选取"
date:   2018-12-18 Tue 12:04:52
categories: math
---

## 方差最小化

<p>
假设对随机变量\(X\)进行了\(n\)次测量，每次测量的数据质量不同，即每个测量值的方差不同 (但期望值相同)，通过取平均估计\(X\)的期望值的时候，应该采用加权平均。
</p>

<p>
加权平均的权重应该如何选取呢？
</p>

<p>
记
\[x=\sum_i^n w_i x_i,\]
可得\(x\)的方差为
\[\sigma^2(x) = \sum_i^n w_i^2\sigma^2(x_i) = \sum_i^n w_i^2\sigma^2_i.\]
</p>

<p>
作为加权平均，权重\(w_i\)需要满足条件
\[\sum_i^n w_i=1,\]
否则\(x\)的期望值就会不等于总体期望，成为有偏估计了。
</p>

<p>
我们希望通过权重的选取让\(x\)的方差尽可能小。通过拉格朗日乘子法，需要优化下述函数
\[s = \sum_i^n w_i^2\sigma^2_i - \lambda \left(\sum_i^n w_i-1\right).\]
对\(w_i\)求导，得
\[\frac{\partial s}{\partial w_i} = 2w_i\sigma_i^2 - \lambda = 0,\]
也就是要求
\[w_i = \frac{\lambda}{2\sigma_i^2},\]
注意到\(\lambda\)是个常数，所以权重应该反比于\(\sigma_i^2\)。
</p>

## 最大似然估计

<p>
也可以通过最大似然方法来估计。似然函数为
\[L = \prod_{i=1}^{n}\frac{1}{\sqrt{2\pi}\sigma_i} e^{-\frac{(x_i-\mu)^2}{2\sigma_i^2}}.\]
取对数后对\(\mu\)求导得
\[\frac{\partial\ln L}{\partial\mu} = \sum_{i=1}^n\frac{1}{\sigma_i^2}(\mu-x_i) = 0,\]
于是
\[\mu = \frac{\sum_{i=1}^n\frac{x_i}{\sigma_i^2}}{\sum_{i=1}^n\frac{1}{\sigma_i^2}},\]
也就是说对期望值的最大似然估计应该采用反比于方差的权重。
</p>

<hr><b>

<p>
也可以考虑另外一种非标准的“最大似然”。
注意到在假定了\(x_i\)是独立的高斯分布随机变量后，上面定义的\(x\)也满足形式为
\[p(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}\]
的高斯分布，且
\[\sigma^2 = \sum_i^n w_i^2\sigma_i^2.\]
这里的\(\mu\)是总体期望。现在要求\(p(x=\mu)\)最大，也就是要求\(\sigma\)最小，后面的步骤跟上面讲的一样。
</p>
