---
title: 交换代数学习笔记 (3)
categories:
  - Math
tags:
  - Commutative Algebra
mathjax: true
date: 2020-06-14 16:13:25
---

继续接上文，这篇补上第二章后面一半，以及习题选（quan）做。

<!--more-->

## Chapter 2. Modules 总结

### 模的张量积

令 $M,N,P$ 为 $A$-摸。一个映射 $f:M\times N\to P$ 被称为是 **$A$-双线性** *$A$-bilinear* 的，当且仅当对于每个 $x\in M$, 映射 $y\mapsto f(x,y)$ 是 $A$-线性的，并且对于每个 $y\in N$，映射 $x\mapsto f(x,y)$ 也是 $A$-线性的。

我们将要构造一个 $A$-模 $T$，称为 $M$ 和 $N$ 的 **张量积** *tensor product*，使得所有 $A$-双线性的映射 $M\times N\to P$ 都自然地一一对应到一个 $A$-线性的映射 $T\to P$。严格地说：

- 令 $M,N$ 为 $A$-模，那么存在一个二元组 $(T,g)$，其中 $T$ 是 $A$-模 而 $g:M\times N\to  T$ 是 $A$-双线性映射，且满足这样的性质：
- 对于任意的 $A$-模 $P$ 和 $A$-双线性映射 $f:M\times N\to P$，都存在唯一的 $A$-线性映射 $f':T\to P$ 使得 $f=f'\circ  g$。
- 进一步地，如果 $(T,g)$ 和 $(T,g')$ 都满足这样的性质，那么存在唯一的同构 $j:T\to  T',j\circ g=g'$。

换句话说，$(T,g)$ 具有如下交换图所示的“~~宇宙财产~~泛性质/万有性质”（*universal property*）：

{% asset_img tensor_product.svg Tensor product %}
  
事实上唯一性（两个满足条件的 $T$ 至多相差一个同构）是直接由 $T,T'$ 的性质导出的：如果 $(T,g)$ 和 $(T',g')$ 都满足条件，那么存在唯一的同态 $j:T\to T'$ 使得 $g'=j\circ g$，反过来也存在唯一的同态 $j':T'\to T$ 使得 $g=j'\circ  g'$. 这样的话我们就有 $g=(j'\circ j)\circ g'$。

但是根据 $T$ 的万有性质，所有满足 $g=h\circ g$ 的映射 $h:T\to T$ 是*唯一*的，但是 $j'\circ j$ 和 ${\rm id}_T$ 又都满足，所以肯定有 $j'\circ j={\rm id}_T$。反过来也有 $j\circ j'={\rm id}_{T'}$，所以 $j$ 就是一个同构。

那么接下来还需要构造这样的一个 $T$。这一部分也很容易：

首先，令 $C=A^{(M\times N)}$，即 $M\times N$（集合笛卡尔积，忽略模结构）的所有元素生成的自由模。接下来令 $D$ 表示所有形如下述的元素生成的子模：

$$
(x_1+x_2,y)-(x_1,y)-(x_2,y)\\
(x,y_1+y_2)-(x,y_1)-(x,y_2)\\
(ax,y)-a\cdot(x,y)\\
(x,ay)-a\cdot(x,y)
$$

然后令 $T$ 为商模 $C/D$，然后记 $(x,y)$ 在 $T$ 中的像为 $x\otimes y$。由 $T$ 的定义可以知道

$$\begin{aligned}
(x_1+x_2)\otimes y&=x_1\otimes y+x_2\otimes y\\
x\otimes(y_1+y_2)&=x\otimes y_1+x\otimes y_2\\
(ax)\otimes y=x\otimes(ay)&=a(x\otimes y)
\end{aligned}$$

也就是说，$g(x,y)=x\otimes y$ 是 $M\times N\to T$ 的双线性映射。接下来要验证 $T$ 的万有性质。

若 $f$ 是 $M\times N\to P$ 的双线性映射，那么可以把它扩展为 $\bar f:C\to P$。根据双线性的定义可以知道 $D\subseteq\Ker\bar f$，因此 $\bar f$ 引导了 $T=C/D$ 上的线性映射 $f':T\to P$，且 $f'(x\otimes y)=f(x,y)$。由于形如 $x\otimes y$ 的元素生成了 $T$，所以所有 $f(x,y)$ 唯一确定了 $f'$ 的取值。这样我们就证明了 $T$ 的万有性。

这样构造的 $T$ 记做 $M\otimes_A N$（不引起歧义的前提下有时会省略下标 $A$），称为 $M$ 与 $N$ 的张量积，它由所有 $x\otimes y$ 生成。如果 $(x_i)_{i\in I},(y_j)_{j\in J}$ 生成了 $M,N$，那么 $(x_i\otimes y_j)_{(i,j)\in I\times J}$ 就生成了 $T$；特别的，如果 $M$ 和 $N$ 都是有限生成的，那么 $T$ 也是。

**注**：如果不明确处于哪个张量积内，那么记号 $x\otimes y$ 是有歧义的。考虑若 $M'\subseteq M,N'\subseteq N$ 是子模，而 $x\in M',y\in N'$，那么有可能 $x\otimes y=0\in M\otimes N$ 但是 $x\otimes y\neq0\in M'\otimes N'$。比方说，若 $A=\mathbb{Z},M=\mathbb{Z},N=\mathbb{Z}/2\mathbb{Z}$ 而 $M'=2\mathbb{Z},N'=N$。那么在 $M\times N$ 中 $2\otimes x=1\otimes(2x)=1\otimes0=0$。但是在 $M'\otimes N'$  里面 $2\otimes1$ 非零。（事实上，作为 $\mathbb{Z}$ 模，$M\cong M'$ 由 $x\mapsto 2x$ 给出）。

然而有如下命题：

> 若在 $M\otimes_A N$ 中有 $\sum_i x_i\otimes y_i=0$，那么存在有限生成子模 $M_0\subseteq M,N_0\subseteq N$ 使得在 $M_0\otimes_A N_0$ 中也有 $\sum_i x_i\otimes y_i=0$。

证明很简单：在 $M\otimes_A N$ 中此元素非 $0$，也就是说在 $A^{(M\times N)}$ 中它属于子模 $D$，因此它是有限个 $D$ 的生成元的加权和。令 $M_0$ 为所有 $x_i$ 以及这些生成元中涉及到的所有 $x$ 生成的子模，$N_0$ 同理，就有 $\sum_i x_i\otimes y_i=0\in M_0\otimes_A N_0$。

如果我们把张量积的出发点从双线性映射改成“多线性映射” $f:M_1\times M_2\times\cdots\times M_r\to P$，那么我们可以得到“多重张量积” $T=M_1\otimes M_2\otimes\cdots\otimes M_r$，它由所有的 $x_1\otimes x_2\otimes\cdots\otimes x_r\quad(x_i\in M_i)$ 生成。

另外我们也可以定义同态的 tensor product：若 $f:M\to M',g:N\to N'$，那么可以定义 $h(x,y)=f(x)\otimes g(y)$，它是 $M\times N\to M'\otimes N'$ 的双线性映射，因此诱导映射 $M\otimes N\to M'\otimes N'$，记做 $f\otimes g$。

如果 $f':M'\to M'',g:N'\to N''$，那么显然 $(f'\circ f)\otimes(g'\circ g)$ 和 $(f'\otimes g')\circ(f\otimes g)$ 在所有 $x\otimes y$ 上取值相同，从而是相等的映射，即

$$(f'\circ f)\otimes(g'\circ g)=(f'\otimes g')\circ(f\otimes g)$$

考虑直和以及张量积组合成的一些模，我们有一些“典范同构”（canonical isomorphism）：

1. $M\otimes N\to N\otimes M$
2. $(M\otimes N)\otimes P\to M\otimes(N\otimes P)\to M\otimes N\otimes P$
3. $(M\oplus N)\otimes P\to(M\otimes P)\oplus(N\otimes P)$
4. $A\otimes  M\to M$

它们分别定义如下：

1. $x\otimes y\mapsto y\otimes x$
2. $(x\otimes y)\otimes z\mapsto x\otimes(y\otimes z)\mapsto x\otimes y\otimes z$
3. $(x,y)\otimes z\mapsto (x\otimes z,y\otimes z)$
4. $a\otimes x\mapsto ax$

这些映射都可以通过张量积的万有性来定义；而它们是同构则可以通过直接构造逆映射来验证。

> 事实上直和也可以用万有性质描述：对于任意的同态 $f:M\to P,g:N\to P$ 都存在唯一的同态 $h:M\oplus N\to P$ 使得 $f=h\circ p_1,g=h\circ p_2$。这里 $p_1:M\to M\oplus N,p_2:N\to M\oplus N$ 就是 $x\mapsto (x,0)$ 和 $y\mapsto (0,y)$ 咯，画成交换图：
>
> {% asset_img direct_sum.svg Direct sum %}

比方说我们构造同构 $\phi:(M\oplus N)\otimes P\to(M\otimes P)\oplus(N\otimes P)$ 如下：首先定义映射

$$\begin{aligned}
\bar\phi:(M\oplus N)\times P&\to (M\otimes P)\oplus(N\otimes P)\\
\bar\phi(x\oplus y,z)&=(x\otimes z)\oplus(y\otimes z)
\end{aligned}$$

（为了不混淆这里我们用 $x\oplus y$ 表示 $M\oplus N$ 中的元素）不难发现 $\bar\phi$ 是双线性的，因此引导同态 $\phi:(M\oplus N)\otimes P\to(M\otimes P)\oplus(N\otimes P)$，并且 $\phi((x\oplus y)\otimes z)=(x\otimes z)\oplus(y\otimes z)$。为了说明它是同构我们来构造其逆映射：定义 $\psi((x\otimes z)\oplus(y\otimes w))=(x\oplus0)\otimes z+(0\oplus y)\otimes w$（事实上就是 $p_1\otimes1$ 和 $p_2\otimes1$ 合起来定义的同态），直接验证即知 $\psi\circ\phi,\phi\circ\psi$ 都是单位映射。

接下来是书里的一个练习：

> 如果 $A,B$ 是两个环，$M$ 是 $A$-模，$P$ 是 $B$-模，并且 $N$ 是 $(A,B)$-双模（即既是 $A$-模又是 $B$-模，并且标量乘 $A$ 上元素和标量乘 $B$ 上元素的运算是交换的，$a(xb)=(ax)b$）。那么 $M\otimes_A N$ 自然的是 $B$-模（标量乘到 $N$ 上），$N\otimes_A P$ 自然是 $A$-模，并且
>
> $$(M\otimes_A N)\otimes_B P\cong M\otimes_A(N\otimes_B P)$$

书里面没说这个 $\cong$ 是怎么个同构，大概是说作为 $(A,B)$-bimodule 同构吧（等价的就是说作为 $A$-模或者作为 $B$-模都同构）。证明梗概当然就是努力说明 $(x\otimes_A y)\otimes_B z\mapsto x\otimes_A(y\otimes_B z)$ 这个映射是良定义的，这样对称的可以定义反过来的映射，并且显然它们互为逆映射。证明的时候要小心地看好每一步的同态是 $A$-模同态，$B$-模同态还是 $(A,B)$-双模同态，这里不再赘述~~实际上是我嫌麻烦.jpg~~。

### 标量限制和扩张

如果有环 $A,B$，环同态 $f:A\to B$，以及 $B$-模 $N$，那么 $N$ 可以看做 $A$-模：定义 $a\cdot x=f(a)x$ 即可。（事实上这个时候 $\Ker f\subseteq\mathop{\rm Ann}(N)$，所以其实相当于把标量乘限制到了 $B$ 的一个子环上）这个 $A$-模称为由 $N$ 进行**标量限制** *restriction of scalars* 得到的。特别的，$f$ 定义出了一种将 $B$ 看做 $A$-模的方式。

> 如果 $N$ 是有限生成 $B$-模而 $B$ 是有限生成 $A$-模，那么 $N$ 作为 $A$-模也是有限生成的。

如果 $y_1\dots y_n$ 生成 $N$（作为 $B$-模）而 $x_1\dots x_m$ 生成 $B$（作为 $A$-模），那么 $nm$ 个乘积 $x_iy_j$生成 $N$（作为 $A$-模）。

如果 $M$ 是一个 $A$-模，而 $B$ 由上述方式看做 $A$-模，那么 $M_B=B\otimes_A M$ 也是一个 $A$-模。而且如果定义 $b(b'\otimes x)=(bb')\otimes x\pod{b,b'\in B,x\in M}$，那么 $M_B$ 就承载了这样一个 $B$-模结构。这个过程被称为**标量扩张** *extension of scalars*。

> 如果 $M$ 是有限生成 $A$-模，那么 $M_B$ 是有限生成 $B$-模。

如果 $x_1,\dots,x_n$ 生成 $M$ 那么 $1\otimes x_1,\dots,1\otimes x_n$ 生成 $M_B$。

**注**：我在互联网的角落里找到的 restriction and extension of scalars 的翻译仅有“纯量限制”和“纯量的扩张”。想来“标量限制”和“标量扩张”的直译也很合理。

### 张量积的正合性

如果我们有一个 $A$-双线性映射 $f:M\times N\to P$，那么对每个 $x\in M$，$y\mapsto f(x,y)$ 是 $A$-线性的，也就是说是 $\Hom_A(N,P)$ 的一员。由于 $f$ 在 $x$ 上也是线性的，因此就对应了一个 $A$-线性映射 $M\to\Hom(N,P)$。反过来如果有一个 $A$-线性映射 $\phi:M\to\Hom_A(N,P)$，就可以以 $(x,y)\mapsto\phi(x)(y)$ 定义一个双线性映射。

这样， $M\times N\to P$ 双线性映射就和 $\Hom(M,\Hom(N,P))$ 一一对应。另一方面它又一一对应于 $\Hom(M\otimes N,P)$，因此我们有典范同态

$$\Hom(M\otimes N,P)\cong\Hom(M,\Hom(N,P))$$

（换句话说，$-\otimes N$ 和 $\Hom(N,-)$ 是一对伴随函子）

命题：若有正合列 $M'\to M\to M''\to0$，那么将其中所有模对 $N$ 取张量积并将同态对 $1$ （即 ${\rm id}_N$）取张量积，得到的序列 $M'\otimes N\to M\otimes N\to M''\otimes N\to0$ 仍然是正合的。

这个证明比较容易，利用到了 $M'\to M\to M''\to0$ 正合当且仅当对任何的 $P$ 都有 $\Hom(M',P)\to\Hom(M,P)\to\Hom(M'',P)\to 0$  正合的结论。

*事实上在范畴论里可以将上述命题推广到：任意函子的左伴随（如果存在）都是右正合的（反过来也是，任意函子的右伴随（如果存在）都是左正合的）。*

如果 $-\otimes N$ 的操作保持任意序列的正合性，那么就称 $N$ 是一个**平坦模** *flat module*。这个定义等价于：

- 所有短正合列对 $N$ 取张量积还是正合的（因为所有正合列可以拆分成短正合列）；
- 如果 $f:M'\to M$ 单射，那么 $f\otimes 1:M'\otimes N\to M\otimes N$ 单射。（等价于说 $0\to M'\to M$ 正合推出 $0\to M'\otimes N\to M\otimes N$ 正合，从而和 $-\otimes N$ 右正合的结论一起可以导出上一条）
- 如果 $f:M'\to M$ 单射并且 $M,M'$ 有限生成，那么 $f\otimes 1$ 单射。

最后一条推出倒数第二条是因为，如果在 $M\otimes N$ 中 $\sum f(x'_i)\otimes y_i=0$，那么存在 $M$ 的有限生成子模 $M_0$ 使得它在 $M_0\otimes N$ 中也为 $0$。然后取 $M'_0$ 为 $x'_i$ 生成的 $M'$ 的子模，考虑 $f:M'_0\to M_0$ 即可。

> 练习：如果 $M$ 是平坦 $A$-模，那么经过 $f:A\to B$ 的标量扩张得到的 $M_B:B\otimes_A M$ 是平坦 $A$-模。

这是因为 $N\otimes_B M_B=N\otimes_B(B\otimes_A M)\cong (N\otimes_B B)\otimes_A M\cong N\otimes_A M$。（用到了上一节的练习）。

### 代数

如果 $f:A\to B$  是环同构，那么如前面所说 $B$ 可以看做 $A$-模。因此它既有 $A$-模结构又有环结构，并且这两个结构是兼容的。这样，装备有 $A$-模结构的环 $B$ 称为一个 **$A$-代数** *$A$-algebra*。也就是说一个 $A$ 代数就是一个二元组 $(B,f)$，$B$ 是一个环而 $f:A\to B$ 是环同态。

> 如果 $A$ 是一个域 $K$，而 $B\neq0$，那么容易证明 $f$ 一定是单射。因此一个 $K$-代数事实上就是一个包含 $K$ 作为子环的环。
>
> 若 $A$ 是任意的一个环，那么自然（且唯一）地存在一个 $\mathbb{Z}\to A$ 的同态 $n\mapsto n.1$（$n$ 个 $1$ 相加），所以任意环都自动地是一个 $\mathbb{Z}$-代数。

如果 $f:A\to B,g:A\to C$，那么 $B,C$ 是两个 $A$-代数。一个 **$A$-代数同态** *$A$-algebra homomorphism* 是一个环同态 $h:B\to C$ 并且同时是 $A$-模同态。事实上相当于说 $h$ 是满足 $g=h\circ f$ 的环同态。

环同态 $f$ 被称为**有限** *finite* 的，并且 $B$ 称为**有限** $A$-代数，当且仅当 $B$ 作为 $A$-模是有限生成的。$f$ 被称为**有限型** *of finite type* 的，而 $B$ 称为**有限生成**$A$-代数，当且仅当存在一个有限子集 $\{x_1\dots,x_n\}\subseteq B$ 使得 $B$ 中每个元素可以写成关于它们的、系数在 $f(A)$ 中的多项式；换句话说存在一个从多元多项式环 $A[t_1,t_2,\dots,t_n]$ 到 $B$ 的*满*同态。（也就是说，$B$ 中每个元素可以用 $\{x_1,\dots,x_n\}\cup f(A)$ 通过有限次加减乘得出）

如果一个环 $A$ 作为 $\mathbb{Z}$-代数是有限生成的，那么称它为**有限生成**的环。

### 代数的张量积

如果 $B,C$ 是 $A$-代数，那么作为 $A$-模它们有一个张量积 $D=B\otimes_A C$。我们现在定义 $D$ 上的乘法使它成为 $A$-代数。

考虑 $B\times C\times B\times C\to D$ 的映射

$$(b,c,b',c')\mapsto(bb')\otimes(cc')$$

那么它是多重线性映射，因此导出映射 $B\otimes C\otimes B\otimes C\to D$，从而就得到一个映射 $D\otimes D\to D$，然后再返回去得到 $A$-双线性映射

$$\begin{aligned}
\mu:D\times D&\to D\\
\mu(b\otimes c,b'\otimes c')&=(bb')\otimes(cc')
\end{aligned}$$

把 $\mu$ 作为 $D$ 上的乘法即可。

由于 $\mu$ 是双线性的（满足分配律），并且乘法交换律以及结合律也是显然的，而且具有单位元 $1\otimes1$，所以它是一个交换环。为了让它成为环同态，只需要定义 $h:A\to D$，$h(x)=f(x)\otimes1=1\otimes g(x)$ 即可（*原书此处有笔误，写成了 $f(x)\otimes g(x)$*）

### 习题选（quan）做

这次是真的全做，除了最后五道题我不知道什么叫做 Tor functor。

{% asset_link Exercises2.tex 第二章习题选做-LaTeX 源码%}

{% asset_link Exercises2.pdf 第二章习题选做-PDF%}
