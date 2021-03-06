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
    <li> 病态条件 (ill-conditioning)
      <ul>
        <li> 参见第四章</li>
        <li> 优化凸函数时也会遇到</li>
        <li> 最常见的是 Hessian 矩阵的病态</li>
        <li> 导致 SGD “卡住”</li>
        <li> 二阶项高于一阶项意味着出现了病态</li>
        <li>监控 \(g^\trsps g\) 和 \(g^\trsps H g\)
          <ul>
            <li>许多情形下，梯度的模在整个学习期间并不显著变小，而 \(g^\trsps H g\) 却增长超过一个数量级</li>
            <li>结果是，虽然梯度很大，但由于曲率更大，所以学习速度很慢</li>
          </ul>
          </li>
      </ul>
      </li>
    <li> 其它场景下处理病态条件的方法不一定适用，比如牛顿法就需要大改才能用</li>
    <li> 局部极值
      <ul>
        <li>凸优化问题等价于寻找局部极小的问题</li>
        <li>神经网络几乎一定会有多个极小值，但这不一定是个大问题</li>
        <li>模型识别问题 (model identifiability)
          <ul>
            <li>如果只要训练数据足够大，就能够唯一确定模型参数，则称为可识别</li>
            <li>包含隐变量的模型不可识别，因为把隐变量交换一下模型还是等价的
            <ul>
              <li>权重空间对称性：对于神经网络，把每层各单元的权重矢量做排列，不会影响结果。如果有 \(m\) 层，每层 \(n\) 个单元，则一共有 \(n!^m\) 种等价的排列方式。</li>
              <li>输入和输出的权重和偏置的放缩不变形：极小值对应 \((m\times n)\) 维双曲面</li>
              <li>无限维的局部极小：并不会导致问题</li>
            </ul>
            </li>
          <li>怕的是取值比全局极小大很多的局部极小<ul><li>对于小神经网络，可以构造出这种来，甚至都没有隐含层</li></ul></li>
          <li>对于实际的神经网络，不清楚是否存在许多比全局极小大许多的局部极小；许多研究者认为这不是个大问题</li>
          <li>通过梯度模来判断是否是局部极小导致了问题
            <ul>
              <li>如果梯度没有变得很小，则一定不是局部极小导致的问题</li>
              <li>如果梯度变得很小，也不一定是局部极小导致的问题</li>
              </ul></li>
          </ul>
        </li>
      </ul>
      </li>
    <li> 平台，鞍点，以及其它平坦区域
      <ul>
        <li>鞍点比局部极小常见多了: 局部极小和局部极大的交叉</li>
        <li>低维：局部极小常见；高维：鞍点常见，鞍点与局部极小的数量之比按维度 \(n\) 的指数增长，这是因为局部极小要求 Hessian 矩阵的所有本征值都是正数</li>
        <li>许多随机函数的 Hessian 矩阵的本征值为正的概率在到达低 cost 区域时变大，这意味着局部极小拥有小 cost 比拥有大 cost 的可能性较大；拥有大 cost 的临界点更有可能是鞍点；拥有极高 cost 的临界点更有可能是局部极大。</li>
        <li>神经网络是否也这样？浅 auto-encoder 不包含 cost 高于全局极小的局部极小，相当于多个矩阵的复合</li>
        <li>一阶梯度下降方法：似乎容易逃离鞍点</li>
        <li>牛顿法：被设计用来寻找临界点，所以受到鞍点的影响较大; saddle-free 方法可以解决这个问题，不过对于大网络还不能 scale</li>
        <li>宽平区域带来的问题更大</li>
      </ul>
    </li>
    <li> 悬崖和梯度爆炸
      <ul>
        <li>太大的梯度会把参数推得太远，让之前的优化白做了</li>
        <li>可以通过 gradient clipping heuristic 避免</li>
      </ul>
    </li>
    <li> 长期依赖
      <ul>
        <li>当计算图太深时就有问题了：本征值的指数放大和缩小，导致梯度的指数放大和缩小</li>
        <li>循环神经网络面临这样的问题，但前馈网络基本上没这个问题</li>
      </ul>
    </li>
    <li> 不准确的梯度</li>
    <li>局部结构和全局结构不匹配
      <ul>
        <li>指向局部极小的方向没有指向全局极小的方向</li>
        <li>训练时间取决于训练路径的长度</li>
        <li>有可能不存在全局极小: 如果精确匹配训练数据，则会无限逼近但永远达不到最优值 (最优值可能是负无穷)</li>
        <li>寻找良好的起始点</li>
      </ul>
    </li>
    <li> 优化的极限
      <ul>
        <li>相关的理论对神经网络的实际应用影响不大</li>
        <li>有些理论结果仅对神经网络输出离散值的情况适用</li>
      </ul>
    </li>
  </ul>
</p>

## 8.3 基本算法

<ul>
  <li>
    随机梯度下降 (随机 minibatch)
  </li>
  <li>
    冲量
    <ul>
      <li>
        计算梯度 \(g\)，更新速度 \(v \leftarrow \alpha v - \epsilon g\)
      </li>
      <li>
        \(\theta \leftarrow \theta + v\)
      </li>
    </ul>
  </li>
  <li>
    Nesterov 冲量
  </li>
</ul>

## 8.4 参数初始化策略

## 8.5 自适应学习率算法

## 8.6 近似的二阶方法

<ul>
  <li>
    牛顿法
    \[
      \theta^\ast = \theta_0 - H^{-1} \nabla_\theta J(\theta_0)
    \]
  </li>
  <li>
    共轭梯度法
    <ul>
      <li>
        通过迭代下降对偶方向的办法来避免求 Hessian 矩阵的逆
      </li>
      <li>
        后一步的搜索方向必定垂直于前一步的搜索方向:
        最陡下降法中，令前一步搜索方向为 \(d_{t-1}\)，在极小值处，沿着这个方向的导数是 0: \(\nabla_\theta J(\theta) \cdot d_{t-1} = 0\)，而下一步的方向是 \(\nabla_\theta J(\theta)\)，故如此。之字形搜索路线的效率不高。
      </li>
      <li>
        令 \(d_t = \nabla_\theta J(\theta) + \beta_t d_{t-1}\)，就是共轭梯度法。\(\beta_t\) 控制上一个方向对下一个方向的贡献比例。
      </li>
      <li>
        如果 \(d_t^\trsps H d_{t-1} = 0\)，则称两个方向是共轭的。令 \(g_t = \nabla_\theta J(\theta_t)\)，有两种常见的计算 \(\beta_t\) 的方法
        <ol>
          <li> Fletcher-Reeves
            \[
              \beta_t = \frac{g_t^\trsps g_t}{g_{t-1}^\trsps g_{t-1}}
            \]
          </li>
          <li> Polak-Ribière
            \[
              \beta_t = \frac{(g_t - g_{t-1})^\trsps g_t}{g_{t-1}^\trsps g_{t-1}}
            \]
          </li>
        </ol>
      </li>
      <li>
        对于二次曲面，共轭方向保证了沿着之前方向的梯度不增加
      </li>
      <li>
        对于 k 维参数空间，只需要最多 k 次线搜索就可以到达最小值。
      </li>
      <li>
        非线性共轭梯度法: 在共轭梯度的基础上，偶尔进行普通的最陡梯度下降
      </li>
    </ul>
  </li>
  <li>
    BFGS (Broyden–Fletcher–Goldfarb–Shanno)
    <ul>
      <li>
        牛顿法的难点在于求 Hessian 矩阵的逆；准牛顿方法用近似方法来求逆；BFGS 方法就是一种准牛顿法。
      </li>
      <li>
        通过迭代求得 \(H^{-1}\) 的近似 \(M_t\)，由此得到搜索方向 \(\rho_t = M_t g_t\)，然后通过线搜索得到步长。
      </li>
      <li>
        对线搜索的精度要求不高
      </li>
      <li>
        需要存储 Hessian 矩阵的逆，对于大模型做不到
      </li>
      <li>
        Limited memory BFGS: 不保存上一步的 \(H^{-1}\)，而假定它是单位矩阵；或者保存其一部分，存储占用为 \(O(n)\)。
      </li>
    </ul>
  </li>
</ul>

## 8.7 优化策略和元算法

<ul>
  <li>
    批量归一化 (batch normalization)
  </li>
  <li>
    坐标下降 (coordinate decent)
  </li>
  <li>
    Polyak 平均
  </li>
  <li>
    监督预训练
  </li>
  <li>
    设计模型来辅助优化
  </li>
  <li>
    持续方法 (continuation methods) 和课程学习
  </li>
</ul>


