---
layout: post
title:  "Logistic map and logistic function"
date:   2016-02-07 Sun 02:56:15
categories: algorithm
---

# [Logistic 函数](https://en.wikipedia.org/wiki/Logistic_function)

<p>
<ul>
<li> 函数形式 \[f(x) = \frac{L}{1 + e^{-k(x-x_0)}}\]</li>
<li> 满足如下微分方程 \[\frac{df}{dx} = f (1 - f)\]</li>
<li> 生态学：种群数量增长 \[\frac{dP}{dt} = r P \cdot \left(1-\frac{P}{K}\right),\]</li>
<li> 解为 \[P(t) = \frac{K P_0 e^{rt}}{K + P_0(e^{rt}-1)}\]</li>
<li> 与化学中自催化反应和物理学中费米-狄拉克分布的关系</li>
<li> Logistic 映射 \[x_{n+1} = r x_n (1-x_n)\]</li>
<li> 反函数 logit: </li>
  <ul>
  <li> 函数形式 \[\mathrm{logit}(p) = \log\left(\frac{p}{1-p}\right)\]</li>
  <li> Probit 函数是下述函数的反函数<
    \[\Psi(x) = \frac{1}{\sqrt{2\pi}} \int^x_{-\infty} e^{-y^2/2}dy\]</li>
  </ul>
</ul>
</p>
