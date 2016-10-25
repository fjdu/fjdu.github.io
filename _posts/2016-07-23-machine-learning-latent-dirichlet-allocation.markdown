---
layout: post
title:  "机器学习算法概要：隐含狄利克雷分布"
date:   2016-07-23 Sat 18:56:14
categories: machine learning
---

# 名称

隐含狄利克雷分布，Latent Dirichlet Allocation，LDA

# 基本思想

文本包含一些主题，不同主题占据不同比重，而每个主题对应一种词频分布。现在的问题是，给定文本，如何从中反推出其中不同主题的比重，以及每个单词分属哪个主题？

LDA 是一种词袋模型，也就是说，它不考虑词语的先后关系和近邻关系，只考虑词语的频度。

看到这种方法的名字，最直接的问题就是：为什么是狄利克雷分布？

## 共轭先验 (Conjugate prior)

如果后验分布与先验分布属于同类函数，则先验分布与后验分布被称为共轭分布，而先验分布被称为似然函数的共轭先验。

<p>
具体地说，就是给定贝叶斯公式
\[
    p(\theta | x) = \frac{p(x | \theta) p(\theta)} {\int p(x|\theta') p(\theta') d\theta'},
\]
假定似然函数 \(p(x | \theta)\) 是已知的，问题就是，选取什么样的先验分布 \(p(\theta)\) 能让后验分布与先验分布具有相同的数学形式。
</p>

<p>
共轭先验的好处主要在于代数上的方便性，可以直接给出后验分布的封闭形式，否则的话只能数值计算。共轭先验也有助于获得关于似然函数如何更新先验分布的直观印象。
</p>

<p>
所有指数家族的分布都有共轭先验。
</p>

<p>
以 beta 分布为例，其分布函数是
\[
    f(q; \alpha,\beta) = \frac{q^{\alpha-1} (1-q)^{\beta-1}}{B(\alpha,\beta)}.
\]
对于二项分布形式的似然函数，可得 beta 先验分布的后验分布是
\[
\begin{split}
    p(q | x) &= \frac{\binom{n}{x} q^x (1-q)^{n-x} f(q; \alpha, \beta)}
    {\int_0^1 dq \binom{n}{x} q^x (1-q)^{n-x} f(q; \alpha, \beta)} \\
    &= \frac{\binom{n}{x} q^x (1-q)^{n-x} {q^{\alpha-1} (1-q)^{\beta-1}}/{B(\alpha,\beta)}}
    {\int_0^1 dq \binom{n}{x} q^x (1-q)^{n-x} {q^{\alpha-1} (1-q)^{\beta-1}}/{B(\alpha,\beta)}}\\
    &= \frac{q^x (1-q)^{n-x} {q^{\alpha-1} (1-q)^{\beta-1}}}
    {\int_0^1 dq \; q^x (1-q)^{n-x} {q^{\alpha-1} (1-q)^{\beta-1}}}\\
    &= f(q; \alpha+x, \beta+n-x)
\end{split},
\]
也就是说，对于一枚硬币，假定其出现正面的几率是 \(q\)，且 \(q\) 满足参数为 \((\alpha,\beta)\) 的先验 beta 分布，那么，扔 \(n\) 次硬币出现 \(x\) 次正面后，我们把对 \(q\) 的认识更新为 \(B(\alpha+x, \beta+n-x)\) 的分布。
</p>

<p>
从硬币的例子我们也可看到上面提到的共轭先验在“直观印象”方面的优势：由于后验分布和先验分布函数形式一样，我们可以直观地把观测导致的从先验到后验的参数更新理解为一个逐步调节的过程。
在 beta 分布中，\(\alpha\) 相对 \(\beta\) 越大，则 \(q\) 越倾向于取较大的值。在后验分布参数中，正面次数 \(x\) 越大，则 \(\alpha\) 越大，的确是符合直觉的。基于这种直觉，我们可以把先验分布 \(B(\alpha,\beta)\) 理解为“虚拟观测”中出现 \(\alpha-1\) 次正面和 \(\beta-1\) (或 \((\alpha,\beta)\)，如果采用平均值作为估计的话) 次反面对应的状态。有意思的是，通常直觉对应的正面反面各一半的情况，意味着扔无穷多次并且正反面次数一样；这不奇怪，因为我们的这种直觉认识实际上是来自过去的经验，包含近乎无穷多次实验。所以，如果要采用 \(q = 1/2\) 作为先验分布，则必须令 \(\alpha = \infty,\ \beta=\infty\)，于是，有限多次的后续实验是不可能改变这个分布的，于是 \(q\) 会一直等于 \(1/2\)。换句话说，在采用 beta 分布作为先验分布的前提下，如果一开始认为正反面几率相等，那做有限次实验不会改变这个信念，不管实验结果如何。
</p>

<p>
进一步推广是采用多个共轭先验的凸性组合 (相当于先从一些分布里按一定几率取出一个，然后再以这个分布采样) 作为先验分布；这被称为超先验，也就是说超参数本身也满足一定的分布。
</p>

### 常见似然函数的共轭先验分布
<table>
<tr><th>似然函数  </th><th>  共轭先验</th></tr>
<tr><td>二项分布  </td><td>  beta 分布</td></tr>
<tr><td>柏松分布  </td><td>  Gamma 分布</td></tr>
<tr><td>类型分布  </td><td>  狄利克雷分布</td></tr>
<tr><td>多项分布  </td><td>  狄利克雷分布</td></tr>
<tr><td>超几何分布</td><td>  beta-binomial</td></tr>
<tr><td>几何分布  </td><td>  beta</td></tr>
<tr><td>正态分布  </td><td>  正态分布</td></tr>
<tr><td>正态分布  </td><td>  逆 Gamma 分布</td></tr>
<tr><td>正态分布  </td><td>  Scaled inverse chi-squared</td></tr>
<tr><td>均匀分布  </td><td>  Pareto 分布</td></tr>
<tr><td>lognormal </td><td>  normal</td></tr>
<tr><td>lognormal </td><td>  Gamma</td></tr>
<tr><td>指数分布  </td><td>  Gamma</td></tr>
<tr><td>Gamma     </td><td>  Gamma</td></tr>
</table>

参考：[Conjugate prior](https://en.wikipedia.org/wiki/Conjugate_prior)

## 多项分布 (Multinomial distribution)

<p>
是二项分布的推广，相当于从扔只有正面反面这两种可能结果的硬币推广到扔有多个面的骰子。
</p>

<p>
多项分布的分布函数是
\[
\begin{split}
    &p(X_1 = x_1, X_2 = x_2, \ldots, X_k = x_k; n, p_1, p_2, \ldots, p_k) \\
    = & \begin{cases}
    \frac{n!}{x_1! x_2!\cdots x_k!} p_1^{x_1} p_2^{x_2} \cdots p_k^{x_k},\ \text{if}\ \sum_{i=1}^k x_i = n \\
    0,\ \text{otherwise}
    \end{cases}.
\end{split}
\]
这里 \(x_{1,\ldots,k}\ge 0\)，\(0 \le p_{1,\ldots,k} \le 1\)，且 \(\sum_{i=1}^k p_i = 1\)。
</p>

<p>
注意到
\[
\begin{split}
    &\binom{n}{x_1} \binom{n-x_1}{x_2} \cdots \binom{n-x_1-x_2-\cdots-x_{k-1}}{x_k}\\
    = & \frac{n!}{x_1!(n-x_1)!} \frac{(n-x_1)!}{x_2!(n-x_1-x_2)!}\cdots \frac{(n-x_1-x_2-\cdots-x_{k-1})!}{x_k!(n-x_1-x_2-\cdots-x_k)!}\\
    = & \frac{n!}{x_1!x_2!\cdots x_k!0!} = \frac{n!}{x_1!x_2!\cdots x_k!},
\end{split}
\]
所以上面的概率分布函数表示的是扔 \(n\) 次有 \(k\) 个面的骰子，出现 \(x_1\) 次 1，\(x_2\) 次 2，...，\(x_k\) 个 \(k\) 的几率。\(p_{1,\ldots,k}\) 是每个面出现的几率。
</p>

<p>
这个概率分布函数的确是归一化的，因为
\[
\begin{split}
    & 1 = \left(p_1 + p_2 + \cdots + p_k\right)^n \\
    = & \sum_{x_1 =1}^n\binom{n}{x_1} p_1^{x_1} \left(p_2 + \cdots + p_k\right)^{n-x_1} \\
    = & \sum_{x_1 =1}^n\binom{n}{x_1} p_1^{x_1} \sum_{x_2 =1}^{n-x_1}\binom{n-x_1}{x_2} p_2^{x_2} \left(p_3 + \cdots + p_k\right)^{n-x_1-x_2} \\
    = & \sum_{\substack{x_1,x_2,\ldots,x_k\\ \sum_{i=1}^k x_i=n}}\binom{n}{x_1}\binom{n-x_1}{x_2}\cdots\binom{n-x_1-x_2-\cdots-x_{k-2}}{x_{k-1}}p_1^{x_1}p_2^{x_2}\cdots p_{k-1}^{x_{k-1}}p_k^{x_k},
\end{split}
\]
上面用到了
\[
    \binom{n-x_1-x_2-\cdots-x_{k-1}}{x_{k}} = \binom{x_k}{x_k} = 1.
\]
</p>

参考：[Multinomial distribution](https://en.wikipedia.org/wiki/Multinomial_distribution)

## 类型分布

<p>
也称为广义伯努利分布，是多项分布的特殊情形 (相当于 \(n=1\) 的情况)；出现每种结果的概率是
\[
    f(x=i|\mathbf{p}) = p_i,
\]
这里 \(x\) 的取值范围是 \(1\) 到 \(k\)。
当然，概率必须归一化，所以 \(\sum_{i=1}^k p_i = 1\)。

也可以把这个分布写成如下形式
\[
    f(x|\mathbf{p}) = \prod_{i=1}^k p_i^{\delta_{x,i}},
\]
这里 \(\delta_{x,i}\) <code>= 1 if x == i otherwise 0</code> 是 Kronecker delta 函数。

<br>
参考 <a href="https://en.wikipedia.org/wiki/Categorical_distribution">Categorical distribution</a>。
</p>

## 狄利克雷分布

<p>
如上面的表所示，狄利克雷分布是类型分布和多项分布的共轭先验。其分布函数长这样
\[
    f(x_1, \ldots,x_K; \alpha_1,\ldots,\alpha_K) = \frac{1}{B(\vec{\alpha})}\prod_{i=1}^K x_i^{\alpha_i-1}
\]
</p>

# 算法实现

# 软件支持

1. [scikit learn](http://scikit-learn.org/stable/modules/generated/sklearn.decomposition.LatentDirichletAllocation.html)
1. [lda 1.0.4](https://pypi.python.org/pypi/lda)
1. [参考实现](https://rstudio-pubs-static.s3.amazonaws.com/79360_850b2a69980c4488b1db95987a24867a.html)
