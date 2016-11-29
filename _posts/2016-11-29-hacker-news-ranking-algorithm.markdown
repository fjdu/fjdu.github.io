---
layout: post
title:  "Hacker News 的排序算法"
date:   2016-11-29 Tue 10:48:25
categories: algorithm
---

这是一篇编译文章。
在本文中我试图理解 Hacker News 的排序算法的工作机制，以及如何借鉴。这个算法很简单，但在需要突出热门话题和新东西的情形，据说效果好得令人吃惊。

>**注**：[Hacker News](https://news.ycombinator.com/) 是著名孵化器 [Y Combinator](https://www.ycombinator.com/) 旗下的一个带有社交性质的新闻网站，由 Paul Graham 于 2007 年创立 (Paul Graham 使用自己发明的编程语言 [Arc](http://arclanguage.org/) 做的)；主要内容是在计算机科学和创业方面，不过，“任何能满足智力好奇心”的东西都可以往上面发。

<p><hr/></p>


# 相关的源代码

Hacker News 是用 `Arc` 语言实现的。涉及到排序的那部分代码由 Paul Graham 在[这个帖子](https://news.ycombinator.com/item?id=1781013)里给出了：

{% highlight lisp linenos=table %}
(= gravity* 1.8 timebase* 120 front-threshold* 1
   nourl-factor* .4 lightweight-factor* .17 gag-factor* .1)

(def frontpage-rank (s (o scorefn realscore) (o gravity gravity*))
  (* (/ (let base (- (scorefn s) 1)
          (if (> base 0) (expt base .8) base))
        (expt (/ (+ (item-age s) timebase*) 60) gravity))
     (if (no (in s!type 'story 'poll))  .8
         (blank s!url)                  nourl-factor*
         (mem 'bury s!keys)             .001
                                        (* (contro-factor s)
                                           (if (mem 'gag s!keys)
                                                gag-factor*
                                               (lightweight s)
                                                lightweight-factor*
                                               1)))))
{% endhighlight %}

`Arc` 的语法比较奇怪，下面用 `Python` 改写一下：
{% highlight python linenos=table %}
gravity_ = 1.8
timebase_ = 120
front_threshold_ = 1
nourl_factor_ = 0.4
lightweight_factor_ = 0.17
gag_factor_ = 0.1

def frontpage_rank(s, scorefn=realscore, gravity=gravity_):
    base = scorefn(s) - 1
    if base > 0:
        val_1 = base**0.8
    else:
        val_1 = base

    val_2 = ((s.age + timebase_) / 60)**gravity
    val_3 = val_1 / val_2

    if s.type not in ['story', 'poll']:
        val_4 = 0.8
    elif s.url == '':
        val_4 = nourl_factor_
    elif 'bury' in s.keys:
        val_4 = 0.001
    else:
        if 'gag' in s.keys:
            val_5 = gag_factor_
        elif is_lightweight(s):
            val_5 = lightweight_factor_
        else:
            val_5 = 1
        val_4 = control_factor(s) * val_5

    score = val_3 * val_4
    return score
{% endhighlight %}

简单地说 (忽略一些特殊情况)，**排序使用的分数**用如下方式算出

<p>
\[
    分数 = V \times \left[\frac{(P - 1)^{0.8}}{(T + 2)^G}\right]
\]
</p>

各符号的意义如下：

- P: 一个条目的**点数** (跟点赞数有关，并且与点赞的人自己的分数有关，具体计算方法没有公开)
- T: 从提交时刻到现在的时间 (以小时计算)
- G: Gravity，重力 (这里跟地球的“重力”没关系)，默认取为 1.8
- V: 跟每个条目具体内容有关的控制指标，比如属于“故事”或“投票”类别的，分数较高，如果不属于这些类别且不包含链接，则分数会降低，如果包含“隐藏”标签，则分数会极低，等等。

### 重力 (G) 和 时间 (T) 的影响

- 随着时间 T 的增长，分数会下降，也就是说越旧的条目分数越低
- 重力越大，则旧条目的分数下降的速度越快

# 如何使用

- 点数 P 的设置
    - 暂时没有资讯的点赞机制，所以 P 或可取为点击数
- 初始点数
    - 保证让新条目处于第一位
    - 由于初始点数是动态设置的，所以需要定期整体重新归一化
- 控制指标
    - 消息类别
    - 消息重要性 (通过某些方法定义)
    - 后台可以通过调节 V 里的某些部分来调节排序
- 避免恶意刷榜
    - 比如，如果某条消息与某个机构有关，他们可能有动机通过某种方法让这条消息长期居于首位。需要避免这种情况的发生。
    - 解决办法：后台调节，或者对操纵点击数的行为进行控制 (比如限制来自同一 IP 的频繁点击)。

# 参考资料

1. Paul Graham 写的关于Hacker News 的若干思考，值得社交问答网站的运营者参考：
[http://paulgraham.com/hackernews.html](http://paulgraham.com/hackernews.html)
2. [Hacker News FAQ](https://news.ycombinator.com/newsfaq.html)
3. Hacker News 上的讨论: [https://news.ycombinator.com/item?id=1781013](https://news.ycombinator.com/item?id=1781013), [https://news.ycombinator.com/item?id=7809498](https://news.ycombinator.com/item?id=7809498)
4. [How Hacker News ranking algorithm works](https://medium.com/hacking-and-gonzo/how-hacker-news-ranking-algorithm-works-1d9b0cf2c08d)，作者是 [Amir Salihefendic](https://medium.com/@amix3k)
5. The StupidFilter Project: [http://stupidfilter.org/main/](http://stupidfilter.org/main/)
5. [Arc Documentation](http://arclanguage.github.io/ref/index.html)
