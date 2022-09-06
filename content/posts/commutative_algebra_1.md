---
title: 交换代数学习笔记 (1)
categories:
  - 数学
tags:
  - Commutative Algebra
math: true
date: 2020-06-09 22:42:18
---

最近在读 Atiyah 的《交换代数导论》(*Introduction to Commutative Algebra*)。
第一章 Rings and Ideals 不难，有抽代基础的话应该很容易。第二章我刚刚看完，有的地方还没太理解（接下来刷一刷习题好了）。这一篇 Blog 是第一章的总结。
<!--more-->


$$
\\gdef\\Ker{\\operatorname{Ker}}
\\gdef\\Image{\\operatorname{Im}}
\\gdef\\Hom{\\operatorname{Hom}}
$$

既然是笔记，那我还是大概把定义总结一下好了。

## Chapter 1. Rings and Ideals 总结

### 环和理想

定义一个**环** *ring* 是同时具有加法阿贝尔群和乘法半群结构，并且满足左右分配律 $(x+y)z=xz+yz;x(y+z)=xy+xz$ 的集合。本书中只考虑（乘法）交换并且有（乘法）单位元的环。

一个**环同态** *ring homomorphism* 是环之间保持加法、乘法、单位元的映射。显然两个环同态的复合还是环同态。

$S$ 是 $R$ 的**子环** *subring* 是说 $S$ 是 $R$ 中一个关于加法和乘法封闭的、包含单位元的子集。

$A$ 的一个**理想** *ideal* $\\mathfrak{a}$ 是一个 $A$ 的对加法封闭的非空子集，且 $A\\mathfrak{a}\\subseteq\\mathfrak{a}$. 给定一个理想，那么所有 $\\mathfrak{a}$ 的陪集（*i.e.* $A$ 中的元素在 $x\\sim y\\iff x-y\\in\\mathfrak{a}$ 这样的等价关系下形成的等价类）构成一个环，称为**商环** *quoetient Ring* $A/\\mathfrak{a}$。

$A$ 的所有包含 $\\mathfrak{a}$ 的理想与 $A/\\mathfrak{a}$ 的所有理想一一对应。

若 $A$ 中 $x$ 与某个非零元素的乘积为 $0$，则称 $x$ 为**零因子** *zero-divisor*。不包含非零零因子的环叫做**整环** *integral domain*。

如果一个元素 $x$ 的某个幂为 $0$（*i.e.* 存在 $n>0$ 使得 $x^n=0$），则称其为**幂零的** *nilpotent*。

如果对某个元素 $x$ 存在 $y$ 使得 $xy=1$，那么 $x$ 称为一个单位 *unit*，$y$ 由 $x$ 唯一确定，记做 $x^{-1}$。$A$ 的所有单位构成一个乘法阿贝尔群。

固定 $x\\in A$，所有 $ax\\;(a\\in A)$ 构成 $A$ 的一个理想，称为**主理想** *principal ideal*，记做 $(a)$。$x$ 是单位当且仅当 $(x)=A=(1)$。**零理想** *zero ideal* $(0)$ 通常记做 $0$。

### 素理想和极大理想

若 $A$ 的每个非零元素都是单位，那么称其为一个**域** *field*。显然域都是整环。其等价条件包括：

- $A$ 仅包含 $0,(1)$ 两个理想。
- 对任意的非零环 $B$，所有 $A\\to B$ 的环同态都是单射。

$A$ 中的一个理想 $\\mathfrak{p}$ 被称为**素的** *prime* 当且仅当 $\\mathfrak{p}\\neq(1)$ 且对任意的 $xy\\in\\mathfrak{p}$，都有 $x\\in\\mathfrak{p}$ 或者 $y\\in\\mathfrak{p}$ 至少之一成立。一个理想 $\\mathfrak{m}$ 称为**极大的** *maximal* 当且仅当 $\\mathfrak{m}\\neq(1)$ 并且不存在理想 $\\mathfrak{a}$ 使得 $\\mathfrak{m}\\subsetneq\\mathfrak{a}\\subsetneq(1)$。或者等价地：

- $\\mathfrak{p}$ 是素理想 $\\iff A/\\mathfrak{p}$ 是整环。
- $\\mathfrak{m}$ 是极大理想 $\\iff A/\\mathfrak{m}$ 是域。

**若无特殊说明，接下来 $\\mathfrak{p,m}$ 均分别指代某个素理想/极大理想。**

于是显然极大理想都是素的，反过来不一定成立。

利用佐恩引理（等价于选择公理）可以证明：每个环 $A$ 至少拥有一个极大理想。推论：

- 对每个理想 $\\mathfrak{a}\\neq(1)$，都存在一个包含它的极大理想。（将上述结论应用到 $A/\\mathfrak{a}$ 上即可）。
- 对 $A$ 的每个非单位 $x$，都存在一个包含它的极大理想（将上一条应用到 $(x)$ 上）。

如果 $A$ 仅包含一个极大理想 $\\mathfrak{m}$，那么称 $A$ 为**局部环** *local ring*，$k=A/\\mathfrak{m}$ 叫做 $A$ 的**剩余域** *residue field*。

如果整环 $A$ 的所有理想都是主理想，那么称之为**主理想整环** *principal integral domain*。这样的环里所有素理想都是极大的。

令 $\\mathfrak{R}$ 表示 $A$ 中所有幂零元素构成的子集，那么它是一个理想并且 $A/\\mathfrak{R}$ 不包含非零的幂零元。$\\mathfrak{R}$ 被称作 $A$ 的 **幂零根** *nilradical*。可以证明它同时也是 $A$ 中所有素理想的交。

类似的，$A$ 中所有极大理想的交 $\\mathfrak{R}$ 称为 **Jacobson根** *Jacobson radical*。$x\\in\\mathfrak{R}\\iff$ 对于任意的$y\\in A$，$1-xy$ 都是单位。

环 $A\_1,A\_2,\\dots,A\_n$ 的有限**直积** *direct product* $\\prod\_{i=1}^n A\_i$ 定义为所有 $(x\_1,\\dots,x\_n)\\;(x\_i\\in A\_i)$ 构成的环，加法和乘法定义为逐项相加/相乘。

### 理想的运算

如果 $\\mathfrak{a,b}$ 都是 $A$ 的理想，那么它们的和 $\\mathfrak{a}+\\mathfrak{b}$ 也是 $A$ 的理想，并且是最小的同时包含两者的理想。更一般的可以定义 $\\sum\_{i\\in I}\\mathfrak{a}\_i$，由所有的 $\\sum\_{i\\in I}x\_i$ 构成，其中 $x\_i\\in\\mathfrak{a}\_i$ 并且只有有限个 $x\_i$ 非零。这是包含所有 $\\mathfrak{a}\_i$ 的最小理想。

对于 $A$ 的一族理想 $(\\mathfrak{a}\_i)\_{i\\in I}$，它们的交 $\\bigcap\_{i\\in I}\\mathfrak{a}\_i$ 也是 $A$ 的理想。

理想的乘积 $\\mathfrak{ab}$ 定义为所有的 $xy\\;(x\\in\\mathfrak{a},y\\in\\mathfrak{b})$ 生成的理想，*i.e.* 所有形如 $\\sum\_i x\_iy\_i$ 的有限和。同样的我们可以定义任意*有限的*一族理想的乘积。

*例子：在 $\\mathbb Z$ 里所有理想都是主理想 $(n)$。此时有 $(n)+(m)=(\\gcd(n,m)),(n)\\cap(m)=(\\mathop{\\rm lcm}(n,m)),(n)(m)=(nm)$。*

这三种运算都是交换、结合的，并且有分配率 $\\mathfrak{a(b+c)=ab+ac}$。在 $\\mathbb Z$ 中我们有 $\\mathfrak{(a+b)(a\\cap b)=ab}$，但是一般地我们只知道右边包含左边。这可以推出 $\\mathfrak{a\\cap b=ab\\iff\\ a+b}=(1)$。

当 $\\mathfrak{a+b}=(1)$ 时我们称 $\\mathfrak{a,b}$ **互素** *coprime*（或者“**互极大**” *comaximal*）。

对于一列理想 $\\mathfrak{a}\_1,\\dots,\\mathfrak{a}\_n$，定义一个映射

$$\\phi:A\\to\\prod\_{i=0}^n(A/\\mathfrak{a}\_i)$$

为 $\\phi(x)=(x+\\mathfrak{a}\_1,\\dots,x+\\mathfrak{a}\_n)$，那么：

- 若 $\\mathfrak{a}\_i$ 两两互素，则 $\\prod\\mathfrak{a}\_i=\\bigcap\\mathfrak{a}\_i$。
- $\\phi$ 是满射 $\\iff\\mathfrak{a}\_i$ 两两互素。
- $\\phi$ 是单射 $\\iff\\bigcap\_i\\mathfrak{a}\_i=0$。

此外有命题：

- 若 $\\mathfrak{a}\\subseteq\\bigcup\_{i=1}^n\\mathfrak{p}\_i$，那么存在一个 $i$ 使得 $\\mathfrak{a}\\subseteq\\mathfrak{p}\_i$。
- 若 $\\bigcap\_{i=1}^n\\mathfrak{a}\_i\\subseteq\\mathfrak{p}$，那么存在一个 $i$ 使得 $\\mathfrak{a}\_i\\subseteq\\mathfrak{p}$。如果 $\\bigcap\_{i=1}^n\\mathfrak{a}\_i=\\mathfrak{p}$，那么存在一个 $\\mathfrak{a}\_i=p$。

定义两个理想的**商** *ideal quotient* $(\\mathfrak{a}:\\mathfrak{b})$ 为所有使得 $x\\mathfrak{b}\\subseteq\\mathfrak{a}$ 的元素构成的理想。特别的，$(0:\\mathfrak{b})$ 称为 $\\mathfrak{b}$ 的**零化子** *annihilator*，记做 $\\mathop{\\rm Ann}(\\mathfrak{b})$。

一个理想 $\\mathfrak{a}$ 的 **根** *radical* 定义为 $r(\\mathfrak{a})=\\{x\\in A\\mid x^n\\in\\mathfrak{a}\\text{ for some } n>0\\}$。如果 $\\phi:A\\to(A/\\mathfrak{a})$ 是标准同态，那么 $r(\\mathfrak{a})=\\phi^{-1}(\\mathfrak{R}\_{A/\\mathfrak{a}})$ 也是 $A$ 的理想。

*某些记号中将 $r(\\mathfrak{a})$ 记做 $\\sqrt{\\mathfrak{a}}$。*

命题：

- $$\\mathfrak{a}\\subseteq(\\mathfrak{a}:\\mathfrak{b});\\;(\\mathfrak{a}:\\mathfrak{b})\\mathfrak{b}\\subseteq\\mathfrak{a};\\;((\\mathfrak{a}:\\mathfrak{b}):\\mathfrak{c})=(\\mathfrak{a}:\\mathfrak{bc})$$
- $$\\left(\\bigcap\_i\\mathfrak{a}\_i:\\mathfrak{b}\\right)=\\bigcap\_i(\\mathfrak{a}\_i:\\mathfrak{b})$$
- $$\\left(\\mathfrak{a}:\\sum\_i\\mathfrak{b}\_i\\right)=\\bigcap\_i(\\mathfrak{a}:\\mathfrak{b}\_i)$$
- $$r(\\mathfrak{a})\\supseteq\\mathfrak{a};\\;r(r(\\mathfrak{a}))=r(\\mathfrak{a});\\;r(\\mathfrak{ab})=r(\\mathfrak{a\\cap b})=r(\\mathfrak{a})\\cap r(\\mathfrak{b})$$
- $$r(\\mathfrak{a})=(1)\\iff\\mathfrak{a}=(1);\\;r(\\mathfrak{a+b})=r(r(\\mathfrak{a})+r(\\mathfrak{b}))$$
- 对任意正整数 $n$，$r(\\mathfrak{p}^n)=\\mathfrak{p}$。
- $r(\\mathfrak{a})$ 是所有包含 $\\mathfrak{a}$ 的素理想的交。

### 理想的扩张与收缩

令 $f:A\\to B$ 为一个环同态。如果 $\\mathfrak{a}\\subseteq A$ 是一个理想，那么 $f(\\mathfrak{a})$ 不一定是理想。定义 $\\mathfrak{a}$ 的**扩张** *extension* $\\mathfrak{a}^e$ 为 $B$ 中由 $f(\\mathfrak{a})$ 生成的理想。

如果 $\\mathfrak{b}\\subseteq B$ 是一个理想，那么 $f^{-1}(\\mathfrak{b})$ 也一定是 $A$ 的理想，称为 $\\mathfrak{b}$ 的**收缩** *contraction*，记做 $\\mathfrak{b}^c$。如果 $\\mathfrak{b}$ 是素的，那么 $\\mathfrak{b}^c$ 也是素的。反过来如果 $\\mathfrak{a}$ 是素的那么 $\\mathfrak{a}^e$ *不一定*是素的。对于扩张与收缩有以下命题：

- $$\\mathfrak{a}\\subseteq\\mathfrak{a}^{ec},\\mathfrak{b}\\supseteq\\mathfrak{b}^{ce}$$
- $$\\mathfrak{a}^e=\\mathfrak{a}^{ece},\\mathfrak{b}^c\\supseteq\\mathfrak{b}^{cec}$$
- 如果令 $C=\\{\\mathfrak{b}^c\\mid\\mathfrak{b}\\subseteq B\\text{ is an ideal}\\},E=\\{\\mathfrak{a}^e\\mid\\mathfrak{a}\\subseteq A\\text{ is an ideal}\\}$，那么就有 $C=\\{\\mathfrak{a}\\mid \\mathfrak{a}^{ec}=\\mathfrak{a}\\},E=\\{\\mathfrak{b}\\mid\\mathfrak{b}^{ce}=\\mathfrak{b}\\}$，并且 $C$ 到 $E$ 由扩张和收缩形成一一对应。

在特殊情况下，如果 $f$ 是满射，那么 $\\mathfrak{a}^e=f(\\mathfrak{a}),\\mathfrak{b}^{ce}=\\mathfrak{b},\\mathfrak{a}^{ec}=\\mathfrak{a}+\\Ker f$。$f$ 是单射的情况很复杂。

理想的五种运算（加法，乘法，交，商，根）的扩张与收缩也有一定性质（等于，包含，包含于），这里暂不赘述。

### 习题选（quan）做

我目前也做了一些第一章练习题，附下：

{% asset\_link Exercises1.tex 第一章习题选做-LaTeX 源码 %}

{% asset\_link Exercises1.pdf 第一章习题选做-PDF%}
