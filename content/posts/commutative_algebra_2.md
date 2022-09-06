---
title: 交换代数学习笔记 (2)
categories:
  - 数学
tags:
  - 交换代数
math: true
date: 2020-06-10 16:31:42
---

接上文，来总结一下第二章（Modules）学的东西。感觉很可能写不完所以说不定会再开一篇。
<!--more-->

$$
\\gdef\\Ker{\\operatorname{Ker}}
\\gdef\\Coker{\\operatorname{Coker}}
\\gdef\\Image{\\operatorname{Im}}
\\gdef\\Hom{\\operatorname{Hom}}
$$

## Chapter 2. Modules 总结

### 模，模同态

固定一个环 $A$。一个 **$A$-模** *$A$-module* 是一个阿贝尔群 $M$，且 $A$ 在其上有一个线性的作用。换句话说，一个 $A$-模是一个二元组 $(M,\\mu)$，其中 $M$ 是一个阿贝尔群，而 $\\mu$ 是一个 $A\\times M\\to M$ 的映射，满足：

$$\\begin{aligned}
a(x+y)&=ax+ay\\\\
(a+b)x&=ax+bx\\\\
(ab)x&=a(bx)\\\\
1x&=x
\\end{aligned}$$

其中 $a,b\\in A;x,y\\in M$。或者等价的，一个阿贝尔群 $M$ 以及一个 $A\\to E(M)$ 的环同态（$E(M)$ 是 $M$ 的自同态环）。

$A$ 本身，或者更一般的， $A$ 上的任意理想 $\\mathfrak{a}$ 都可以看做 $A$-模。

一个 $A$-模之间的映射 $f:M\\to N$ 是 **$A$-模同态** *$A$-module homomorphism*（或者说是 **$A$-线性** *$A$-linear* 的）当且仅当 $f(x+y)=f(x)+f(y);f(ax)=a\\cdot f(x)$。换句话说 $f$ 是一个群同态，并且它和 $A$ 中元素的作用均交换。$A$-模同态的复合仍然是 $A$-模同态。

特别的，如果 $A=k$ 是一个域，那么 $k$-模就等价于 $k$-线性空间，$k$-模同态就等价于 $k$-线性映射。

固定 $A$-模 $N,M$，则 $N\\to M$ 的所有同态也构成一个 $A$-模：只需定义 $(f+g)(x)=f(x)+g(x);(af)(x)=a\\cdot f(x)$ 即可。这个模记做 $\\Hom\_A(N,M)$；不引起歧义的时候有时候会省去下标 $A$。

同态 $u:M'\\to M$ 以及 $v:N\\to N'$ 分别引导同态

$$\\bar u:\\Hom(M,N)\\to\\Hom(M',N);\\qquad\\bar v:\\Hom(M,N)\\to\\Hom(M,N')$$

定义如下：

$$\\bar u(f)=f\\circ u;\\qquad\\bar v(f)=v\\circ f$$

对任意的 $M$ 存在一个自然的同构 $\\Hom(A,M)\\cong M$，因为任意同态 $f:A\\to M$ 唯一的由 $f(1)$ 确定。

### 子模与商模

如果 $A$-模 $M$ 的一个子群 $M'$ 也构成一个 $A$-模（在标量乘下封闭），那么称之为 $M$ 的一个**子模** *submodule*。此时商群 $M/M'$ 在 $a(x+M')=ax+M'$ 的意义下也构成 $A$-模，称为 $M$ 对 $M'$ 的**商模** *quotient module*。$M\\to M/M'$ 的自然的变换（将每个元素映射到它所在的陪集）同时也是 $A$-模同态。

对任意 $M$ 的子模 $M'$，存在一个从 $M/M'$ 的子模到 $M$ 中包含 $M'$ 的子模的一一对应。*~~（这句话哪里都有，群，环（理想），模...）~~*

如果 $f:M\\to N$ 是 $A$-模同态，那么 $f$ 的**核** *kernel* 定义为 $\\Ker f=\\{x\\in M\\mid f(x)=0\\}$，它是 $M$ 的一个子模。

$f$ 的**像** *image* 为 $\\Image f=\\{f(x)\\mid x\\in M\\}$，是 $N$ 的一个子模。其**余核** *cokernel*（有的地方也翻译成“上核”？~~反正就是 co-核就对了~~）定义为 $N/\\Image f$。

如果 $M'\\subseteq\\Ker f$ 是一个子模，那么 $f$ 可以引导出同态 $\\bar f:M/M'\\to N$，定义为 $\\bar f(x+M')=f(x)$，此时 $\\bar f$ 的核为 $\\Ker f/M'$。特别的，取 $M'=\\Ker f$，就得到一个 $A$-模同构

$$M/\\Ker f\\cong\\Image f$$

### 子模上的运算

模的运算和理想类似：对于一族 $M$ 的子模 $(M\_i)\_{i\\in I}$，可以定义它们的和 $\\sum\_{i\\in I} M\_i$（包含所有形如 $\\sum\_{i\\in I} x\_i$ 的元素，其中 $x\_i\\in M\_i$ 且只有至多有限项非零），它们的交 $\\bigcap\_{i\\in I}M\_i$。两者都也是 $M$ 的子模。

另外，如果两个子模之间有包含关系，那么可以把其中一个看做另一个的子模然后求商模。有如下两个命题成立：

- 若 $L\\supseteq M\\supseteq N$ 是 $A$-模，则
  $$(L/N)/(M/N)\\cong L/M$$
- 若 $M\_1,M\_2$ 都是 $M$ 的子模，则
  $$(M\_1+M\_2)/M\_1\\cong M\_2/(M\_1\\cap M\_2)$$

对于第一个，构造满同态 $\\phi:L/N\\to L/M;\\phi(x+N)=x+M$，则其核为 $M/N$，因此命题成立。

对于第二个，构造满同态 $\\theta:M\_2\\to(M\_1+M\_2)/M\_1;\\theta(x)=x+M\_1$，那么其核为 $M\_1\\cap M\_2$，命题成立。$\\square$

一般地我们没办法定义两个子模的积，但是我们可以定义子模和理想的积 $\\mathfrak{a}M$，即所有形如 $\\sum\_i a\_ix\_i\\;(a\_i\\in\\mathfrak{a},x\_i\\in M)$ 的元素构成的子模。

对两个子模 $N,P\\subseteq M$，定义 $(N:P)$ 为所有使得 $xP\\subseteq N$ 的元素 $x\\in A$，它是一个 *$A$ 的理想*。特别的，$(0,M)$ 是所有使得 $xM=0$ 的元素 $x\\in A$，称为 $M$ 的**零化子** *anhilator*，记做 $\\mathop{\\rm Ann}(M)$。如果 $\\mathfrak{a}\\subseteq\\mathop{\\rm Ann}(M)$，则 $M$ 也可以看做一个 $A/\\mathfrak{a}$-模。

如果 $\\mathop{\\rm Ann}(M)=0$，那么称 $M$ 为**忠实** *faithful* 的。如果 $\\mathop{\\rm Ann}(M)=\\mathfrak{a})$，那么 $M$ 可以看做一个忠实 $A/\\mathfrak{a}$-模。

### 模的直和与直积

如果 $M,N$ 是 $A$-模，它们的**直和** *direct sum* $M\\oplus N$ 定义为其笛卡尔积构成的 $A$-模，加法和标量乘法均逐项进行。更一般的，如果 $(M\_i)\_{i\\in I}$ 是一族 $A$-模，那么它们的直和 $\\bigoplus\_{i\\in I}M\_i$ 定义为所有 $(x\_i)\_{i\\in I}$ 构成的模，其中 $x\_i\\in M\_i$ 且只有有限项非零。

如果我们把“只有有限项非零”的限制去掉，那么得到的 $A$-模称为 $(M\_i)\_{i\\in I}$ 的**直积** *direct product*，记做 $\\prod\_{i\\in I}M\_i$。于是直和与直积在有限维的情况下是相同的，无限维则不一定。

如果环 $A$ 可以写作直积 $\\prod\_{i=1}^nA\_i$，那么所有形如

$$(0,0,\\dots,0,a\_i,0,\\dots,0)$$

其中 $a\_i\\in A\_i$ 的元素，形成了 $A$ 的一个理想 $\\mathfrak{a}\_i$（并且是一个主理想，其生成元 $e\_i$ 是一个幂等元（$e\_i^2=e\_i$）），且 $A$ 看做 $A$-模可以写作所有 $\\mathfrak{a}\_i$ 的直和。并且若令 $\\mathfrak{b}\_i=\\bigoplus\_{j\\neq i}\\mathfrak{a\_i}$，那么 $A\_i\\cong A/\\mathfrak{b}\_i$，因此 $A\\cong\\prod\_{i=1}^n(A/\\mathfrak{b}\_i)$。

### 有限生成模

如果一个 $A$-模 $M$ 可以由有限个元素生成（$M=\\sum\_{i=1}^n(x\_i)$），那么称它为 **有限生成** *finitely generated* 的。如果一个 $A$-模 $M$ 同构于 $\\bigoplus\_{i\\in I}M\_i$ 的形式并且每个 $M\_i$ 同构于 $A$，就称它为**自由** *free* 的，有时记做 $A^{(I)}$。

有限生成的自由模都长成 $\\bigoplus\_{i=1}^n A$ 的样子，记做 $A^n$。（以 $A^0$ 表示平凡模 $0$）

命题：一个模是有限生成的当且仅当它是某个 $A^n$ 的商模。

**Mark**: 本节后面的几个命题咱不太理解用来做什么。

命题：如果 $M$ 是有限生成的，$\\phi:M\\to M$ 是模自同态并且 $\\phi(M)\\subset\\mathfrak{a}M$，那么 $\\phi$ 满足一个长成这样的等式

$$\\phi^n+a\_1\\phi^{n-1}+\\dots+a\_n=0$$

其中 $a\_i\\in\\mathfrak{a}$。

推论：若 $\\mathfrak{a}M=M$，那么存在一个 $x\\equiv1\\pmod{\\mathfrak{a}}$ 使得 $xM=0$。取 $\\phi(x)=x$，$x=1+a\_1+a\_2+\\dots+a\_n$ 即可。

命题（*中山正引理 Nakayama's Lemma*）：如果 $M$ 是有限生成的，$\\mathfrak{a}\\subseteq\\mathfrak{R}$ 是包含在 Jacobson 根里的一个理想，那么 $\\mathfrak{a}M=M$ 蕴含 $M=0$。（存在 $x\\equiv1\\pmod{\\mathfrak{R}}$ 使得 $xM=0$，但是这样的 $x$ 都是单位。）

推论：$M,\\mathfrak{a}$ 如上个命题所述，$N\\subseteq M$ 使得 $M=\\mathfrak{a}M+N$，则 $M=N$。（应用到 $M/N$ 中即可）

推论：若 $A$ 是局部环，$\\mathfrak{m}$ 是其极大理想而 $k=A/\\mathfrak{m}$ 是其剩余域，$M$ 是有限生成 $A$-模，则 $M/\\mathfrak{m}M$ 显然由 $\\mathfrak{m}$ 零化，因此是一个 $k$-模（$k$-线性空间），且显然是有限维的。此时若 $x\_1,\\dots,x\_n$ 在 $M/\\mathfrak{m}M$ 中的像形成线性空间的一组基，那么 $x\_1,\\dots x\_n$ 生成 $M$。（若令 $N$ 表示 $x\_i$ 生成的子模，那么有 $N+\\mathfrak{m}M=M$。）

### 正合列

hmm，终于写到正合列了~~噩梦的开始~~。

一个由 $A$-模和 $A$-模同态构成的序列

$$\\cdots\\xrightarrow{}M\_{i-1}\\xrightarrow{f\_i}M\_i\\xrightarrow{f\_{i+1}}M\_{i+1}\\xrightarrow{}\\cdots$$

称为**正合** *exact* 的，如果 $\\Image f\_i=\\Ker f\_{i+1}$。特别的：

- $0\\xrightarrow{}M'\\xrightarrow{f}M$ 正合当且仅当 $f$ 是单射；
- $M\\xrightarrow{g}M'\\xrightarrow{}0$ 正合当且仅当 $g$ 是满射；
- $0\\xrightarrow{}M'\\xrightarrow{f}M\\xrightarrow{g}M''\\xrightarrow{}0$ 正合当且仅当 $f$ 是单射、$g$ 是满射，且 $g$ 诱导了 $\\Coker(f)$ 到 $M''$ 的同构。

最后这种正合列叫做**短正合列** *short exact sequence*。任意的正合列都可以拆成短正合列：定义 $N\_i=\\Image f\_i=\\Ker f\_{i+1}$，则 $0\\xrightarrow{}N\_i\\xrightarrow{}M\_i\\xrightarrow{}N\_{i+1}\\xrightarrow{}0$ 对于每个 $i$ 都是正合的（反过来也对）。

命题：

1. $M'\\xrightarrow{u}M\\xrightarrow{v}M''\\xrightarrow{}0$ 正合当且仅当对于任意的 $A$-模 $N$，下述序列正合：
   $$0\\xrightarrow{}\\Hom(M'',N)\\xrightarrow{\\bar v}\\Hom(M,N)\\xrightarrow{\\bar u}\\Hom(M',N)$$
2. $0\\xrightarrow{}N'\\xrightarrow{u}N\\xrightarrow{v}N''$ 正合当且仅当对于任意的 $A$-模 $M$，下述序列正合：
   $$0\\xrightarrow{}\\Hom(M,N')\\xrightarrow{\\bar u}\\Hom(M,N)\\xrightarrow{\\bar v}\\Hom(M,N'')$$

这个命题的四个部分“都是简单的练习题”。

hm，我好像还没证过，我来试试。

{% spoiler "此命题的证明" %}

(1.) $\\Image plies$: $v$ 是满射 $\\Image plies$ $\\bar v:f\\mapsto f\\circ v$ 是单射，显然。

对于 $\\bar u$ 和 $\\bar v$，不难证明 $f\\in\\Ker\\bar u\\iff\\Ker f\\supseteq\\Image u=\\Ker v$。那么 $\\Image\\bar v\\subseteq\\Ker\\bar u$ 立即成立（$g\\circ v$ 的核肯定包含 $v$ 的核）。

反过来如果 $\\Ker f\\supseteq\\Ker v$，那么复合映射

$$g:M''\\xrightarrow{v^{-1}}M/\\Ker v\\xrightarrow{}M/\\Ker f\\xrightarrow{f}N$$

就是满足 $f=g\\circ v$ 的同态（*注意这里用到了 $v$ 是满射*）。因此我们有 $\\Image\\bar v=\\Ker\\bar u$。

(1.) $\\Image pliedby$: 取 $N=M''/\\Image v$，考虑投影同态 $\\phi:M''\\to N$。显然 $\\bar v(\\phi)=\\phi\\circ v=0$，因此由 $\\bar v$ 的单射性可知 $\\phi=0$，于是 $N$ 为零模，即 $\\Image v=M''$，$v$ 是满射。

取 $N=M''$，那么由于 $\\bar u\\circ\\bar v=0$，也就是说对任意的 $f$ 都有 $v\\circ u\\circ f=0$，取 $f$ 为恒等映射可知 $v\\circ u=0$，即 $\\Image u\\subseteq\\Ker v$。

再取 $N=M/\\Image u$，令 $\\phi:M\\to N$ 为投影同态，则 $\\bar u(\\phi)=\\phi\\circ u=0$，即 $\\phi\\in\\Ker\\bar u=\\Image\\bar v$。因此存在 $\\psi:M''\\to M/\\Image u$ 使得 $\\phi=\\psi\\circ v$，所以 $\\Image u=\\Ker\\phi\\supseteq\\Ker v$。

(2.) $\\Image plies$: 如果 $u$ 是单射，那么显然 $\\bar u:f\\mapsto u\\circ f$ 是单射。

接下来有 $f\\in\\Ker\\bar v\\iff v\\circ f=0\\iff\\Image f\\subseteq\\Ker v=\\Image u$，因此显然 $\\Image\\bar u\\subseteq\\Ker\\bar v$（显然 $u\\circ f$ 的像包含在 $u$ 的像里）。

反过来，如果 $\\Image f\\subseteq\\Image u$，那么因为 $u$ 是单射，所以肯定可以把 $f$ 的像都拉回到 $N'$ 中，这样就得到了一个同态 $g$ 使得 $f=u\\circ g$。因此 $\\Image\\bar u\\supseteq\\Ker v$。

(2.) $\\Image pliedby$: 这个好像很简单的样子。取 $M=A$，然后由 $\\Hom(A,N)\\cong N$，并且这种同构意义下 $\\bar u,\\bar v$ 和 $u,v$ 是一模一样的。

{% endspoiler %}

命题（**“蛇引理”**，*Snake Lemma*）：

如果我们有这样一张交换图（*i.e.* $b\\circ f=f'\\circ a,c\\circ g=g'\\circ b$）：

{% asset\_img snake\_lemma.svg "Snake Lemma" %}

其中 $A,B,C,A',B',C'$ 都是 $A$-模，$a,b,c,f,g,f',g'$ 都是 $A$-模同态，并且上下两行都是正合列，则存在这样一个正合列：

$$0\\rightarrow\\Ker a\\xrightarrow{\\bar f}\\Ker b\\xrightarrow{\\bar g}\\Ker c\\xrightarrow{d}\\Coker a\\xrightarrow{\\bar f'}\\Coker b\\xrightarrow{\\bar g'}\\Coker c\\rightarrow0$$

这里 $\\Ker$ 之间的映射是 $f,g$ 的限制（根据交换性易证 $f(\\Ker a)\\subseteq\\Ker b$ etc.），而 $\\Coker$ 之间的映射由 $f',g'$ 诱导而出（同样易证 $f'(\\Image a)\\subseteq\\Image b$ etc.）。

事实上左上和右下的 $0$ 可以省掉，相应的结果中左右的 $0$ 也会省掉。

中间的这个 $d$ 称作**边缘同态** *boundary homomorphism*，它的构造以及正合列的证明如下：

{% spoiler "Proof of Snake Lemma" %}

首先来定义 $d$。若 $x\\in\\Ker c\\subseteq C$，那么由于 $g$ 是满射，所以存在 $y\\in B$ 使得 $x=g(y)$。此时 $g'(b(y))=c(g(y))=c(x)=0$，即 $b(y)\\in\\Ker g'=\\Image f'$，因此存在 $t\\in A$ 使得 $f'(t)=b(y)$。取这样的 $t$ 所在的等价类（即映射到 $\\Coker a=A'/\\Image a$ 上）作为 $d(x)$ 即可。

这样的话我们其实没有证明 $d$ 是良定义的（注意这里 $f'$ 是单射，因此仅存在 $y$ 的选择这里是不确定的）。我们先简单地证明一下它是良定义的：如果我们选择了两个不同的 $y\_1,y\_2$ 使得 $g(y\_1)=g(y\_2)=x$，那么 $y\_1-y\_2\\in\\Ker g=\\Image f$，因此 $b(y\_1)-b(y\_2)\\in\\Image(b\\circ f)$。而对应地选取的两个 $t$ 满足 $f'(t\_1)=b(y\_1),f'(t\_2)=b(t\_2)$，从而 $f(t\_1-t\_2)\\in\\Image(f'\\circ a)=f'(\\Image a)$；而 $f'$ 是单射，就有 $t\_1-t\_2\\in\\Image a$，因此 $t\_1$ 和 $t\_2$ 会处在 $\\Image a$ 的相同陪集里面，从而不影响 $d(x)$ 的取值。

接下来要证明结果是正合列。对于 $\\bar f$ 是单射、$\\Image\\bar f=\\Ker\\bar g$ 以及反过来 $\\Coker$ 之间的两个映射，证明都是显然的；其实只需要证明 $\\Image\\bar g=\\Ker d$，以及 $\\Image d=\\Ker\\bar f'$。

为此我们看看 $d$ 到底做了什么：

1. 首先，对于 $x\\in\\Ker c$，利用 $g$ 的“逆映射”将其拉回到 $M=(c\\circ g)/\\Ker g$。这一步是一个同构 $\\Ker c\\cong M$。
2. 利用 $b$ 把 $M$ 映射到 $B$ 里面去。由于上一步我们把相差一个 $\\Ker g=\\Image f$ 的元素都等价起来了，所以这一步我们要把相差一个 $b(\\Image f)=\\Image(b\\circ f)$ 的元素都等价起来。因此我们把 $M=\\Ker(g'\\circ b)/\\Image f$ 映射到了 $N=\\Ker g'/\\Image(b\\circ f)$。这一步不一定是同构。
3. 将上一步中 $N$ 的元素通过 $f'$ 拉回去。由于 $N$ 只包含若干 $\\Ker g'=\\Image f'$ 中的等价类，因此是可以拉回去的。并且 $f'$ 是单射，所以拉回去的时候不用考虑额外商掉核什么的。因此我们有 $N=\\Image f'/\\Image(f'\\circ a)\\cong A'/\\Image a=\\Coker  a$。

接下来为了验证正合性，就要计算 $\\Ker d$ 以及 $\\Image d$。由于 $d$ 是两边两个同构中间一个同态，所以其实只需要考虑求中间那个由 $b$ 诱导的同态（下面记做

$$\\bar b:\\Ker(g'\\circ b)/\\Image f\\to\\Ker g'/\\Image(b\\circ f)$$

）的像以及核。

$\\bar b$ 的核：如果 $\\bar b(x+\\Image f)=0$，即 $b(x)\\in\\Image(b\\circ f)=b(\\Image f)$，那么不难证明 $x\\in\\Image f+\\Ker b$。因此 $\\Ker\\bar b=(\\Image f+\\Ker b)/\\Image f$，把它用 $g$ 拉过去就可以知道 $\\Ker d=g(\\Ker b)$，也就正好是正合列中限制了 $g$ 的定义域之后它的像。

$\\bar b$ 的像；$\\Ker(g'\\circ b)$ 作用一个 $b$ 之后得到的是 $\\Image b\\cap\\Ker g'$，因此 $\\bar b(M)=(\\Image b\\cap\\Image f')/\\Image(f'\\circ a)$。把它用 $f'$ 拉回去可以得到 $\\Image d=f^{-1}(\\Image b)/\\Image a$。而 $\\bar x\\in\\Ker\\bar f'\\iff f'(x)\\in\\Image b$，因此即得 $\\Image d=\\Ker\\bar f'$。

{% endspoiler %}

这样我们就证明了蛇引理。

令 $C$ 表示一族 $A$-模，$\\lambda$ 为一个 $C\\to\\mathbb{Z}$ 的函数（或者更一般地，到任意阿贝尔群）。如果对任意的短正合列 $0\\to M'\\to M\\to M''\\to 0$ 都有 $\\lambda(M')-\\lambda(M)+\\lambda(M'')=0$，就称 $\\lambda$ 是**加性** *additive* 的。

举例：若 $A$ 是一个域 $k$，$C$ 是所有的有限维 $k$-向量空间，$\\lambda(M)=M$ 的维数，则 $\\lambda$ 是加性的。

命题：若 $\\lambda$ 如上定义，$0\\to M\_0\\to M\_1\\to\\dots\\to M\_n\\to0$ 正合，那么

$$\\sum\_{i=0}^n(-1)^i\\lambda(M\_i)=0$$

证明：把它拆成 $n+1$ 个短正合列 $0\\to N\_i\\to M\_i\\to N\_{i+1}$，注意到 $\\lambda(M\_i)=\\lambda(N\_i)+\\lambda(N\_{i+1})$，因此交错求和结果为 $0$。

*PS*：看上去这个证明要求 $C$ 中的模的子模还在 $C$ 中。

本篇 Blog 暂时到此为止吧，第二章的知识比第一章困难多了...

下一篇继续写张量积相关内容（下一篇大约要过几天了，不做习题的情况下根本不明白这些东西是用来做什么的）。qwq
