---
title: Tellegens Principle
categories:
  - 算法
tags:
  - FFT/NTT
math: true
date: 2020-05-18 22:05:46
---

### 问题

给出一个 $m\\times n$ 的矩阵 $A$，以及一个用于计算 $u\\mapsto Au$ 的（线性的）算法，求一个几乎同样次数乘、同样次加减法的用于计算 $v\\mapsto A^Tv$ 的算法。

本篇 Blog 是个人口嗨。

<!--more-->

### 形式描述

我们有一个**函数式**的计算模型：

一个具有 $n$ 个输入位和 $m$ 个输出位的*线性算法*定义如下：

一个自然数 $l$， $l$ 条以 $i=n+1,\\dots,n+l$ 编号的指令：

- $r\_i=r\_j+r\_k$，其中 $1\\leqslant j,k<i$；
- $r\_i=\\alpha r\_j$，其中 $\\alpha$ 是常数，$1\\leqslant j<i$。

以及一个输出位的映射：$f:\\{1,2,\\dots,m\\}\\to\\{1,2,\\dots,n+l\\}$。

一个算法*计算*一个矩阵 $P\_{m\\times n}$ 当且仅当：

对于任意列向量 $u=(u\_1,u\_2,\\dots,u\_n)^T$，如果定义：

- $r\_i=u\_i$，若 $i\\leqslant n$；
- 按算法中的描述定义 $r\_i$，若 $i>n$。

则 $\\forall i, r\_{f(i)}=(Pu)\_i$。

我们需要做的是：给定一个计算 $A$ 的算法，求一个计算 $A^T$ 的算法，要求两者中的乘法次数相同，加法次数相差 $m-n$。我们假定对于 $A,A^T$ 都不存在恒为 $0$ 的输出。

我们也假设算法里没有“无用变量”，即不出现在后面的计算也不作为输出位的变量。

### 构造

为了方便表示，我们把变量记做不同的英文字母。我们以 $I\_x$（仅出现在前 $n$ 个位置，占位用）表示 $x$ 这个变量是输入的；以 $O\_x$（仅出现在最后，表示输出位映射）。如下是两对例子：

$I\_a;I\_b;c=2a;d=3b;e=c+d;O\_e$ 计算了 $\\begin{pmatrix}2&3\\end{pmatrix}$。对应地，$I\_d;a=2d;b=3d;O\_a;O\_b$ 计算了 $\\begin{pmatrix}2\\\\3\\end{pmatrix}$

$I\_a;I\_b;I\_c;d=a+b;e=c+d;O\_e;O\_e$ 计算了 $\\begin{pmatrix}1&1&1\\\\1&1&1\\end{pmatrix}$。对应地，$I\_a;I\_b;c=a+b;O\_c;O\_c;O\_c$ 计算了 $\\begin{pmatrix}1&1\\\\1&1\\\\1&1\\end{pmatrix}$。

那么我们定义一个算法的*转置*如下：

- 将输入变量记为 $i\_1,\\dots,i\_m$；
- 对于原算法中的每个变量 $x$，定义一个新变量 $x'$。
- 在原算法中每有一条指令 $y=\\alpha x$，就将 $\\alpha y'$ 加到 $x'$ 上（通过定义临时变量）；
- 在原算法中每有一条指令 $y=x+z$，就将 $y$ 加到 $x$ 上；
- 在原算法中每有一条指令 $O\_x$（实际上并不是“指令”，这里我们意会一下），就把对应位置的 $i\_-$ 加到 $x$ 上；
- 特殊情况：若只存在一条指令用到 $x$：如果是乘法指令 $y=\\alpha x$，那么直接定义 $x'=\\alpha y'$；如果是加法或输出，那么不定义新变量，而是使用相应的变量代替 $x'$。
- 新算法中的输出位对应原算法中的输入变量，处理与上述的一般变量相同。

### 证明

首先我们证明这样定义的转置和原来的乘法、加法次数满足要求：如果原算法中一个变量在 $a$ 个指令里被作为加数（$y=x+x$ 的情况算两遍），$b$ 个指令里被作为乘数，$c$ 个位置作为输出，那么在转置中，就会出现 $b$ 次乘法、以及 $a+b+c-1$ 次加法。所以若原算法中共有 $a$ 次加法指令和 $b$ 次乘法指令，那么新算法就会有 $2a+b+m-(a+b+n)=a+(m-n)$ 次加法，$b$ 次乘法。

接下来还要证明新算法正确地计算了 $A^T$。对于原算法一个变量 $x$，及一个输出位 $i$，定义 $f\_{x,i}$ 为 “$x$ 对 $i$ 的贡献”。严格的说，定义为：若强行将算法中所有在 $x$ 之前的所有变量置为 $0$（若 $x$ 本身为输入变量，则改为把除 $x$ 外的所有输入变量置为 $0$），而 $x$ 置为 $1$，$x$ 后面的变量照常计算，此时算法的第 $i$ 个输出位的值。

显然若 $t$ 为第 $j$ 个输入变量，则 $f\_{t,i}=P\_{j,i}$。

对于一个变量 $x$，定义 $g\_{x,i}$ 表示当转置算法的第 $i$ 位输入为 $1$ 而其它为 $0$ 的时候 $x'$ 的值（如果 $x$ 是那个“特殊情况”，那么改为代替 $x'$ 的变量）。因此不难发现若 $t$ 是原算法第 $j$ 个输入变量，那么 $g\_{t',i}$ 就表示新算法计算的矩阵中 $i,j$ 位置的值。因此只需证明 $f\_{t,i}=g\_{t',i}$ 即可。

而 $f\_{t,i}=g\_{t',i}$ 其实是显然的：固定 $i$，对于 $t$ 在原算法中的出现位置从后往前进行归纳，容易证明此结论。

因此，定理成立。
