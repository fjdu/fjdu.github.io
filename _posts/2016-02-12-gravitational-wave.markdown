---
layout: post
title:  "直接探测引力波"
date:   2016-02-12 Fri 16:22:55
categories: physics
---

<style>
h1 { 
    display: block;
    font-size: 2em;
    margin-top: 0.67em;
    margin-bottom: 0.67em;
    margin-left: 0;
    margin-right: 0;
    font-weight: bold;
}
h2 {
    display: block;
    font-size: 1.2em;
    margin-top: 0.83em;
    margin-bottom: 0.83em;
    margin-left: 0;
    margin-right: 0;
    font-weight: bold;
}
h3 { 
    display: block;
    font-size: 1em;
    margin-top: 1em;
    margin-bottom: 1em;
    margin-left: 0;
    margin-right: 0;
    font-weight: normal;
}
table {
    border-collapse: collapse;
    width: 100%;
}

th, td {
    padding: 8px;
    text-align: left;
    border-top: 1px solid #ddd;
    border-bottom: 1px solid #ddd;
}

tr:hover {background-color: #f5f5f5}
tr:nth-child(even){background-color: #f2f2f2}
</style>

<section>
<h1>原文中的一些基本信息</h1>

<table style="width:100%">
<tr> <td>探测到信号的时间</td> <td>2015年9月14日，09:50:45 UTC</td> </tr>
<tr> <td>峰值形变</td> <td>\(1.0\times10^{-21}\)</td> </tr>
<tr> <td>匹配滤波器</td> <td>信噪比24</td> </tr>
<tr> <td>假信号几率</td> <td>每203000年一次</td> </tr>
<tr> <td>信号源的光度距离</td> <td>\(410^{+160}_{-180}\) MPc</td> </tr>
<tr> <td>红移</td> <td>\(0.09^{+0.03}_{-0.04}\)</td> </tr>
<tr> <td>两个黑洞的初始质量</td> <td>\(36^{+5}_{-4}\) 和 \(29^{+4}_{-4}\) \(M_\odot\)</td> </tr>
<tr> <td>黑洞最终质量</td> <td>\(62^{+4}_{-4}\) \(M_\odot\)</td> </tr>
<tr> <td>以引力波形式辐射掉的能量</td> <td>\(3.0^{+0.5}_{-0.5}\) \(M_\odot\)</td> </tr>
</table>
</section>

<section><h1> 引力波是个什么东西</h1>
简单说，就是爱因斯坦场方程弱场近似下的行波解。这不是说强场情况下就没有引力波了，只不过，在强场情形，方程高度非线性，精确求解很困难。
<h3>源方程</h3>
\[
    \square^2 h_{\mu\nu} = -16\pi S_{\mu\nu},
\]
这个框框\(\square\)叫做达朗贝尔算符。物理学里的波动方程一般都长这个样。
\(h_{\mu\nu}\)是相对平直时空度规 (度规，就是时空的度量，用来计算两个时空点之间距离) 的小扰动，可以大致理解为两个自由漂浮物体之间距离在受到引力波作用后变化的比例。\(S_{\mu\nu}=T_{\mu\nu}-\frac{1}{2}\eta_{\mu\nu}T^\lambda_\lambda\), \(T_{\mu\nu}\)是物质场的能量动量张量，可以大致理解为物质的能量以及能量的流动率。 \(\eta_{\mu\nu}\)是平直时空度规，是些常数。从这个方程可以算出引力波的强度由哪些因素决定。
<h3>齐次方程</h3>
\[
    \square^2 h_{\mu\nu} = 0,
\]
\[
    \frac{\partial}{\partial x^\mu} h^\mu_\nu = \frac{1}{2} \frac{\partial}{\partial x^\nu} h^\mu_\nu.
\]
描述了真空中引力波的传播。
\(h_{\mu\nu}\)作为一个四维对称张量，表面上应该有十个独立分量。但广义相对论里有许多坐标选取的自由，这种由于“坐标自由”导致的自由度不是真正的自由度，没有可观测的物理效应。可以通过限制坐标的类型 (比如协和坐标条件之类的)，来限制物理量的自由度。加上各种坐标条件后，真正的独立自由度只有两个。这两个独立自由度在围绕传播方向旋转时螺旋量分别为\(\pm2\)，这也暗示引力量子化后，引力子的自旋是2。
<h3>转动物体的辐射功率</h3>
\[
P = \frac{32G\Omega^6 I^2 e^2}{5c^5},
\]
这里\(\Omega\)是自转角速度，\(I\)是物体的转动惯量，\(e\)是赤道椭率。
<h3>远场</h3>
<div>
\[
    h_{\mu\nu} = \frac{2G}{R c^4} \ddot{I}_{\mu\nu} = 1.6\times10^{-44} \frac{\ddot{I}_{\mu\nu}}{R}.
\]
其中的数值用的是SI单位。
</div>
</section>

<section><h1> 比全宇宙的星光还亮?</h1>
<div>
咱们来粗略估计一下这两个黑洞合并时发出的引力波的峰值功率吧。
已经知道质量损失为三个太阳质量，现在只需要一个时间尺度。既然是粗略计算，就取为两个黑洞紧贴着相互绕转的周期吧，可以用牛顿力学来算。
\[
    T^2 = \frac{4\pi^2}{G(m_1+m_2)} (r_1+r_2)^3,
\]
把\(r_1\)和\(r_2\)取为两个黑洞的史瓦西半径，\(r_\text{s} = \frac{2Gm}{c^2}\)，于是
\[
    T^2 = \frac{32\pi^2G^2}{c^6} (m_1+m_2)^2.
\]
代入数字，\(T\simeq0.006秒\)。注意到这个0.006秒相当于150赫兹，正好是论文中提到的峰值幅度对应的频率。
</div>
于是峰值功率是
\[
    3M_\odot c^2/T = 9 \times 10^{49} 瓦 = 2.3\times10^{23} L_\odot,
\]
也就是说，峰值功率相当于\(2.3\times10^{23}\)个太阳的亮度。\(10^{23}\)正好是可观测宇宙中恒星总数的量级 (参考<a href="https://www.zhihu.com/question/21341364/answer/18716795">这个链接</a>)。只是粗略估计，不必太当真。
</section>

<section><h1> 为什么引力波那么难以探测？</h1>
说好的比整个宇宙的星光还亮呢？
<ul>
<li>首先，这个东西距离实在太远了。即便同样大的功率以电磁波的形式传到地球，亮度大约也只有太阳的万分之一。这当然不算很暗，但也不像表面上“整个宇宙的光度”看起来那样夸张。</li>
<li>更重要的，是引力波与物质的作用方式与电磁波不同。在实验室参考系中，引力波对物质的作用可以理解为一种潮汐力。潮汐力的特点是，其大小正比于一个东西相对另一个东西的距离，以及这个东西自身的质量。所以需要大空间尺度才比较明显。或者换个说法，假定用两个同样面积的望远镜去接收源距离和总辐射功率都相同的电磁波和引力波，哪个接收到的能量多？对于电磁辐射，可以做到完全接收，但对于引力波，吸收的能量与望远镜面板区域内引力势的二阶梯度有关，而这个梯度往往是极小的 (因为引力波的波长非常长)，因此引力波几乎不被吸收。其实，如果电磁波也像引力波一样把能量集中在极长的波段，吸收率也会特别低。
</li>
</ul>
</section>

<section><h1>LIGO怎么能探测到那么细微的位移</h1>
<div>
那可是\(10^{-21}\)!  怎么能探测到宏观物体的比质子还小千倍的位移？
<ul>
<li>探测方法是看干涉条纹亮度的变化。一阶近似下正比于干涉臂长度的变化比例，也就正比于引力波的振幅。</li>
<li>简单说，就是尽量增加干涉臂的长度，增加光的强度，减少各种干扰。</li>
</ul>
</div>
<h3>一些障碍</h3>
<ul>
<li>热噪声</li>
<li>散点噪声</li>
<li>机械噪声</li>
<li>量子噪声</li>
<li>光压噪声</li>
<li>引力梯度噪声</li>
</ul>
<h3>解决方案</h3>
<ul>
<li>光学共振腔：把引力波的效应放大了300倍。</li>
<li>激光共振放大：把光强度从700瓦放大到100千瓦。</li>
<li>信号回收反射镜：增加带宽</li>
<li>震动隔绝系统</li>
<li>高度真空：避免瑞利散射导致的激光相位扰动。</li>
<li>伺服控制：光压导致的试验物体运动是校准过的。</li>
</ul>
</section>

<section><h1>怎么得到黑洞的质量、距离这些信息的</h1>
<div>
前面我们基于相当粗糙的计算用两个黑洞的质量得出了与观测可比的0.006秒这个数字。那么反过来，如果观测数据给出了0.006秒这个数字，我们也应该可以得出两个黑洞质量（之和）。更加精细的模型 (基于数值相对论计算) 与观测数据的时间序列结合可以给出更多信息，比如两个黑洞各自的质量，合并过程造成的质量损失之类。知道了黑洞各自的质量和质量损失，可以算出引力波辐射的功率，结合实际探测到的引力波强度 (激光传播的路径长度的变化比例)，可以得到那两个黑洞与地球的距离，这样算出来的距离称为光度距离。另外，这次的引力波信号被两个在不同地点的观测站几乎同时探测到。“几乎同时”，也就是说，不完全是同时，根据这个时间差 (\(6.9^{+0.5}_{-0.4}毫秒\))，可以得到两个黑洞在天空中的大致方位 (两个观测站只能确定出一个长条形区域)。
</div>
</section>

<section><h1>引力波有多普勒效应吗？</h1>
真空中引力波的传播方程与电磁波的一致 (至少弱场近似下如此)，所以一样会有多普勒效应和宇宙学红移。宇宙学红移的存在意味着观测站测量到的时间长度跟信号出发地点的时间长度不一致，做模型拟合时需要考虑进去。此次发现的论文的附属文章的一个表里分别列出了探测器参考系中的物理量的值，以及源参考系中的值。
</section>

<section><h1>量子化的引力波</h1>
<div>
如果引力子有质量，则传播速度会小于光速，并且不同波长的引力子的传播速度会不同，这会影响引力波的波形。本次发现给出的限制是，引力子的康普顿波长超过\(10^{13}千米\)。这个限制比之前基于太阳系的研究给出的限制好些，但并不比基于弱引力透镜给出的限制好多少。不过这个东西是许多人对这次直接探测到引力波感到特别兴奋的一个原因，因为提供了一种独立的用来研究引力的可能未知特性的方法。
</div>
</section>

<section><h1>黑洞面积不减定理</h1>
<div>
由于引力波辐射，合并后新黑洞的质量小于合并前两个黑洞的质量之和。但这里有个不平凡的东西值得提一下。Bekenstein以及霍金等人发现，黑洞必须携带熵。黑洞熵正比于黑洞表面积，而表面积正比于质量平方。霍金曾经证明了黑洞面积不减定理 (前提是不考虑黑洞的霍金辐射；对于宏观黑洞霍金辐射极慢，当然可以不考虑)，即两个黑洞合并变成一个新黑洞后，新的面积不会比原来各自的面积小。换言之，黑洞合并过程不会违背热力学第二定律。确实是这样，因为
\[
36^2 + 29^2 = 2137 < 62^2 = 3844,
\]
所以面积确实增大了。
也就是说，黑洞合并后，质量是会减小，但不能小太多。
</div>
</section>

<section><h1>References</h1>
<ol>
<li> Abbott, 2016, Observation of Gravitational Waves from a Binary Black Hole Merger </li>
<li> Schutz, 2010, Gravitational Waves, Sources, and Detectors </li>
<li> Bassan, Advanced Interferometers and the Search for Gravitational Waves Lectures from the First VESF School on Advanced Detectors </li>
<li> Saulson, Fundamentals of Interferometric Gravitational Wave Detectors </li>
<li> Weinberg, Gravitation and cosmology principles and applications of the general theory of relativity </li>
<li> <a href="https://dcc.ligo.org/public/0122/P1500227/006/placeholder.pdf"> LIGO Doc1</a> </li>
<li> <a href="https://dcc.ligo.org/LIGO-P1500218/public/main"> LIGO Doc2 </a> </li>
<li> <a href="https://dcc.ligo.org/LIGO-P1500269/public/main"> LIGO Doc3 </a> </li>
<li> http://arxiv.org/abs/1404.5623 </li>
<li> <a href="http://journals.aps.org/prd/abstract/10.1103/PhysRevD.57.2061"> Will, 1998, Bounding the mass of the graviton using gravitational-wave observations of inspiralling compact binaries </a>  </li>
</ol>
</section>
