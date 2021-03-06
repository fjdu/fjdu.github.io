---
layout: post
title:  "用人工神经网络解决量子多体问题"
date:   2017-02-10 Fri 13:54:26
categories: algorithm
---

_译自 [Giuseppe Carleo](http://www.itp.phys.ethz.ch/people/person-detail.html?persid=221305) and [Matthias Troyer](http://www.comp.phys.ethz.ch/people/troyer.html) 2017, [Solving the quantum many-body problem with artificial neural networks](http://science.sciencemag.org/content/355/6325/602)_

预印本可在[这里](https://arxiv.org/abs/1606.02318)看到。

---

<br>

# 摘要

量子多体问题的困难来自多体波函数的指数复杂性带来的非平凡关联。本文作者通过把机器学习的方法应用于波函数来降低其复杂度；对某些物理上有意思的情形，让波函数的形式简化到计算上可行。基于可变数量隐藏神经元的人工神经网络模型，作者引入了量子态的变分表示。一个基于强化学习的框架能找到基态，并能描述复杂相互作用量子系统的幺正演化。作者提出的方法在描述一维和二维自旋相互作用典型模型方面达到了较高精度。

# 正文

量子力学里的波函数包含了量子态的全部信息。原则上，需要指数数量的信息来编码一个量子态。但实际上描述一个系统需要的信息往往远远小于相应的希尔伯特空间的最大容量。不多的量子纠缠以及不多的处于这种状态的系统使得用不多的经典计算资源求解多体薛定谔方程成为可能。

数值方法要么对有限多个物理上有意义的位形进行采样，要么对量子态进行高效率的压缩。量子蒙特卡洛方法属于第一类，通常需要波函数半正定。压缩方法依赖于波函数的高效表示，比如量子态的矩阵积或张量积之类的。但对于高维系统现在的方法还是很成问题的，比如我们还不太了解高维系统的动力学性质，或者强相互作用费米子系统的基态的精确性质。问题的核心在于如何系统性地削减完整的多体波函数的指数增长的复杂性而只留下必要的特征。

放在更广的场景，我们面对的实质上是个维度缩减和特征提取问题，而人工神经网络是这方面举足轻重的方法。之前神经网络已被应用到一些物理系统，不过主要是关于物质的复杂相的分类，而在这些情形对那些相的精确采样是可能的。对于没有关于样本的先验知识的情形之前还没有探索但很有价值。

<p>
这里我们引入波函数的一种人工神经网络表示，对应的参数集合是 \(\mathcal{W}\)。我们给出一种针对参数集 \(\mathcal{W}\) 的强化学习随机框架，用来获取量子哈密顿量 \(\mathcal{H}\) 对应的基态和含时态的最佳表示。神经网络的参数通过定态变分蒙特卡洛 (VMC) 或者含时 VMC 方法训练获得。这个方法的正确性通过应用于一维和二维的 Ising 模型和海森堡模型验证。神经网络量子态 (NQS) 的能力在基态和非平衡态动力学中得到了证明。
</p>

### 神经网络量子态

<p>
把波函数看成黑盒。作者讨论了限制性玻尔兹曼机 (RBM) 架构并应用于自旋\(1/2\)的量子系统。这种情形，RBM 神经网络由包含 N 个节点的可见层 (对应于给定基下的自旋变量) 和包含 M 个辅助自旋变量的隐藏层组成:
\[
    \Psi_M(\mathcal{S}; \mathcal{W}) = \sum_{\{h_i\}} e^{\sum_j a_j\sigma_j^z + \sum_i b_i h_i + \sum_{ij} W_{ij} h_j\sigma_j^z}
\]
这里 \(h_i=\{-1,1\}\)，而网络参数 \(\mathcal{W} = \{a,b,W\}\) 完全描述了网络对给定输入态 \(\mathcal{S}\) 的响应。
</p>

### 基态

### 幺正动力学

