---
layout: post
title:  "Word2vec 的未来发展"
date:   2016-06-19 Sun 20:15:44
categories: machine learning
---

作者：Tomas Mikolov

源地址：[http://www.machinelearning.ru/wiki/images/d/db/MikolovWord2vecSlides.pdf](http://www.machinelearning.ru/wiki/images/d/db/MikolovWord2vecSlides.pdf)

# 下一步工作

- 各种对 word2vec 的诠释
- 分布式稀疏表示
- 形态特征
- 处理一词多义现象
- 句子与文档的表示

# Word2vec 与分布式语义

- Word2vec 与之前的 (非神经网络的) 方法密切相关
- 比较上下文计数与上下文预测语义矢量 (Baroni et al, 2014)
    - 平均而言 word2vec 比分布式语义技术更好并且更强壮
- 作为隐式矩阵分解 (Levy & Goldberg 2014)
- Golve: Global Vetors for Word Representation (Pennington et al. 2014)
- Word2vec 使用的技巧可以被用于传统的分布式语义技术

# 若干争议

- Golve
- Socher: “Glove 11% better on word analogies than word2vec!!!”    
- Goldberg: "at least train the models on the same data ..."
- Glove 表现不如 word2vec, 并且后者速度更快，且内存消耗小得多；Levy (2005)

# 分布式稀疏表示

- Word2vec: 把 1-of-N 表示变成 D 维连续矢量
- 连续矢量可被转换回稀疏表示，构成 M-of-N 编码：适用于速度要求高的应用
- 可通过随机投影 + 量化或者 max() 函数实现
- 细节在 word2vec 论坛里

# 形态特征

- 许多作者讨论过
- 往输入/输出层添加更多特征来表达词语结构
- 对结构丰富的语言很有用
- 有助于构造训练中未见过的单词的表示

# 一词多义

Word2vec 论坛提到的一些想法

- 学习 word2vec 矢量
- 对每个词库中的单词，通过搜集其近邻单词矢量来获得其统计特征
- 对每个词库中的单词进行 k-means 聚类
- 用 k-means 的聚类中心和每个单词的上下文矢量来标注训练集
- 训练多义 word2vec 模型

# 句子、段落，以及文档的表示

- 基于 RNN 的方法
    - Sequence to sequence learning (Stutskever et al. 2014)
    - Skip-throught vectors (Kiros et al. 2015)
- 这些方法是否给出比加权 bag-of-ngrams 方法好的句子表示？经常不清楚
- RNN 真的需要吗？能否通过更简单和更快的方法获得更好的表示？这是未来研究的内容。
