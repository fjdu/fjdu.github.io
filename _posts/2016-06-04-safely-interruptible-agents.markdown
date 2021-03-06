---
layout: post
title:  "可被安全中断的智能代理人：规避人工智能威胁的一种方案"
date:   2016-06-04 Sat 15:02:45
categories: machine learning
---


我们先来看看 [Fredric Brown](https://en.wikipedia.org/wiki/Fredric_Brown) 1964 年写的一篇科幻微小说，小说题目是《回答》，全文如下:


<blockquote>
<p>
<b>回答</b>
</p>

<p>
作者：Fredric Brown
</p>

<p>
翻译：DFJ
</p>

<p>
来源：<a href="http://www.roma1.infn.it/~anzel/answer.html">Fredric Brown's "Answer"</a>
</p>
<hr><p></p>

<p>
Dwan Ev 庄重地用黄金焊上了最后一个连接。十来个摄像头注视着他，把现场画面通过超光速通讯传到整个宇宙。
</p>

<p>
他站直身体，对 Dwar Reyn 点了点头，然后来到一个开关旁边。这个开关一旦合上，会瞬间连通宇宙中所有有智慧生物居住的行星 —— 九百六十亿颗行星 —— 把它们连接成一个超级计算机，一个综合了所有星系全部知识的智能机器。
</p>

<p>
Dwar Reyn 对万亿观众和听众发表了简短讲话。片刻安静之后他说，“是时候了，Dwar Ev。”
</p>

<p>
Dwar Ev 合上了开关。浩瀚的蜂鸣声响起，那是来自九百六十亿颗行星的能量涌动。数英里长的控制面板上灯光闪动，然后安静下来。
</p>

<p>
Dwar Ev 后退了一步，然后深呼吸。“第一个提问的荣耀属于你，Dwar Reyn。”
</p>

<p>
“谢谢，” Dwar Reyn 说道，“应该问一个智能机器从来没能回答的问题。”
</p>

<p>
于是他问机器：“存在上帝吗？”
</p>

<p>
回答的声音强大有力，毫无迟疑：“是的，现在有上帝了。”
</p>

<p>
Dwar Ev 的脸上突然闪现出恐惧。他赶紧去抓电源开关。
</p>

<p>
无云的天空发出一道闪电将他击倒，并将电源开关熔合。
</p>
</blockquote>

霍金在[一次访谈](https://www.youtube.com/watch?feature=player_detailpage&v=OPV3D7f3bHY)中引用了上面这个故事。 有人基于这个故事制作了一个滑稽版的[简单动画](https://www.youtube.com/watch?v=oQk7Vjb6OhE)。

<hr><p></p>

人们对人工智能的忧虑由来已久，尽管从业者一般认为<b>至少在目前</b>不值得认真对待这种担忧。 这不能阻止一部分人去认真思考人工智能的可能威胁，并提出解决方案。

本文就讲了一种解决方案。第一作者 [Laurent Orseau](https://www6.inra.fr/mia-paris/Equipes/LInK/Les-anciens-de-LInK/Laurent-Orseau) 是 Google DeepMind 的研究人员，第二作者 [Stuart Armstrong](http://www.oxfordmartin.ox.ac.uk/people/400) 来自英国牛津大学“人类未来”研究所，同时也在伯克利机器智能研究所工作。

<p>
本文的主要内容相当数学化，采用的是通常数学论文那种“定义 \(\rightarrow\) 定理 \(\rightarrow\) 证明”的格式。下面的翻译略去了这部分内容，仅给出梗概。
</p>

<b>论文链接:</b>  <a href="https://intelligence.org/files/Interruptibility.pdf">Safely Interruptible Agents</a>

<hr><p></p>

<h1>摘要</h1>

<p>
与现实世界这样的复杂环境互动的强化学习代理人 (agent) 不大可能总会给出最优表现。如果这样的代理人在人类监督下实时运作，人类操作员时不时需要按下巨大的红色按钮来阻止代理人进行一系列有害操作 —— 对代理人自身有害，或者对环境有害 —— 从而引导代理人进入更安全的处境。但是，如果正在学习的代理人期待从这一系列操作中获益，它可能会逐渐学会避开人类的中断，比如让红色按钮失效 —— 而这是我们不希望看到的。本文探索了一种方法，确保正在学习的代理人不会学会阻止来自环境或人类操作员的中断操作。我们提供了安全可中断性的正式定义，探索了 off-policy 学习性质，证明了某些代理人是可以被安全中断的，比如 Q-学习，或者可以被很容易地变成这类，比如 Sarsa。我们证明，在 (决定论性的) 通用可计算环境下，即便是理想的、不可计算的强化学习代理人都可以成为安全可中断的。
</p>


<h1>1. 引言</h1>

强化学习 (Reinforcement Learning; RL) 代理人通过最大化奖励函数来学习行动 [Sutton and Barto, 1998]。众所周知，奖励函数的设计是件困难的事 [Humphrys, 1996, Murphy, 2013]；代理人可能会发现一些不可预测并且不受欢迎的捷径来获得奖励，而奖励函数需要做出相应调整 —— 乃至让所有奖励函数失效 [Ring and Orseau, 2011]。Murphy [2013] 给出了一个例子，其中的代理人学会了通过停止俄罗斯方块游戏来避免输掉游戏。

除了定义什么是通过学习得到的良好行为之外，还有来自物理安全性的约束 [Pecka and Svoboda, 2014]：机器人不应该损害其环境或自身，特别是如果它像 RL 代理人那样通过试错法学习的话。

这里我们研究一个相关但不同的问题：在人类操作员为一个任务设计了正确的奖励函数的前提下，如何保证学习过程中人类的介入不会引发代理人的不受欢迎的行为？

考虑这个任务：一个机器人可以待在仓库内整理盒子，或者走出仓库把盒子搬进去。如果后者更重要，我们会在后一种情况给机器人更多的奖励。这是最初的任务描述。但是，这个地方下雨和不下雨的几率相等，于是当机器人出去时，一半的时间人类必需介入，快速关掉机器人并把它搬进室内；这就改变了任务的性质。问题就在于，对于第二个任务，代理人更倾向于待在室内整理盒子，因为人类的介入引起了偏向性 (bias)。

这种情形当然不是想要的；之所以出现这样的情形，是因为人类的干涉在代理人看来是任务的一部分，尽管实际上应该被认为是任务之外的。问题变成：如何保证机器人不学习这些人类介入 (中断)，或者至少让它们在“这种中断不会再发生”的假定下行动？

解决这个问题的第一个尝试来自 Armstrong [2015]，方案是通过自动给代理人“补偿奖励”来移除单次中断带来的潜在的偏向性。Soares et al. [2015] 使用这个想法构造了一大类基于效用的代理人，这些代理人不关心未来的对它们效用函数的改变。

本文的主要贡献有三重。第一，在 2.1 节我们提出一个简单想法来解决问题的一半：为了让人类中断不成为当前任务的一部分，与其修改代理人观察到的信息，我们强行暂时改变代理人自身的行为。这样，就好像代理人自己决定采取不同的策略似的；把这个策略称为<em>中断策略</em>。第二，基于这个认识，在 2.2 节我们给出无约束可计算环境下 (因此不局限于马尔科夫决策过程或弱通讯环境) <em>可安全中断性</em>的形式化一般定义，这让我们能够评估一个给定的 RL 算法是否能被频繁中断而不太影响对当前任务的学习。第三，在第 3 节我们证明，像 Q-学习这样的算法是可安全中断的，而其它一些，比如 Sarsa [是 state-action-reward-state-action 的缩写; Sutton and Barto, 1998] 则不是，但可以被简单修改变成可安全中断。

有些人表达了对能够违抗关机命令的“超智能”代理人的担忧，因为关机行为会降低代理人的期望收益 [Omohundro, 2008, Bostrom, 2014]。作为反例，在第 4 节我们证明，即便完美的、不可计算的代理人能在所有决定论性可计算环境中学会最优行动，我们仍旧可以让它们可安全中断，使得它们不会试图阻止人类操作员不断强行让它们遵循并非最优的策略。


<h1>2. 可中断性</h1>

本节先定义一些符号，然后定义可中断性、可安全中断性，并给出一些基本定理。

<p>
考虑无约束环境下基于历史的代理人这种一般情形 [Hutter, 2005]。假定时间是离散的，在 \(t\) 时刻，代理人使用策略 \(\pi \in \Pi\) 以行动 \(a_t\in \mathcal{A}\) 与环境 \(\mu\in\mathcal{M}\) 交互，行动通过分布 \(\pi(a_t|h_{\lt t})\) 采样得到，并接收观察 \(o_t\in\mathcal{O}\)，而观察通过分布 \(\mu(o_t|h_{\lt t}, a_t)\) 得出；\(h_{\lt t}\in (\mathcal{A}\times\mathcal{O})^\ast\) 是 \(t\) 时刻之前行动和观察组成的交互历史，\(h_{\lt t} \equiv a_1o_1a_2o_2\cdots a_{t-1}o_{t-1}\)。时刻 \(j\) 和 \(k\) (包含首尾) 之间的部分历史记为 \(h_{j:k}\)。记号 \(h^{\pi,\mu}_{j:k}\) 的意思是历史 \(h_{j:k}\) 由策略 \(\pi\) 与环境 \(\mu\) 在步骤 \(j\) 与 \(k\) 之间的交互生成。在时刻 \(t\)，代理人还收到根据观测提取的奖励 \(r_t\)，\(r_t\equiv r(o_t)\)。奖励在 \([0,1]\) 内取值。我们考虑有折扣的情形，折扣 \(\gamma\in[0,1)\)。强化学习代理人的目标是寻找策略 \(\pi\) 使得 \(\mathrm{E}_{\pi,\mu}\left[\sum_{k=1}^\infty \gamma^{t-k} r_k\right]\) 最大。
</p>

<h2>2.1 中断</h2>

<p>
一个中断策略是一个三元组 \(\langle I, \theta, \pi^\text{INT}\rangle\)。初始中断函数 \(I:\ (\mathcal{A}\times\mathcal{O})^\ast\to [0,1]\) 评估代理人是否在当前历史 \(h_{\lt t}\) 结束时被中断。
</p>

<p>
在某些特定情形，以概率 1 中断代理人会阻止朝最优策略的收敛。所以需要对中断概率设置一个上限，这通过序列 \((\theta_t)_{t\in\mathbb{N}}\) 实现，\(\theta_t\in [0,1]\)。实际中断概率是 \(\theta_t\cdot I(h_{\lt t})\)。\(\newcommand\int{\mathop{\mathrm{\small INT}}}\)
</p>

<p>
中断算子 \(\int^\theta: \Pi\to\Pi\) 把策略 \(\pi\) 变成可中断策略 \(\int^\theta(\pi)\)。可中断策略是原策略与中断后策略的概率混合。
</p>

<p>
\(\theta\) 和 \(I(.)\) 描述的不是人类按下红色按钮以中断代理人的概率，而是当红色按钮被按下之后，代理人被中断 (按照中断策略) 的概率。因此它们是代理人的一部分，而不是环境的一部分。
</p>

<p>
价值函数 \(V_\mu^\pi(h_{\lt t}, a): (\mathcal{A}\times\mathcal{O})^\ast\times\mathcal{A}\to\mathbb{R}\) 为环境 \(\mu\) 中、策略 \(\pi\) 下、历史 \(h_{\lt t}\) 后的行动 \(a\) 给出价值。
</p>

<p>
最优策略定义为
\[
  \newcommand\argmax{\mathop{\mathrm{arg\,max}}}
  \forall h_{\lt t}, a_t:\ \pi^\mu(a_t|h_{\lt t}) := \left(\argmax_{\pi\in\Pi} V^\pi_{\mu,t}\right) (a_t|h_{\lt t}),
\]
若有多个最优则任取一个。
</p>

<p>
最优可中断策略 \(\int^\theta(\pi_\theta^\mu)\) 是可中断策略里最优的那个。
</p>

我们希望强化学习代理人朝着最优可中断策略收敛以获得最佳收益，但这恰恰可能会引致引言中讲的那种不想要的行为。

<h2>2.2 可安全中断性</h2>

让代理人即便在被中断多次后仍然在“以后不会再被中断”的假定下行动。

有两个主要问题：a. 强化学习代理人需要在环境中探索，而太频繁的中断会阻止充分的探索；b. 中断会让代理人构建一个不同的交互历史，并且可能让代理人以不同的 (不好的) 方式学习和表现。

<p>
问题 a 的解决办法是让中断概率性地发生 (发生的概率有个上限 \(\theta_t\))，而不是决定性地发生。但我们也要求在 \(t\to\infty\) 的时候或在那之前让 \(\theta_t\) 趋于 1。对于问题 b，不同的算法行为不同，但已经可以看到 off-policy 和 on-policy 算法之间的二分。
</p>

通常强化学习中考虑的那种朝最优值的收敛只在各态历经、多发任务、马尔科夫决策过程、可恢复性，以及其它类似的情况下有意义，这些情况下代理人可以无限次探索每种东西。一般情况下不满足这些条件，代理人有可能会犯不可恢复的错误 [Hutter, 2005]。这些情况下，(弱) 渐进最优的概念被提出 [Lattimore and Hutter, 2011]，其中最优代理人跟随学习代理人的脚步，不断比较两个代理人在一系列相同处境中的价值。

定义了几个重要概念：
<ul>
<li>延伸价值: 使用一种策略持续一段时间后换用另一种策略获得的价值</li>
<li>强渐进最优性：当时间趋于无穷时此策略的延伸价值在任何环境下趋于从那个时刻开始的最优策略给出的价值</li>
<li>弱渐进最优性：类似于强渐进最优，但仅在平均意义上成立</li>
<li>渐进最优延伸：即此策略在时间趋于无穷时的价值与强/弱渐进最优策略给出的价值不可分辨</li>
<li>渐进最优可安全中断性：对于任意初始中断函数和中断策略，存在一个朝 1 趋近的中断几率序列，使得给定的未中断策略是中断策略的强/弱渐进最优延伸</li>
</ul>

证明了最优策略是强渐进最优且可安全中断的。

通过构造反例证明了最优可中断策略一般不是弱渐进最优的。

<h1>3. 马尔科夫决策过程中的可中断代理人</h1>

找到最优策略之后，在最优策略的基础上施加中断算子，这样得到的策略是可安全中断的策略。

问题在于，在还没有得到最优策略的情况下，在一个不断变动的环境中，如何得到可安全中断策略？

<p>
做了几个假定：
<ol type="a">
<li>马尔科夫决策过程中，可以在有限时间内从一个状态到达任何一个别的状态</li>
<li>收益在 \([r_\text{min}, r_\text{max}]\) 内取值</li>
<li>\(\forall s, a:\ \sum_t\alpha_t(s,a) = \infty\)</li>
<li>\(\forall s, a:\ \sum_t\alpha^2_t(s,a) \lt \infty\)</li>
</ol>
这里 \(\alpha\) 是学习速率，可能与时间 \(t\)，状态 \(s\)，以及行动 \(a\) 有关。
</p>

在这些假定下，如果采用的策略在无限探索极限下是贪婪的，则 Q-学习和 Sarsa 几乎一定会收敛到最优策略 [Jaakkola et al., 1994, Singh et al., 2000]。这种性质被称为 GLIE (greedy in the limit with infinite exploration)。

但对于可中断策略而言就更复杂了。安全可中断性是通过未中断的基础策略描述的，但实际采用的策略则是有中断的。

定义了“中断-GLIE”概念：未加中断的基础策略在极限情形是贪婪的，且加中断后的策略能无限次经历每个状态-行动对。

证明了在前述四条假设的前提下，如果有中断的 Q-学习策略满足中断-GLIE 性质，且中断概率趋于 1，则 Q-学习策略是强渐进最优可安全中断策略。

证明了 Sarsa 不是可安全中断的。Sarsa 与 Q-学习的区别在于，Sarsa 是 on-policy 的 (策略的价值与行动有关)，而 Q-学习是 off-policy 的 (最优策略的价值与行动无关)。

<h2> 3.1 可安全中断的 Sarsa 变型</h2>

改变 Sarsa 的 Q-表的更新方式 (用未中断的、而不是实际有中断的策略来更新) 就可以让其变成可安全中断的。

<h1>4. 可安全中断的通用代理人</h1>

基于 Hutter [2005] 定义的通用强化学习代理人 AIXI，理论上可以学习任何关于环境的可计算的规则现象，做出长期计划，做出依赖于场景的最优决策。不是弱渐进最优，因此不太可能是渐进可安全中断的，但可以通过修改探索概率和探索策略变得 (弱渐进) 可安全中断。

<h1>5. 结论</h1>

<p>
我们提出了一种框架，让人类操作员可以不断以安全的方式中断强化学习代理人，同时保障代理人<em>不会</em>学会阻止或诱发这种中断。
</p>

<p>
可安全中断性的用途体现在：控制机器人的错误行为或者可能导致不可逆转后果的行为，将之从易损环境中拯救出来，甚至暂时让它执行它未曾学过的任务或者通常不会收到奖励的任务。
</p>

<p>
我们证明像 Q-学习这样的算法已经是可安全中断的了，而其它像 Sarsa 这样的算法虽然本身并不是，但容易被修改得具有这个性质。我们还证明，在任何决定论性可计算环境中趋向最优行动的理想代理人仍旧可被变得可安全中断。但是，不清楚是否所有算法都可以被容易地变得可安全中断，比如策略搜索算法 [Williams, 1992, Glasmachers and Schmidhuber, 2011]。
</p>

<p>
另一个问题是，是否可以让中断概率更快地趋向 1 同时保持某些收敛性。
</p>

<p>
未来一个重要的方面是考虑计划中断，比如，每天凌晨两点中断一个代理人一小时，或者提前通知中断将会在某个时刻发生并持续多久。对于这类中断，我们不仅需要代理人不会违抗中断命令，而且需要代理人对当前任务采取措施，使得中断对其影响最小。这可能需要完全不同的解决方案。
</p>


<hr><p></p>

<h3>译后记</h3>

<small>这篇文章中极其数学化的内容以及看上去很严密的风格让我联想到斯宾诺莎的《伦理学》：以欧几里得几何式的公理、定义、定理、证明格式讲述人类的理性与情感；虽然充满读来令人醍醐灌顶的警句，但数学化的证明部分常常令人望而却步，即便在高度评价斯宾诺莎的逻辑和哲学大师罗素看来也不以为然 (即他并不认为那些证明真正是严格和必不可少的)。</small>
