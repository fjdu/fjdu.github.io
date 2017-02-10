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

P1. 量子力学里的波函数包含了量子态的全部信息。原则上，需要指数数量的信息来编码一个量子态。但实际上描述一个系统需要的信息往往远远小于相应的希尔伯特空间的最大容量。不多的量子纠缠以及不多的处于这种状态的系统使得用不多的经典计算资源求解多体薛定谔方程成为可能。

P2



