---
layout: post
title:  "Okapi BM25, TF-IDF, 以及 ElasticSearch/Lucene 搜索结果的分数"
date:   2017-03-16 Thu 21:53:20
categories: coding
---

# Okapi BM25

*来源*: [https://en.wikipedia.org/wiki/Okapi_BM25](https://en.wikipedia.org/wiki/Okapi_BM25)

[Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25) 是搜索引擎用来对匹配文档进行排序的函数，依据是每个文档与搜索词的相关度。BM 是 Best Matching (最佳匹配) 的缩写。这个方法是上世纪七八十年代在概率搜索的框架下被提出的。Okapi 是第一个使用这种方法的信息获取系统的名称。

BM25 是基于词频的方法；也就是说，它不考虑多个搜索词在文档里是不是靠近，只考虑它们各自的出现次数。BM25 不是单个函数：许多函数都可以叫 BM25，彼此之间有些形式和参数个数的差异。最常用的形式之一是

<p>
\[
\text{score}(D,Q) = \sum_{i=1}^n\text{IDF}(q_i)\cdot\left[\frac{f(q_i,D)\cdot\left(k_1+1\right)}{f(q_i,D) + k_1\cdot\left(1-b+b\cdot\frac{|D|}{\text{avgdl}}\right)}\right],
\]
其中各符号含义如下：
<ul>
<li>\(D\): 文档</li>
<li>\(Q\): 搜索词 (多个)</li>
<li>\(f(q_i,D)\): \(q_i\) 这个词在文档 \(D\) 中的出现次数</li>
<li>\(|D|\): \(D\) 的单词数</li>
<li>\(\text{avgdl}\): 整个文档库中文档的平均长度</li>
<li>\(k_1\), \(b\): 自由参数，一般取值范围是 \(k_1\in[1.2,2.0]\), \(b=0.75\)</li>
<li>\(\text{IDF}(q_i)\): inverse document frequency，通常由下述公式计算
\[
  \text{IDF}(q_i) = \log\left(\frac{N-n(q_i)+0.5}{n(q_i) + 0.5}\right),
\]
这里 \(N\) 是文档库里总的文档数，\(n(q_i)\) 是包含单词 \(q_i\) 的文档个数。一个单词的 \(\text{IDF}\) 大，意味着这个单词只在较少文档中出现，也就意味着这个单词比较独特。<br>
这里 \(\text{IDF}\) 的定义有个问题，那就是，如果一个词在超过半数的文档里出现，则其 \(\text{IDF}\) 是负数，于是这个词对 BM25 分数的贡献是负的。一般不希望这样的特性，所以当 \(\text{IDF}\) 为负数时可强行改为 0，或者一个比较小的正数，或者改用一种平滑过渡到 0 的函数形式。
</li>
</ul>
</p>

可以看到，如果搜索词中包含比较独特的词，则会提升分数；搜索词在一个文档中出现的次数越高，分数也会更高；但文档越长，分数会越低。

<p>
BM25 的一个较新升级版叫 BM25F，把文档结构和锚文本也考虑进来。另一个叫 BM25+，只是在上面公式中的方括号里加了一个 \(\delta\)，用来弥补原来公式对超长文档的不公。
</p>

# TF-IDF

*来源*: [https://en.wikipedia.org/wiki/Tf%E2%80%93idf](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)

思想类似，不过是针对单个搜索词的：
<p>
\[
  \text{tfidf} = (\text{term frequency})\cdot\text{IDF}.
\]
</p>

# ElasticSearch/Lucene 的分数计算

*来源*: [https://www.elastic.co/guide/en/elasticsearch/guide/current/practical-scoring-function.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/practical-scoring-function.html), 以及 [https://www.elastic.co/guide/en/elasticsearch/guide/current/scoring-theory.html](https://www.elastic.co/guide/en/elasticsearch/guide/current/scoring-theory.html)  这个文档不一定最新的，不过这会儿我没找到最新的。

ElasticSearch 底层采用了 Lucene，而 Lucene 的分数计算综合了布尔模型 (Boolean model), TF-IDF, 以及矢量空间模型。

## 布尔模型

布尔模型使用 `AND`, `OR`, `NOT` 这三个逻辑运算符根据搜索词对文档进行筛选，只保留选中的文档，并不计算分数 (或者说选中的分数都是 1，没选中的都是 0)。

## TF

<p>
TF-IDF 前面已经提过，不过按照 ElasticSearch 的文档，他们采用的 \(\text{tf}\) 函数形式是
\[\text{tf}(t, d) = \sqrt{\text{t 在 d 内的出现次数}}。\]
</p>

在索引阶段如果把一个字段的索引类型 (`index_options`) 设为 `docs`，则搜索阶段这个字段不会被用来计算出现频率，而只会被用来筛选是否在一个文档中出现。

## IDF
<p>
他们使用的 \(\text{idf}\) 的函数形式是
\[
  \text{idf}(t) = 1 + \log\left(\text{文档总个数}/(1+\text{包含 t 的文档个数})\right)。
\]
</p>

## 字段长度归一化

<p>
字段包含的内容越长，权重越低。比如，一般说来出现在标题里的词会比出现在正文内容里的词更重要 (不过考虑到现在流行的标题党，似乎这个说法不太对)。
计算字段长度归一化因子的公式是
\[
  \text{norm}(d) = 1 / \sqrt{\text{字段包含的单词数}}。
\]
这种归一化可以通过设置去掉。
</p>

<p>
TF, IDF, 字段长度归一化因子，这三个东西都是在索引阶段计算好的。对于单个词语的搜索，只使用了这些因子。但对于多个词语的搜索，则还结合了下面的方法。
</p>

## 矢量空间模型

其实就是余弦相似度，根据两个矢量的内积来计算，与最开始计算 BM25 的公式类似。

## Lucene 实际实用的公式

<p>
\[
\begin{split}
\text{score}(q, d) =\;& \text{queryNorm}(q) \cdot \text{coord}(q,d)  \cdot\\
& \sum_{t \in q} \text{tf}(t, d) \cdot \text{idf}(t)^2 \cdot t.\text{getBoost}() \cdot \text{norm}(t,d)
\end{split}
,\]
其中，
<ul>
<li>\(\text{queryNorm}(q,d)\) 是搜索归一化因子，目的是希望不同搜索的结果可以互相比较，但实际上在这点上做得不好，不要太看重这个。它的数值一般通过 \(1/\sqrt{\text{各搜索词的 IDF 的平方和}}\) 计算。</li>
<li>\(\text{coord}(q,d)\) 用来奖励那些包含较多搜索词的文档；也就是说，在把匹配搜索词的权重加起来后，再乘以匹配的搜索词的个数，然后除以此次搜索词的总个数。</li>
<li>\(t.\text{getBoost}()\) 是额外的提权因子。</li>
<li>\(\text{norm}(t,d)\) 是字段长度归一化因子。</li>
</ul>
</p>
