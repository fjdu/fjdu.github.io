---
layout: post
title:  "顶级大数据处理框架"
date:   2016-03-03 Thu 17:51:49
categories: big data
---

这是一篇翻译文章。

<h1>原文</h1>
[Matthew Mayo](http://www.kdnuggets.com/2016/03/top-big-data-processing-frameworks.html)

<section>
<h1> 摘要 </h1>
<p>
讨论了五个大数据处理框架：Hadoop，Spark，Flink，Storm，Samaza
</p>
<p>
如今大量数据不断产生，去纠结具体多大才算大数据没什么意义。 就像“人工智能”一样，大数据这个词的具体含义是变化的。几十年前对人工智能的期待大多已经实现，如今已经不管这部分叫“人工智能”了；同样，由于我们这个社会在不断地创造、保存、处理指数级增长的数据，今天的大数据在明天也就是“还不错哦”而已。不能用于大数据的传统数据处理方法会被淘汰。
</p>
<p>
所以，问题就是，我们要对这些数据做什么。答案当然依赖于具体场景。但每个人都在处理大数据，并且这种处理可以抽象到可以被各种大数据处理框架应对的程度。其中一些框架是周知的，比如Hadoop和Spark，另一些的使用则比较小众，但它们仍然获得了可观的市场份额和声誉。
</p>
<p>
我们这里来看顶级开源大数据处理框架中的五个。当然，在用的并不是只有这几个，但希望它们可作为可获取的那些里面一个小的代表性样本。
</p>
</section>

<section>
<h1> Hadoop </h1>
<p>
经典之作，如今顶尖的框架之一。几乎成了大数据的代名词。你已经知道了Hadoop，MapReduce，以及其生态系统和相关的技术，比如Pig，Hive，Flume，HDFS等。Hadoop是第一个，在工业界被广泛采用。
</p>
<p>
为什么仍然使用Hadoop。尽管Hadoop被用来处理复杂数据，其本身其实相当简单。如果你的数据可以批量处理，可以被分割成小的处理任务，分发到计算集群，然后综合计算结果，并且整个过程都逻辑清晰，那么你的数据很可能适合用Hadoop处理。
</p>
<p>
Hadoop生态系统里有些工具的用途超出了当初作为支持MapReduce算法的范畴。一个值得注意的就是YARN，Hadoop的资源管理层。它可以用在Hadoop之外，比如用于Spark。
</p>
</section>

<section>
<h1> Spark </h1>
<p>
Spark是大数据处理王国的王位继承人。Spark和Hadoop经常被放到二选一的位置，但并不是非要这样的。Hadoop的生态系统可以容纳Spark的处理引擎，用来替代MapReduce，由此产生各种由两个生态系统的工具混合起来的环境。一个具体的例子，大数据巨头Cloudera如今在把MapReduce替换为Spark。另一个例子，Spark不包含自身的分布式存储层，这样它可以利用Hadoop的分布式文件系统（HDFS），或跟Hadoop无关的，比如Mesos。
</p>
<p>
Spark跟Hadoop和MapReduced的不同在于，Spark是工作在内存里的，于是速度快很多。Spark也绕过了Hadoop的默认MapReduce引擎的线性数据流，于是更灵活的构建管道成为可能。
</p>
<p>
什么时候应该用Spark？如果你不想被MapReduce的范式套住，并且还没有建立起Hadoop的环境，或者基于内存的处理对处理时间提升明显，那就应该用Spark。另外，如果你对紧密集成的机器学习感兴趣，Spark的机器学习库MLib针对分布式建模利用了其架构。
</p>
<p>
当涉及到大数据处理时，Hadoop和Spark可能是主力，但不是仅有的选择。
</p>
</section>

<section>
<h1> Flink </h1>
<p>
数据流引擎。目标是提供针对数据流的计算工具。把批量处理作为数据流的特殊情形，Flink既是批处理又是实时处理框架，但流处理还是放在第一位的。
</p>
<p>
Flink提供了一系列API，包括针对Java和Scala的流API，针对Java，Scala，Python的静态数据API，以及在Java和Scala中嵌入SQL查询的代码。它也自带机器学习和图像处理包。Flink还有一系列额外特性：
<ul>
  <li> 高性能、低延迟 </li>
  <li> 支持有序和乱序事件 </li>
  <li> 针对有状态计算的仅限一次语义 </li>
  <li> Backpressure连续流模型 </li>
  <li> 通过轻量级分布式快照做容错 </li>
</ul>
</p>
<p>
选Flink而不选Spark？Flink真的是面向流的。Spark是批处理的，尽管它可以通过缩减每次处理的事件来模拟流的效果，但毕竟不如Flink。如果你要处理真正的实时数据，Spark是不行的，得用Flink。
</p>
</section>

<section>
<h1> Storm </h1>
<p>
分布式计算框架，其应用被设计成有向无环图。被设计成容易处理无限流，并且可用于任何编程语言。每个节点每秒处理上百万个元组，高度可伸缩，提供任务处理保证。用Clojure写的。
</p>
<p>
可用于实时分析，分布式机器学习，以及大量别的情形，特别是数据流大的。Storm可以运行在YARN上，集成到Hadoop生态系统中。五个特征：
<ul>
<li> 快 </li>
<li> 可伸缩 </li>
<li> 容错 </li>
<li> 可靠 </li>
<li> 易操作 </li>
</ul>
</p>
<p>
不支持批处理。不支持有状态的管理(可以用Trident)。
</p>
</section>

<section>
<h1> Samza </h1>
<p>
消息传递基于Apache Kafka, 集群资源管理基于YARN。特性：
<ul>
<li> 简单API </li>
<li> 状态管理 </li>
<li> 容错 </li>
<li> 可持续 </li>
<li> 可伸缩 </li>
<li> 可插拔 </li>
<li> 处理器隔离 </li>
</ul>
</p>
</section>
