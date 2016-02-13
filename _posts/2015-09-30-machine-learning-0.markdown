---
layout: post
title:  "Machine Learning"
date:   2015-09-30 Wed 11:06:30
categories: machine learn
---

- 监督学习：提供的反馈包含正确答案
- 非监督学习：只给惩罚值
- 学习函数：连续情形叫回归，离散情形叫分类
- 支持向量机：容易上手


# Murphy

- Market basket analysis: 市场篮子分析
- Frequent itemset mining: association rules
- Joint density model

- Data mining: 强调模型的可解释型
- Machine learning：强调模型的准确性

- Parametric model: 参数个数固定
- Nonparametric model: 参数个数不固定
    - K-最近邻居
        - Memory-based learning
        - Instance-based learning
        - Voronoi tessellation: K = 1, 每个给定点对应一个区域，这个区域内的点与这个给定点的距离比与其它给定点的距离都小。
- 维度的诅咒
    - 当N趋于无穷时，KNN可以达到最佳理论精度的一半
    - KNN对高维数据不适用
        - 体积比与尺度比的关系：只占很小一部分体积，可能对应很大一部分尺度
    - 解决方法：引入inductive bias，先验偏好，参数化模型
- 线性回归
    - 把基函数换成非线性函数，就可得到SVM，神经网络，分类与回归树等模型。
- Logistic regression
    - Decision rule
    - Linearly separable
- Overfitting
    在KNN情形，如果K ＝ 1...
- Model selection
    - Data partition: training set and validation set
    - Cross validation
        - Round-robin fashion
        - Leave-one out cross validation
- No free lunch theorem
    - No universally best model
    - Wolpert 1996

- "Probability is nothing but common sense reduced to calculation."  Pierre Laplace 1812
- corr[X, Y] = 1 iff Y = aX + b
    - Correlation coefficient is not the slope of the regression line.
    - But the "normalized" one is.
    - Correlation coefficient = 0 does not imply independence!

- KL divergence: Kullback-Leibler divergence = relative entropy
    - KL(p|q) >= 0 with equality iff p = q.

- Discrete distribution with maximum entropy is the uniform distribution
    - Principle of insufficient reason

- Mutual information
    - I(X; Y) = KL(p(X, Y) | p(X)p(Y))
    - I(X; Y) >= 0 with equality iff p(X,Y) = p(X)p(Y)
    - I(X; Y) = H(X) - H(X|Y)
        - H(X|Y) = sum p(Y) H(X|Y)
    - Pointwise mutual information

- 连续随即变量的mutual information
    - 离散化？结果受到离散方法的影响。所以这个办法不好。
    - MIC: maximal information coefficient
        - 
