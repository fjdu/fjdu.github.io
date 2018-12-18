---
layout: post
title:  "多次测量取平均时的权重选取"
date:   2018-12-18 Tue 12:04:52
categories: math
---

## 均值的方差最小化

<p>
假设对随机变量\(X\)进行了\(n\)次测量，每次测量的数据质量不同，即每个测量值的方差不同 (但期望值相同)，通过取平均估计\(X\)的期望值的时候，应该采用加权平均。
</p>

<p>
加权平均的权重应该如何选取呢？
</p>

<p>
记
\[x=\sum_{i=1}^n w_i x_i,\]
可得\(x\)的方差为
\[\sigma^2(x) = \sum_{i=1}^n w_i^2\sigma^2(x_i) \equiv \sum_{i=1}^n w_i^2\sigma^2_i.\]
</p>

<p>
作为加权平均，权重\(w_i\)需要满足条件
\[\sum_{i=1}^n w_i=1,\]
否则\(x\)的期望值就会不等于总体期望，成为有偏估计了。
</p>

<p>
我们希望通过权重的选取让\(x\)的方差尽可能小。通过拉格朗日乘子法，需要优化下述函数
\[s = \sum_{i=1}^n w_i^2\sigma^2_i - \lambda \left(\sum_{i=1}^n w_i-1\right).\]
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
\[\frac{\partial\ln\!L}{\partial\mu} = \sum_{i=1}^n\frac{1}{\sigma_i^2}(\mu-x_i) = 0,\]
于是
\[\mu = \frac{\sum_{i=1}^n\frac{x_i}{\sigma_i^2}}{\sum_{i=1}^n\frac{1}{\sigma_i^2}},\]
也就是说对期望值的最大似然估计应该采用反比于方差的权重。
</p>

<hr><br>

<p>
也可以考虑另外一种非标准的“最大似然”。
注意到在假定了\(x_i\)是独立的高斯分布随机变量后，上面定义的\(x\)也满足形式为
\[p(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}\]
的高斯分布，且
\[\sigma^2 = \sum_i^n w_i^2\sigma_i^2.\]
这里的\(\mu\)是总体期望。现在要求\(p(x=\mu)\)最大，也就是要求\(\sigma\)最小，后面的步骤跟上面讲的一样。
</p>

## 最小二乘拟合

<p>
对于一组观测数据\((x_i,y_i)\) (\(i=1,\ldots,n\))，希望用一个函数\(f(x;\theta)\)来拟合，其中\(\theta\)是待求参数。每个观测数据的测量误差可能不一样，所以做最小二乘的时候也涉及到加权的问题。与上面类似，似然函数为
\[L = \prod_{i=1}^{n}\frac{1}{\sqrt{2\pi}\sigma_i} e^{-\frac{1}{2\sigma_i^2} \left[y_i-f(x_i;\theta)\right]^2}.\]
让\(\ln\!L\)对\(\theta\)求导得到\(\theta\)的最大似然估计需要满足的方程
\[\sum_{i=1}^n \frac{1}{\sigma_i^2}\left[f(x_i;\theta)-y_i\right]\frac{\partial f(x_i;\theta)}{\partial \theta}=0.\]
</p>

<p>
对于最小二乘拟合，其目标函数是
\[s = \sum_{i=1}^n w_i\left[y_i - f(x_i;\theta)\right]^2.\]
让\(s\)对\(\theta\)求导得到极值条件
\[\sum_{i=1}^n w_i \left[y_i - f(x_i;\theta)\right]\frac{\partial f(x_i;\theta)}{\partial \theta}=0.\]
为了与最大似然方法的结果一致，可见使用最小二乘法时应该让权重反比于方差，即
\[w_i = \frac{1}{\sigma_i^2}.\]
</p>

<p>
取平均值相当于拟合一个常数函数，即\(f(x_i;\theta)=\theta\) (也就是之前的期望值\(\mu\))。容易验证由此得到的\(\theta\)的计算公式与之前一样。
</p>

## 卡方检验

<p>
我们知道，\(n\)个独立同分布的标准正态分布随机变量的平方和满足\(\chi^2(n)\)分布。所以如果每个随机变量虽然满足正态分布，但没有归一化，则需要先归一化再求平方和。归一化当然应该把原始数据除以标准差 (而不是方差；取平方和之后才是方差)。
</p>
