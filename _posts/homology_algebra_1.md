---
title: 同调代数学习笔记 (1)
categories:
  - Math
tags:
  - Homology Algebra
mathjax: true
date: 2020-06-16 18:11:53
---

看交换代数的时候书里提到了很多可以 generalize 的东西，于是好奇来看了交换代数。目前在看 Weibel 的 *An Introduction to Homology Algebra*。

这部分不打算详细写所有的知识点了，写一些过程中不太懂的东西好了。

<!-- more -->

这一篇可能知识点结构没那么清楚，凑活着看吧。以及没有涉及到链复形（），下一篇再写（我不是才看了 15 页吗.jpg）

## 定义

### 预加性范畴，加性范畴

范畴相关的基本定义我就不说了x。

一个 **Ab**-范畴（*预加性范畴*）是说任意两个对象（object，这个东西好像怎么翻译的都有）之间的态射集都有构成阿贝尔群，群运算用加法表示，单位元（零态射）用 $0$ 表示；并且这个加法还要和态射的复合分配：$h(f+f')g=hfg+hf'g$。

一个*加性范畴*首先一个预加性范畴，其次它要有零对象 $0$（同时是始对象和终对象，也就是说对任意 $A$，$\Hom(A,0)$ 和 $\Hom(0,A)$ 都有唯一元素（这里当然就一定是零态射）），并且对任意两个对象 $A,B$ 都要存在它们的乘积 $A\times B$。（这里的乘积既是 product $A\mathbin\Pi B$ 又是 coproduct $A\amalg B$）

在一个加性范畴 $\mathcal{A}$ 里面，态射 $f:B\to C$ 的一个 kernel 是一个态射 $i:A\to B$ 使得 $fi=0$，并且这是 universal 的（也就是说如果有另一个态射 $q:A'\to B,fq=0$，那么一定存在唯一的态射 $q':A'\to A$ 使得 $q=iq'$，也就是这样的交换图（换句话说 $i$ 就是 $0$ 和 $f$ 的等化子）

{% asset_img kernel.svg Kernel %}

把上面的图反过来的话，就得到了态射 $f$ 的 cokernel：即一个态射 $e:C\to D$ 使得 $ef=0$ 并且这个性质是 universal 的。

加性范畴中一个态射 $i$ 称为单的，如果 $ig=0\implies g=0$。一个态射 $e$ 称为满的，如果 $he=0\implies h=0$（普通范畴也可以定义单和满，即 $ig=ih\implies g=h$ 等）。接下来是原书中“容易看出”的习题：

> 一个态射的 kernel 必定是单的，cokernel 一定是满的。

这个证明很容易：如果 $i:A\to B$ 是 $f:B\to C$ 的 kernel 而 $g:P\to A,ig=0$。那么 $f(ig)=0$，从而存在*唯一*的态射 $g'$ 使得 $ig=ig'$。然而 $ig=i0$，因此必定有 $g=0$。对于 cokernel 也完全一样。

> 可能有些人会不理解：kernel 不应该是 $B$ 的子集吗？cokernel 不应该是 $C$ 的商集吗？然而范畴论中一个对象 $B$ 的“子对象”就是一个单射 $i:A\to B$；如果存在同构 $\phi:A_1\to A_2$ 使得 $i_1=i_2\phi$ 的话那么两个子对象 $i_1:A_1\to B$ 和 $i_2:A_2\to B$ 就会被看做“一样的”。如果我们把箭头反过来，单射改成满射，那么就定义了“商对象”。有时候我们也会把 kernel 和 cokernel 里面多出来的对象记做 $\ker f$ 和 $\coker f$。

### 阿贝尔范畴

一个*阿贝尔范畴*首先需要是一个加性范畴，其次：

- 每个态射都存在 kernel 和 cokernel；
- 每个单射都是它的 cokernel 的 kernel；
- 每个满射都是它的 kernel 的 cokernel。

典型的阿贝尔范畴就是 $R$-模范畴 $\bf mod$-$R$。

在一个阿贝尔范畴里面可以定义一个态射 $f:B\to C$的*像* $\im f=\ker(\coker f)$，并且可以把 $f$ 分解成一个满射 $B\to\im f$ 和一个单射 $\im f\to C$。证明如下：

{% asset_img image.svg Image %}

对于一个态射 $f:B\to C$ 首先取其 $\coker$（图中未标出态射名），然后取 $C\to\coker f$ 这个态射的 kernel，记做 $\im f$。$\im f\to C$ 的态射记做 $m$。由于 kernel 都是单的，所以 $m$ 是单的。接下来由于 $B\xrightarrow{f}C\to\coker f$ 复合为 $0$。所以根据 $m$ 的泛性质，存在一个态射 $v:B\to\im f$ 使得 $f=mv$。接下来只需要证明 $v$ 是满的。

如果 $t:\im f\to D$ 使得 $tv=0$，那么考虑 $t$ 和 $m$ 的推出，如图所示。由于 $t_1f=t_1mv=m_1tv=m_10=0$，所以存在一个 $\coker f$ 到推出的态射（图中虚线）使得 $t_1$ 是这样一个复合。但是 $m$ 是 $C\to\coker f$ 的核，从而我们知道 $t_1m=0$，也就是说 $m_1t=0$。

关于推出，下面的引理说明了阿贝尔范畴中单射的推出还是单射，也就是说 $m$ 是单射 $\implies m_1$ 是单射。从而我们就可以由 $m_1t=0$ 得到 $t=0$。

> 引理：在阿贝尔范畴中若 $i:A\to B$ 是单射，那么它沿 $f:A\to C$ 的推出 $i_1:C\to P$ 也是单射（$P$ 是推出的对象）。

{% fold 阿贝尔范畴中单态射的推出 %}

要证明这个引理，只需要考虑到 $i$ 和 $f$ 的推出可以看成由 $i$ 和 $-f$ 引导的 $[i,-f]:A\to B\times C$ 的 cokernel。即，若 $\phi:B\times C\to P$ 是 $[i,-f]$ 的 cokernel，那么 $i_1:B\to B\times C\to P$ 就可以看做 $i$ 的推出，而 $f_1:C\to B\times C\to P$ 可以看做 $f$ 的推出，如下图所示：

{% asset_img pushout_cokernel.svg Pushout and cokernel %}

将 $\coker[i,-f]$ 的 universal property 和 pushout 比较一下就可以知道这就是一个 pushout。(对任何的 $p:B\to F,q:C\to F$，如果 $pi=qf$，那么一定有 $[p,q][i,-f]=0$，因此 $[p,q]$ 经过 $\coker[i,-f]$)。然而既然 $i$ 是单的，那么容易证明 $[i,-f]$ 也是单的，因此 $[i,-f]$ 是它的 cokernel 的 kernel。

这样，如果 $g:D\to C$ 使得 $i_1g=0$，则 $D\xrightarrow{g}C\xrightarrow{p_2}B\times C\longrightarrow\coker[i,-f]$ 也为 $0$，因此存在 $g':D\to A$ 使得 $p_2g=[i,-f]g'$。再把这个等式投影到 $B$ 上，就有 $0=ig'$，因此 $g'=0$，从而 $g$ 也为 $0$。这样，我们就证明了 $i_1$ 是单态射。（取对偶的话，就是说满态射的拉回还是满态射）

{% fold_end %}

对于推出与拉回，下面还有一个引理：

若 $B\xleftarrow{g}P\xrightarrow{f}C$ 是 $B\xrightarrow{k}A\xleftarrow{h}C$ 的拉回，那么由 $g$ 诱导的态射 $\ker f\to\ker k$ 是同构。（对偶的，如果后者是前者的推出，那么由 $h$ 引导的态射 $\coker f\to\coker k$ 是同构）

{% fold Proof %}

记内射 $i:\ker f\to P,j:\ker k\to B$。首先我们解释一下 $g$ 怎样诱导 $\ker f\to\ker k$ 的态射：考虑到 $kgi=hfi=0$，因此存在态射 $e:\ker f\to\ker k$ 使得 $gi=je$。

{% asset_img pullback_isomorphism.svg Pullback %}

由于 $P$ 是拉回，利用拉回的泛性质，存在一个态射 $t:\ker k\to P$ 使得 $gt=i,ft=0$，因此存在 $u:\ker k\to\ker f$ 使得 $t=iu$，从而 $giue=gte=je=gi$，并且 $fiue=0=fi$，因此（根据拉回的态射的唯一性）$iue=i$，而 $i$ 是单射，从而 $ue=1_{\ker f}$。另一方面，$jeu=giu=gt=j$，$j$ 也是单射，所以 $eu=1_{\ker k}$。从而 $e$ 是同构。

{% fold_end %}

## 正合列，一些引理

阿贝尔范畴中由对象和态射组成的一个序列 $A\xrightarrow{f}B\xrightarrow{g}C$ 被称作*正合*的（或者说在 $B$ 处正合），当且仅当 $\im f=\ker g$。换句话说如果把 $f$ 分解开，那么序列 $A\xrightarrow{f_1}\im f\xrightarrow{f_2}B\xrightarrow{g}C$ 中 $D\xrightarrow{\phi}B\xrightarrow{g}C$ 为 $0$ 当且仅当 $\phi=f_2\phi'$。

对于正合列有一个等价的表述，描述如下：若 $A\xrightarrow{f}B\xrightarrow{g}C$ 满足 $gf=0$，那么该序列正合当且仅当：

- 对任意的 $h:D\to B$，若 $gh=0$，那么存在同态 $l:E\to A$ 以及满同态 $k:E\to D$ 使得 $hk=fl$。

{% fold Proof %}

记 $i:\ker g\to B$ 为典范内射，$p:A\to\im f$ 为典范单同态，$j:\im f\to\ker g$ 为典范内射（由于 $gf=0$，不难构造这样的同态），这样就把 $f$ 分解成了 $f=ijp$。

必要性。若序列正合，而 $h:D\to B$ 使得 $gh=0$，那么存在唯一的态射 $c:D\to\ker g$ 使得 $h=ic$。令 $E=A\times_{\ker g}D$ 为 $c$ 和 $jp:A\to\ker g$ 的拉回，$l:E\to A,k:E\to D$ 是典范投影，即 $ck=jpl$。那么 $hk=ick=ijpl=fl$。由于 $p$ 是满的而 $j$ 是同构（序列正合），所以 $jp$ 也是满的，从而 $k$ 作为 $jp$ 的拉回也是满同态。

{% asset_img exact_seq_1.svg Exact sequence %}

充分性。由于 $gi=0$，所以根据假设存在 $l:E\to A$ 以及满的 $k:E\to\ker g$ 使得 $ik=fl$，即 $ijpl=ik$。由于 $i$ 是单的，所以 $jpl=k$ 是满的，从而 $j$ 是满的。所以 $j$ 既单又满，于是是同构，也就是说 $\im f=\ker g$。

{% fold_end %}

### 蛇引理

若有如下交换图并且其中上下两行均为正合列，那么存在正合列 $\ker a\to\ker b\to \ker c\xrightarrow{\delta}\coker a\to\coker b\to\coker c$，其中 $\delta:\ker c\to\coker a$ （不严格地）定义为 $\delta=g_1^{-1}bf_2^{-1}$。

{% asset_img snake_lemma_2.svg Snake Lemma %}

{% fold Proof %}

其实一个简单的证明是，任何小的（对象构成集合的）阿贝尔范畴都到某个 $R$-模范畴有满、忠实、正合的嵌入，所以我们只需要对 $R$-模范畴证明上述引理（我之前的交换代数学习笔记 (2) 中证明了 $R$-模范畴中的情况）。但是这里我出于练习目的还是决定把它证一遍。

**第一部分：** $\delta$ 之外的序列及正合性

我们把 $\ker a\to A'$ 的态射写作 $i_a$，$A\to\coker a$ 的态射写作 $e_a$。由于 $bf_1i_a=g_1ai_a=0$，存在 $p_1:\ker a\to\ker b$ 使得 $f_1i_a=i_bp_1$。同样的，存在 $p_2:\ker b\to\ker c$ 使得 $f_2i_b=i_cp_2$。反过来也有 $q_1:\coker a\to\coker b,q_2:\coker b\to\coker c$：

{% asset_img snake_lemma_3.svg Snake Lemma %}

由于 $i_cp_2p_1=f_2f_1i_a=0$ 而 $i_c$ 是单的，所以 $p_2p_1=0$。如果 $h:D\to\ker b$ 满足 $p_2h=0$，那么 $f_2i_bh=0$。根据上一个引理，存在同态 $l:E\to A'$ 以及满同态 $k:E\to D$ 使得 $f_1l=i_bhk$，从而 $g_1al=bf_1l=bi_bhk=0$。由于 $g_1$ 是单同态，所以 $al=0$。因此存在 $m:E\to\ker a$ 使得 $l=i_am$，然后有 $i_bp_1m=f_1i_am=f_1l=i_bhk$。然而 $i_b$ 又是单的，所以 $p_1m=hk$。综上，对任意的 $h:D\to\ker b, p_2h=0$，我们找到了同态 $m:E\to\ker a$ 以及满同态 $k:E\to D$ 使得 $p_1m=hk$，因此根据前面的引理，$\ker a\to\ker b\to\ker c$ 是正合的。对偶地，$\coker a\to\coker b\to\coker c$ 也是正合的。如下图，事实上就是说我们证明 $al=0$，从而把 $l$ 拉回到 $\ker a$ 上去。

{% asset_img snake_lemma_4.svg Snake Lemma %}

**第二部分：** $\delta$ 的构造

不过重点在于 $\delta:\ker c\to\coker a$ 的构造。类似 $R$-模的情况，我们首先要把 $\ker c$ 拉回到 $B'$ 上，然后映射到 $B$ 里面，然后在拉回到 $A$ 里面。由此我们定义 $p=B'\times_{C'}\ker c$ 为 $f_2$ 与 $i_c$ 的拉回，$q=\coker a\amalg_AB$ 为 $e_a$ 和 $g_1$ 的推出。令 $\pi:p\to\ker c,\pi':p\to B'$ 为投影映射，$\iota:\coker a\to q,\iota':B\to q$ 为余投影映射，我们将找到一个（事实上是唯一的）同态 $\delta:\ker c\to\coker a$ 使得 $\iota\delta\pi=\iota'b\pi'$。（由于 $\iota$ 是单的，$\pi$ 是满的，显然 $\delta$ 至多只能有一个）

由于 $g_2b\pi'=ci_c\pi=0$，所以存在 $k:p\to A'$ 使得 $b\pi'=g_1k$（本来 $k$ 应该是到达 $\ker g_2$，但是 $g_1$ 是单射所以 $A$ 和 $\im g_1$ 是同构的）；记 $j:\ker\pi\to p$ 为内射，根据前面那个小的引理，可以知道 $\ker\pi$ 和 $\ker f_2$ 是同构的，也就是说和 $\im f_2$ 是同构的，从而导出一个同态 $h:A'\to\ker\pi$ 使得 $\pi'jh=f_1$。那么 $g_1kjh=b\pi'jk=bf_1=g_1a$。然而 $g_1$ 单，从而 $kjh=a$，因而 $e_akjh=e_aa=0$。由于 $h$ 满，所以 $e_akj=0$。

{% asset_img snake_lemma_5.svg Snake Lemma %}

根据拉回的定义最上面这一行是正合的，因此 $\pi$ 是 $j$ 的 cokernel，从而存在 $\delta:\ker c\to\coker a$ 使得 $\delta\pi=e_ak$。这样的话，$\iota\delta\pi=\iota e_ak=\iota'g_1k=\iota'b\pi'$，满足我们想要的条件。

**第三部分：** 序列在 $\ker c$ 和 $\coker a$ 处的正合性

根据拉回的定义，存在一个映射 $t:\ker b\to p$ 使得 $\pi t=p_2,\pi't=i_b$，从而 $\iota\delta p_2=\iota\delta\pi t=\iota' b\pi' t=\iota' bi_b=0$，而 $\iota$ 单从而 $\delta p_2=0$。

接下来如果 $d:F\to\ker c$ 使得 $\delta d=0$，那么由于 $p\xrightarrow{\pi}\ker c\to 0$ 正合，根据前面正合列的等价定义，存在满同态 $m:G\to F$ 和同态 $n:G\to p$ 使得 $\pi n=dm$。由于 $e_akn=\delta\pi n=\delta dm=0$，所以正合列 $A'\to A\to\coker a$ 又给出满同态 $\epsilon:H\to G$ 和同态 $\zeta:H\to A$ 使得 $kn\epsilon=a\zeta$。因此 $b\pi'n\epsilon=g_1kn\epsilon=g_1a\zeta=bf_1\zeta$，那么考虑 $\eta=\pi'n\epsilon-f_1\zeta:H\to B'$，就有 $b\eta=0$，从而存在一个 $\vartheta:H\to\ker b$ 使得 $\eta=i_b\vartheta$，因而 $i_cp_2\vartheta=f_2i_b\vartheta=f_2(\pi'n\epsilon-f_1\zeta)=i_c\pi n\epsilon=i_cdm\epsilon$。再次由于 $i_c$ 是单射，就有 $p_2\vartheta=dm\epsilon$。由于 $m\epsilon$ 是满的，上面关于正合列的等价定义告诉我们 $\ker b\xrightarrow{p_2}\ker c\xrightarrow{\delta}\coker a$ 是正合的。另外一半由对偶性即得。

{% fold_end %}

### 四引理

若有下列行正合的交换图

{% asset_img four_lemma.svg Four lemma %}

那么：

- 如果 $\alpha,\gamma$ 满而 $\delta$ 单，则 $\beta$ 满。
- 如果 $\beta,\delta$ 单而 $\alpha$ 满，则 $\beta$ 单。

{% fold Proof %}

只证明第一个，另一个是对偶。首先我们可以假设 $A\to B$ 是单的，否则可以把 $A$ 替换成 $\im(A\to B)$，同样的，可以假设 $C'\to D'$ 是满的。然后对

{% asset_img four_lemma_aux.svg Four lemma %}

这两个交换图分别应用蛇引理：对第一个交换图应用蛇引理后，由于 $D'\to D$ 是单射而 $C'\to C$ 是满射，所以最后一个 $\ker$ 和第二个 $\coker$ 都是 $0$，从而夹在他们中间的也只能是 $0$，i.e. $\ker(C'\to D')\to\ker(C\to D)$ 是单射。

再对第二个图应用蛇引理（注意到 $\im(B\to C)=\ker(C\to D)$），由于三个 $\coker$ 里面两边的都是 $0$，中间的也必定是 $0$，从而 $\beta:B'\to B$ 是满射。

{% fold_end %}

**推论（五引理）**： 如果把上面这个图每行变成五个，然后竖着的五个态射中最左边是满的，最右边是单的，第二个和第四个是同构，那么中间那个也是同构。

证明：四引理的两个部分分别用一次就可以知道中间那个既单又满，所以是同构。
