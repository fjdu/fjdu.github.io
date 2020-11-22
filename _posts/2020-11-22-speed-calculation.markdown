---
layout: post
title:  "也许有用的快速计算方法"
date:   2020-11-22 Sun 10:04:22
categories: math
---

<table>
<tr><td>\( a\times 5 \)</td> <td> \( a/2\times10 \) </td></tr>
<tr><td>\( a\times 6 \)</td> <td> \( a\times2\times3 = a + a\times5 \) </td></tr>
<tr><td>\( a\times 7 \)</td> <td> \( a\times2 + a\times5 = a/3 \times (20+1)\) </td></tr>
<tr><td>\( a\times 8 \)</td> <td> \( a\times3 + a\times5 = a\times10 - a\times2 \) </td></tr>
<tr><td>\( a\times 9 \)</td> <td> \( a\times10 - a \) </td></tr>
<tr><td>\( a\times11 \)</td> <td> 从右向左累加 </td></tr>
<tr><td>\( a\times12 \)</td> <td> \(a\times10+a\times2\)  </td></tr>
<tr><td>\( a\times13 \)</td> <td> \(a\times10+a\times3\)  </td></tr>
<tr><td>\( a\times14 \)</td> <td> \(a\times7\times2\)  </td></tr>
<tr><td>\( a\times15 \)</td> <td> \(a\times30/2\)  </td></tr>
<tr><td>\( a\times16 \)</td> <td> \(a\times8\times2\)  </td></tr>
<tr><td>\( a\times17 \)</td> <td> \(a\times10 + a\times7 = a/3 + a/3/2\times100 \)  </td></tr>
<tr><td>\( a\times18 \)</td> <td> \(a\times9\times2\)  </td></tr>
<tr><td>\( a\times19 \)</td> <td> \(a\times20 - a\)  </td></tr>
<tr><td>\( a\times21 \)</td> <td> \(a\times20 + a\)  </td></tr>
<tr><td>\( a\times22 \)</td> <td> \(a\times11\times2\)  </td></tr>
<tr><td>\( a\times25 \)</td> <td> \(a\times100/4\)  </td></tr>
<tr><td>\( a\times33 \)</td> <td> 略小于\(a\times100/3\)  </td></tr>
<tr><td>\( a/7 \)</td> <td> 约等于 \(a\times 3/20\times(1 - 1/20)\)  </td></tr>
<tr><td>\( a/9 \)</td> <td> 约等于 \(a\times 0.11\)  </td></tr>
<tr><td>\( a/17 \)</td> <td> 约等于 \(a\times 6/100\times(1-0.02)\)  </td></tr>
</table>

<table>
<tr><td>\( \sqrt{a} \)</td> <td> \(\displaystyle \frac{a+n^2}{2n} \)，\(n\)是\(\sqrt{a}\)的猜测值 </td></tr>
<tr><td>\( \sqrt[3]{a} \)</td> <td> \(\displaystyle \frac{a+2 n^3}{3n^2} \)，\(n\)是\(\sqrt[3]{a}\)的猜测值 </td></tr>
<tr><td>\( a^{1/k} \)</td> <td> \(\displaystyle \frac{a+(k-1)n^k}{k \times n^{k-1}}\)，\(n\)是\({a}^{1/k}\)的猜测值  </td></tr>
<tr><td>\( (1+x)^{k} \)</td> <td> 约等于\(1 + k x\)，要求\(x\)的绝对值明显小于1；前几个公式由这个推出  </td></tr>
</table>

