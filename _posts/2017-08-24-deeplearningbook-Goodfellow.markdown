---
layout: post
title:  "Goodfellow et al.《深度学习》"
date:   2017-08-24 Thu 10:59:26
categories: math
---

<div hidden>
\(
\newcommand\trsps{\mathsf{T}}
\)
</div>

# [8 深度模型的训练](http://www.deeplearningbook.org/contents/optimization.html)

## 8.1 学习的优化不同于普通的优化

- 经验风险最小化
  - 直接的学习目标做不到直接优化, 只能优化一个间接目标
  - 避免过拟合
  - 某些损失函数不可导，比如 0-1 loss
- 代理损失函数和提早停止
  - 对于 0-1 loss，经常用 log-likelihood 替代
  - 替代损失函数可以比原损失函数更 robust
- Batch 和 minibatch 算法
  - 目标函数往往是对训练样本的求和
  - 把全部训练样本都考虑进来的话，计算量太大
  - 随机取出少量样本进行平均
  - 算法的收敛往往不要求提供精确的梯度
  - 许多训练样本是冗余的，本来也没有提供多少信息
  - 使用全部样本的方法叫 batch 或者 deterministic
  - 每次随机取一个样本的方法叫 stochastic 或者 online 方法
    - Online 方法一般是用来描述训练样本来自数据流的情形
  - 每次随机取一部分样本的方法叫 minibatch 或者 minibatch stochastic 方法，或者就叫 stochastic 方法
    - Stochastic gradient descent (SGD)
  - Minibatch 的大小与下述因素有关
    - 较大的 batch 给出的梯度更准确，但计算量更大
    - 如果 batch 太小，多核架构的利用率不高，所以不能太小
    - 如果一个 batch 里的所有样本被并行处理，则内存占用正比于 batch 大小
    - 有些硬件架构对于大小为 2 的指数的 batch 效率较高
    - 小的 batch 自带正则化效果
      - 小 batch 会添加噪声，起到正则化的作用
      - 泛化误差在 batch size 为 1 时最好
      - 但 batch size 太小会导致运算量加大许多，因为较大的噪声要求较小的学习率
    - 不同方法使用 minibatch 的不同信息
      - 只依赖梯度的方法比较强壮
      - 依赖 Hessian 矩阵的二阶方法需要较大的 minibatch
      - 与条件数太大的矩阵的逆相乘导致误差放大
    - 对样本要随机化，避免采样过程里的相关性
    - 多个 minibatch 同步进行: 异步并行分布式计算
    - Minibatch stochastic 方法看上去像在线方法，所以可以最小化泛化误差
      - 仅当样本不被重复使用时这种解读才成立
      - 多个 epoch 能带来其它好处，所以可以接受
      - 样本数多则只用一个 epoch

## 8.2 神经网络优化的挑战

<p>
<ul>
<li> 传统的机器学习做法：仔细设计目标函数和约束条件来让优化问题是凸优化</li>
<li> 病态条件 (ill-conditioning)</li>
  <ul>
  <li> 优化凸函数时也会遇到</li>
  <li> 最常见的是 Hessian 矩阵的病态</li>
  <li> 导致 SGD “卡住”</li>
  <li> 二阶项高于一阶项意味着出现了病态</li>
  <li>监控 \(g^\trsps g\) 和 \(g^\trsps H g\)</li>
  </ul>
</ul>
</p>
