---
title: 同调代数学习笔记 (2)
categories:
  - Math
tags:
  - Homology Algebra
  - Undone
mathjax: true
date: 2020-06-18 23:21:01
---

上次只说了阿贝尔范畴上的一些东西，以及蛇引理什么的。这次写一些链复形吧。

## 链复形

链复形的概念最初由代数拓扑/代数几何等方面导出，下面的例子会提到。

### 定义

加性范畴 $\mathcal{A}$ 中的一个*链复形* $A_\bullet$ 是一列 $A_i\in\rm{Ob}(A)$（对每个 $i\in\mathbb Z$）以及之间的态射 $d_n:A_n\to A_{n-1}$，满足 $d_{n-1}d_n=0$：

$$\cdots\xrightarrow{}A_{n+1}\xrightarrow{d_{n+1}}A_n\xrightarrow{d_n}A_{n-1}\xrightarrow{}\cdots$$

*链映射* $f:A_\bullet\to B_\bullet$ 定义为一族态射 $f_n:A_n\to B_n$，使得对任意 $n$ 都有 $d_nf_{n-1}=f_nd_n$（这里我们没有区分两个 $d$）。

$\mathcal{A}$ 中的所有链复形这样组成了一个范畴，记做 $\rm{Ch}(\mathcal{A})$。其中所有满足 $A_{-1}=A_{-2}=\dots=0$ 的链复形构成的满子范畴（满子范畴就是说，如果两个对象处于子范畴内那么它们之间所有态射都处于子范畴内）记做 $\rm{Ch}_{\geq0}(\mathcal{A})$。

对于一个 $A\in\mathcal{A}$，可以把它看做一个链复形 $\cdots\to0\to A\to0\to\cdots$（只有下标为 $0$ 的位置非零），这样就把 $\mathcal{A}$ 看做了 $\rm{Ch}(A)$ 的一个满子范畴。

对于阿贝尔范畴中的链复形 $A_\bullet$，定义其 *$n$-维闭链* $Z_n=\ker d_n$，*$n$-维边缘链* $B_n=\im d_{n+1}$，都是 $A_n$ 的子对象。显然有 $B_n\subseteq Z_n$（i.e. 存在一个子对象之间的单射 $B_n\to Z_n$），因此可以定义商对象 $H_n=Z_n/B_n$（即上述单射的 $\coker$），称为 $A_\bullet$ 的 *$n$-维同调群*。

### 一些引理

> 1. 如果 $\mathcal{A}$ 是阿贝尔范畴，那么 ${\rm Ch}(\mathcal{A})$ 也是。
> 2. 链映射 $f:A_\bullet\to B_\bullet$ 是单射当且仅当每个 $f_n:A_n\to B_n$ 是单射。
> 3. 链映射 $f:A_\bullet\to B_\bullet$ 是满射当且仅当每个 $f_n:A_n\to B_n$ 是满射。
> 4. ${\rm Ch}(\mathcal A)$ 中 $A_\bullet\xrightarrow{f}B_\bullet\xrightarrow{g}C_\bullet$ 是正合的当且仅当每个 $A_n\xrightarrow{f_n}B_n\xrightarrow{g_n}C_n$ 是正合的。

证明很容易：如果每个 $f_n:A_n\to B_n$ 都有核，那么所有 $\ker f_n$ 自然组成了一个链复形，这个链复形（以及 $\ker f_n\to A_n$ 的态射族）构成了 $f$ 的核。余核的情况与此对偶，因此 1. 4. 即证。考虑到任意加性范畴中映射 $h:U\to V$ 是单射当且仅当 $0\to U$ 是其核（满射与此对偶），所以 2. 3. 即证。

### 例子

#### 单纯同调

首先先说一下什么叫单纯复形：

> 单纯形：欧式空间里的若干仿射无关的点构成的一个几何体（它们的凸包）。如果其中有 $n+1$ 个点 $v_0,\dots,v_n$，那么就记其为 $[v_0,\dots,v_n]$ 并称为 $n$-维单纯形。如果从这些顶点中选取一个子集，那么会得到一个 $m$ 维单纯形（$m\leq n$） $[v_{i_0},\dots,v_{i_m}]$，称之为原来的单纯形的一个面。
>
> 单纯复形：若干个单纯形组成的集合 $K$ ，要求 $K$ 中任何一个单纯形的任何一个面还在 $K$ 里面，并且 $K$ 中任意两个单纯形的交集都是它们的一个公共面。（即，*规范相交*）$K$ 的维数就是其中单纯形的最大维数。

如果有一个 $n$-维单纯复形 $K$，令 $K_t$ 表示其中所有 $t$-维单纯形的集合（$0\leq t\leq n$。每个 $t$-维单纯形都有 $t+1$ 个 $(t-1)$-维面；并且如果我们把 $K$ 中所有顶点排好序，那么这些面也是有序的，即存在映射 $\partial_i:K_t\to K_{t-1}\pod{0\leq i\leq t}$ 定义为 $\partial_i([v_0,\dots,v_t])=[v_0,\dots,\hat v_i,\dots,v_t]$（$\hat v_i$ 表示我们删去了 $v_i$）。

任意给定一个环 $R$，定义 $K$ 的*单纯同调* $C_\bullet$ 如下：$C_t$ 为集合 $K_t$ 生成的自由模；如果 $t<0$ 或者 $t>n$ 那么 $C_t=0$；$\partial_i$ 诱导了模同态 $C_t\to C_{t-1}$，也记做 $\partial_i$，那么定义 $d_t:C_t\to C_{t-1}$ 为它们的交错求和

$$d_t=\sum_{j=0}^t(-1)^j\partial_j$$

换句话说，

$$d_t([v_0,\dots,v_t])=\sum_{j=0}^t(-1)^j[v_0,\dots,\hat v_j,\dots,v_t]$$

为了证明这样的映射满足 $d_{t-1}d_t=0$，只需要发现每个 $[v_0,\dots,\hat v_i,\dots,\hat v_j,\dots,v_n]$ 都在 $d_{t-1}(d_t([v_0,\dots,v_t]))$ 中出现了两次，并且系数分别为 $(-1)^{i+j}$ 和 $(-1)^{i+j-1}$。

这样定义出来的链复形 $C_\bullet$ 中的 $t$-维闭链在几何上恰好是一个“没有边界的闭区域”（比方说，一条闭合曲线，一张闭合曲面），而 $t$-维边缘链恰好是一个高一维的区域的边界。这个时候同调群 $H_n$ 就刻画了 $K$ 中所有“$n$-维的洞”；比方说，如果 $K$ 是一个四面体的**表面**，那么 $K$ 中所有面按一定符号加起来就是一个闭链，但是不是一个边缘链（因为 $K$ 不包含这个四面体的内部）。因此此时 $H_3\cong R$，说明了 $K$ 中有恰好一个三维的洞。

#### 奇异同调

奇异同调和单纯同调类似，只不过单纯形奇异单纯形：即，固定一个拓扑空间 $X$，其上的一个奇异 $n$-单纯形定义为 $\Delta^n\to X$ 的一个连续映射（$\Delta^n$ 为 $n$ 维标准单形，即 $R^n$ 中 $e_1,\dots,e_n$ 张成的凸包），而其*奇异同调*的对象 $S_t$ 定义为所有这样的奇异 $t$-单纯形生成的自由 $R$-模。

前面的面映射 $\partial_i:K_t\to K_{t-1}$ 也替换成了 $f\mapsto f\circ\partial_i$，从而诱导映射 $d_n:S_n\to S_{n-1}$；其中 $\partial_0,\dots,\partial_t:\Delta^{t-1}\to\Delta^t$ 为 $\Delta^{t-1}$ 到 $\Delta^t$ 的某个面的投影。

可以证明的是如果 $X$ 本身是一个单纯形，那么其单纯同调及奇异同调之间的链映射 $C_\bullet\to S_\bullet$ 所诱导的其同调群上的 $R$-模同态是同构；也就是说 $X$ 的单纯同调群和奇异同调群同构。
