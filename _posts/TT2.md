---
title: 类型论学习笔记 (1) Dependent Type 与 Lambda Cube
categories:
  - Type_Theory
mathjax: true
date: 2019-10-13 21:39:26
---

这篇 Blog 里我会继续介绍 Typed Lambda Calculus 相关，包括 Denpendent Type 和 Lambda Cube 等，会稍微提一下 PTS；之后解释 Lambda-Cube 在 CH 同构意义下对应什么样的逻辑系统。

<!--more-->

首先我们回顾一下前一篇博客讲了什么。我们讲了类型系统的基本形式：term和type的定义、归约规则和推导规则；然后是 lambda calculus 的语法、$\lambda_\to$ 类型系统。

## 依赖类型

*注意以下两段里好多东西都是“猜出来”的，比如 $\forall$ 的这个用法只是形容一下我们想要它长成什么样。*

$\lambda_\to$ 类型系统的表达能力其实是很弱的（根据 CH 同构它甚至只同构于命题逻辑，连全称量词和存在量词都没有），比如说我们想要表达一个 $id$ 函数，定义为 $id_A=\lambda x\!:\!A.x$。那么我们自然想要一个 $\forall A. A\to A$ 这样的类型名，而不是要对每个类型都造一个 $id$ 出来，比如 $id_A, id_B,id_{A\to B\to A}$ 这种垃圾的东西。于是我们得到一个依赖于类型的类型（type depending on type），即这里 $\forall A. A\to A$ 是一个依赖于 $A$ 的类型。同时，$\forall A.\lambda x\!:\!A.x$ 也是一个依赖于 $A$ 这个类型的项（term depending on type）。

另外，假设我们有自然数类型 $\mathbb{N}$，我们想要造出一个定长的数组类型 ${\rm Vec}\ n\ A$，其中 $A$ 是类型而 $n:\mathbb{N}$。那么我们发现类型里面出现了非类型的项，这样就出现了一个依赖于项的类型（type depending on term）。甚至如果我们要有一个“往数组前面添一个项”的函数，那应该就会变成 $\forall n\!:\!\mathbb{N}.A\to {\rm Vec}\ n\ A\to{\rm Vec}\ (n+1)\ A$ 这种类型，于是我们甚至需要对一般的项（而非前例中的类型）做全称量词。

PS：细心的读者可以注意到这里的 $\lambda$ 都具体指出了变量的类型，因为对于更复杂的系统来说这一般是有用的。具体可以自行查阅资料（其实是我也不太懂区别在哪里）。

### 类型与项

上面的例子告诉我们，如果需要更加复杂的类型系统（各种 dependency），那么就应该模糊“类型”与“项”的差别，至少不要在语法上对它们区分。如果我们不在语法上区分这两者，那么 $id$ 就可以大概写成这样：$\lambda A\!:?.\lambda x\!:\!A.x$。这里 $A$ 是一个类型，但是我们也要给 $A$ 一个像“类型”一样的东西，于是我们记 $A:*$ 表示 $A$ 是一个类型。这里 $*$ 是一个**常量**（*constant*），直观上说它就是“类型的类型”（实际上这是不对的，因为 $*$ 本身不是类型，也就是说 $*:*$ 不成立）。

但是我们还有 type depending on type，那么就会有 $f(A)=A\to A$ 这样的函数。可以发现 $f$ 实际上是类型到类型的函数，所以我们理所应当的应该有 $f:*\to*$。这样我们还会有$*\to*\to*,(*\to*)\to*$ 等等无穷多种用 $*,\to$ 组成的式子，它们都被称为 **Kinds**（放弃翻译）。对于 Kinds，我们还有一个 constant，记做 $\square$，用 $k : \square$ 来表示 $k$ 是一个 Kind。我们不需要更多 constant 了，因为我们目前并不会搞出 $\square\to\square$ 这种函数来（至少我们不需要）。实际上我们可能还会有 $\mathbb{N}\to*$ 这种类型，直觉告诉我们它们也应该是 Kind。换句话说，如果一个函数类型最后的返回值是 $*$，我们就称它是一个 Kind。

如果 $f:k:\square$（意思是 $f:k$ 且 $k:\square$），那么就称 $f$ 是一个**构造子**（*constructor*）。

由于语法上不再区分类型和项，我们就以这种方式来区分：$A:*$ 时称 $A$ 是一个类型，而 $a:A:*$ 的时候称 $a$ 是一个项。注意这里大小写字母已经没什么区别了。

这样的话我们有两个常量 $*$ 和 $\square$，称它们为 **Sorts**（因此我放弃翻译——Kind 和 Sort 怎么区分啊（崩溃）），即一个表达式应该属于“哪一类”。比如当 $a:A:*$ 的时候，$a$ 就属于“项”这一类，$b:B:\square$ 的时候，$b$ 就属于“构造子”（类型）这一类。

### 依赖积

我们发现仅仅是 $\to$ 这种表示函数的方式已经不够用了：比方说给定一个 $n$，生成一个含 $n$ 个 $0$ 的定长数组，这显然应该用一个接收一个参数 $n:\mathbb{N}$ 返回一个 ${\rm Vec}\ n\ \mathbb{N}$ 的函数表示，但是它不能用 $\to$ 来表示：应该是 $\mathbb{N}\to{\rm Vec}\ ?\ \mathbb{N}$，里面的 $?$ 是不定的！

*PS：事实上我们并没有介绍怎么造出一个 $\rm Vec$ 来，这里也只是以此来举例子。*

我们思考这样一件事情：为了对每个 $n$ 给出一个 ${\rm Vec}\ n\ \mathbb{N}$，我们应该从 ${\rm Vec}\ 0\ \mathbb{N},{\rm Vec}\ 1\ \mathbb{N},{\rm Vec}\ 2\ \mathbb{N}\dots$ 中分别选出一个来（实际上，这一系列类型也会被称为 type family）。如果我们把每个类型 ${\rm Vec}\ n\ \mathbb{N}$ 当做一个集合，那么实际上就是在从每个集合中选出一个元素。这其实就是这些集合的一个笛卡尔积，即 $\prod_{n:\mathbb{N}}{\rm Vec}\ n\ \mathbb{N}$。

出于这样的考虑（实际上 $A\times B$ 在类型论里也确实表示类似笛卡尔积的东西，也就是 pair type），我们将这样的函数的类型写作 $\Pi n\!:\!\mathbb{N}.{\rm Vec}\ n\ \mathbb{N}$。更一般的，我们以
$$
\Pi x\!:\!A.B
$$
表示一个依赖函数类型，即**依赖积类型**（*dependent product type*），其中 $B$ 里面可能含有 $x$。它的解释是传入一个参数 $a$ 之后会返回一个 $B[x:=a]$ 的项，比如如果 $f:(\Pi n\!:\!\mathbb{N}.{\rm Vec}\ n\ \mathbb{N})$，那么 $(f\ 42) : ({\rm Vec}\ 42\ \mathbb{N})$ 这样。

这样的话，我们也不需要箭头类型 $A\to B$ 了，它相当于 $\Pi$ 类型的一个特例（$x$ 不在 $B$ 里出现）。也就是说，当 $\Pi x\!:\!A.B$ 中 $x$ 不在 $B$ 中 free 出现的时候，我们就可以将其简写为 $A\to B$。

## 形式化叙述——Lambda Cube

有了这些想法，我们就可以做一些形式化的叙述：

### “伪项”的定义

一个**伪项**（*pseudo-term*）$\cal T$ 定义如下：
$$
\begin{aligned}
{\cal T}&::=V\mid C\mid {\cal TT}\mid\lambda V\!:\!{\cal T}.{\cal T}\mid \Pi V\!:\!{\cal T}.{\cal T}\\
V&::=\text{a variable}\\
C&::=\text{a constant}
\end{aligned}
$$
注意我们也不再区分表示类型的变量和表示项的变量。存在两个常量，分别是 $*,\square$。

为什么叫做“伪项”呢？是因为现在我们不在语法上区分“项”和“类型”了，所以一个表达式有可能表达一个项，也有可能表达一个类型，也可能什么都不是，所以只能称为“伪项”。为了方便，下面不引起歧义的情况下我们仍然将其称为项。

### 归约规则

仍然是 $\alpha$-conversion 导出句法等价的项，而 $\beta$-reduction 用来归约。需要注意的是仍然只有 $\lambda$ 项可以进行 reduce，$\Pi$ 项本身表示一个类型，并不能进行归约（$(\Pi x\!:\!A.B)y$ 会表示啥？？？）

### 推导规则

接下来是全新的推导规则。由于对于 type depending on type、type depending on term、term depending on type 都分别有的系统支持有的系统不支持（因此会出现八种类型系统），我们先叙述通用的几条 inference rule。其中 $s$ 只能取 $*$ 或 $\square$。（$s$ 取自 sort 的首字母）

有一点需要注意的是，这里 $\Gamma$ 必须是有序的。不然的话就会出现这样的情况： 上下文里有 $a:*,x:a$，这时候我把 $a$ 给抽象掉了（$\lambda a$ 了），$x$ 的类型就不在上下文里了。有序的上下文被称为 **telescope**（望远镜？）。

**（只看这几条规则，读者可以尝试想一想它缺少了什么（实际上它几乎是没用的），然后再往下翻。）**（答案在下面的分割线之后）
$$
\begin{array}{cl}
\cfrac{}{\vdash*:\square}{}&\rm(axiom)\\
\cfrac{\Gamma\vdash A:s\quad x\notin\Gamma}{\Gamma,x:A\vdash x:A}&\rm(start\ rule)\\
\cfrac{\Gamma\vdash A:B\quad\Gamma\vdash C:s\quad x\notin\Gamma}{\Gamma,x:C\vdash A:B}&\rm(weakening\ rule)\\
\cfrac{\Gamma\vdash A:(\Pi x\!:\!U.V)\quad\Gamma\vdash B:U}{\Gamma\vdash (AB):V[x:=B]}&\rm(application\ rule)\\
\cfrac{\Gamma,x:U\vdash A:V\quad\Gamma\vdash (\Pi x\!:\!U.V):s}{\Gamma\vdash(\lambda x\!:\!U.A):(\Pi x\!:\!U.V)}&\rm(abstraction\ rule)\\
\cfrac{\Gamma\vdash A:B\quad\Gamma\vdash B':s\quad B=_\beta B'}{\Gamma\vdash A:B'}&\rm(conversion\ rule)
\end{array}
$$
我们来看看这和之前有什么不同：

1. 引入变量和进行抽象的时候都十分小心：只有 $A:*$ 或 $A:\square$ 的时候我们才允许有 $x:A$ 存在（当然，$A$ 本身就是 $\square$ 不算）。这就导致每个 well-type 的表达式要么是一个项，要么是一个 constructor（一个传统意义上的“类型”也算在其中），或者是一个 kind；
2. 上下文中后面的变量的类型可以依赖于前面的变量，反过来不可以；
3. weakening rule 的存在意义是把两边分开的推导过程的 $\Gamma$ 统一起来，因为 abs 和 app 都要求两边上下文相同。
4. 抽象的时候得到的是一个依赖积类型，而不是以前的箭头类型；同样 application 的时候也变成了处理依赖积；
5. 加入了一条 conversion rule。注意到里面有一条要求是 $\Gamma\vdash B':s$，这是为了防止类似 `if true then Int else 233` 这种东西出现嘛。至于为什么之前没有这条规定，是因为不允许三种 dependency 的话这条 rule 其实是可以证明的。

---

接下来就是补上缺少的 Rule：如果你没有翻得太快，并且有耐心自己想一想，你可能已经发现了：

我们根本*没有任何方法*来推断出 $(\Pi x\!:\!A.B):s$ 的结论，从而没法做抽象，于是这门语言就废了。为了补充上差的 Rule，我们需要考虑两点：第一点是它应该属于什么 sort，第二点是这样的规则对应什么直观的含义。

对于第一点，考虑 $\Pi x\!:\!A.B$ 类型需要具有什么样的 sort。比较直观的，如果 $B$ 是一个类型，那么 $\Pi x\!:\!A.B$ 也应该是一个类型。同样如果 $B$ 是一个 Kind，那么 $\Pi x\!:\!A.B$ 也应该是一个 Kind。所以说如果  $\Pi x\!:\!A.B$ 属于某个 sort，那么应该要和 $B$ 相同。

对于第二点，考虑如果 $A:s_1,B:s_2$，那么这个函数类型实际上是：我们拿到一个 $a:A$，就可以产出一个 $b:B$。从而如果 $s_1$ 是 $*$，那么 $a$ 就是一个 term；否则 $s_1$ 是 $\square$，那么 $a$ 就是一个 constructor，直观上看就是一个类似 type 的东西。对于 $s_2$ 和 $b$ 也一样。

那么如果 $s_1=s_2=*$，由于 $b$ （函数返回值）会依赖于 $a$（函数参数），那么就会出现 term depending on term。同样的，$s_1=*,s_2=\square$ 表示 type depending on term，$s_1=\square,s_2=*$ 就是 term depending on type，$s_1=s_2=\square$ 就是 type depending on type。

那么我们定义一个 $(s_1,s_2)$ rule 如下：
$$
\cfrac{\Gamma\vdash A:s_1\quad\Gamma,x:A\vdash B:s_2}{\Gamma\vdash (\Pi x\!:\!A.B):s_2}\qquad(s_1,s_2)\ \rm rule
$$
$(*,*)$ rule 是肯定要取的。剩下三种 rule 都可以独立的取或者不取，这样就会得到八个类型系统：
$$
\begin{array}{|l|cccc|}
&(*,*)&(\square,*)&(*,\square)&(\square,\square)\\\hline
\lambda\!\!\to&\checkmark&&&\\
\lambda2&\checkmark&\checkmark&&\\
\lambda\rm P&\checkmark&&\checkmark&\\
\lambda\rm P2&\checkmark&\checkmark&\checkmark&\\
\lambda\underline{\omega}&\checkmark&&&\checkmark\\
\lambda\omega&\checkmark&\checkmark&&\checkmark\\
\lambda\rm P\underline{\omega}&\checkmark&&\checkmark&\checkmark\\
\lambda\rm P\omega&\checkmark&\checkmark&\checkmark&\checkmark
\end{array}
$$
最后一个系统 $\lambda\rm P\omega$ 也被称为$\lambda\rm C$。（我猜是指 Complete？没有查过）

为什么叫做 Lambda-Cube？请看下图：

{% fi /images/lambda_cube.png, Lambda-Cube, Lambda-Cube %}

## PTS

用来造出这个 Lambda Cube 的方式可以推广出一类叫做**纯类型系统**（*pure type system*）的系统。

由于笔者本人对此没有深入了解，于是并不是太明白为什么需要这一类系统。这里抄录 *Lambda Calculi with  Types* 部分原书内容：

>One of the successes of the notion of PTS's is concerned with logic. In subsection 5.4 a cube of eight logical systems will be introduced that is in a close correspondence with the systems on the $\lambda$-cube. This result is the so called ‘propositions-as-types’ interpretation. It was observed by Berardi (1989) that the eight logical systems can each be descripted as a PTS in such a way that the propositions-as-types interpertation obtains a canonical simple form.
>
>Another reason for introducing PTSs is that several propsistions about the systems in the $\lambda$-cube are needed. The general setting of the PTSs makes it nicer to give the required proofs. Most results in this subsection are taken from Geuvers and Nederhog (1991) and also serve as a preparation for the strong normalization proof in Section 5.3.

大概是在更 general 的层面上去做 propositions-as-types interpretation（即“命题即类型”，CH 同构），以及更优美地（因为表述更加通用）给出一些关于 $\lambda$-cube 的命题，比如 strong normalization for the $\lambda$-cube，也就是说对于 $\lambda$-cube 中的任何一个系统，只要这个系统中 $\Gamma\vdash A:B$，那么 $A$ 和 $B$ 的任何 reduction 序列都是终止的（也就是说对 $A$ 和 $B$ 的任意顺序的规约必定停机）。

### 语法

一个 PTS 中的一个表达式，或者称为伪项，定义与 $\lambda$-cube 中相同：
$$
\begin{aligned}
{\cal T}&::=V\mid C\mid {\cal TT}\mid\lambda V\!:\!{\cal T}.{\cal T}\mid \Pi V\!:\!{\cal T}.{\cal T}\\
V&::=\text{a variable}\\
C&::=\text{a constant}
\end{aligned}
$$
区别在于常量中不一定会有 $*,\square$，取而代之的是一个集合 ${\cal S}\subseteq C$，称为 *sorts*。在 $\lambda$-cube 里，$\cal S=\{*,\square\}$。

### 类型推导规则

对于所有 *sort*，它们之间可能会有一些关系，比如 $\lambda$-cube 里的 $*:\square$。一个 PTS 会包含一个公理集合 $\cal A$，包含若干个形如
$$
c:s
$$
的形式，其中 $c\in C$ 而 $s\in\cal S$。注意 $c$ 不一定是一个 sort，我们也可以钦定一个类型之类的。

之后是跟 $\lambda$-cube 一样的通用的推导规则：
$$
\begin{array}{cl}
\cfrac{(c:s)\in\cal A}{c:s}{}&\rm(axiom)\\
\cfrac{\Gamma\vdash A:s\quad x\notin\Gamma}{\Gamma,x:A\vdash x:A}&\rm(start)\\
\cfrac{\Gamma\vdash A:B\quad\Gamma\vdash C:s\quad x\notin\Gamma}{\Gamma,x:C\vdash A:B}&\rm(weakening)\\
\cfrac{\Gamma\vdash A:(\Pi x\!:\!U.V)\quad\Gamma\vdash B:U}{\Gamma\vdash (AB):V[x:=B]}&\rm(application)\\
\cfrac{\Gamma,x:U\vdash A:V\quad\Gamma\vdash (\Pi x\!:\!U.V):s}{\Gamma\vdash(\lambda x\!:\!U.A):(\Pi x\!:\!U.V)}&\rm(abstraction)\\
\cfrac{\Gamma\vdash A:B\quad\Gamma\vdash B':s\quad B=_\beta B'}{\Gamma\vdash A:B'}&\rm(conversion)
\end{array}
$$
之后还需要有一些规则用来推断 $\Pi$ 类型的 inference。这些规则记在一个集合 $\cal R$ 中。$\cal R$ 包含若干个 sort 的三元组 $(s_1,s_2,s_3)$，表示当 $A:s_1,B:s_2$ 时 $(\Pi x\!:\!A.B):s_3$。
$$
\cfrac{\Gamma\vdash A:s_1\quad\Gamma,x:A\vdash B:s_2\quad(s_1,s_2,s_3)\in\cal R}{\Gamma\vdash (\Pi x\!:\!A.B):s_3}\qquad\rm(product)
$$
在 $\lambda$-cube 里 $s_2$ 都和 $s_3$ 相等，这时候我们也用 $(s_1,s_2)$ 来简记 $(s_1,s_2,s_2)$ rule。

一个 PTS 由这样三个集合 $\cal(S,A,R)$ 决定。比如说 $\lambda2$ 表示如下：
$$
\lambda2\quad
\begin{array}{|ll|}\hline
\cal S&*,\square\\
\cal A&*:\square\\
\cal R&(*,\square),(\square,*)\\\hline\end{array}
$$
PTS 有很多应用，比如一系列 AUTOMATH 的类型系统可以这样表示：
$$
\lambda{\rm AUT{-}68}\quad
\begin{array}{|ll|}\hline
\cal S&*,\square,\Delta\\
\cal A&*:\square\\
\cal R&(*,*),(*,\square,\Delta),(\square,*,\Delta)\\
&(\square,\square,\Delta),(*,\Delta,\Delta),(\square,\Delta,\Delta)\\\hline\end{array}
$$

## $\lambda$-cube 与逻辑系统

继续上一篇博客最后的话题，即将类型论对应于命题。

### 命题逻辑

上篇博客里我们提到 $\lambda\!\to$ 对应一阶命题逻辑 first-order proposition logic，那么接下来我们来考虑不同的 rule 会让我们得到什么。

#### 二阶命题逻辑

我们来考虑 $(\square,*)$ 这条 rule，即 term depending on type。出于简单性考虑，我们先来看仅包含 $(*,*),(\square,*)$ 的系统 $\lambda2$。

$(\square,*)$ 让这样的推断成为可能：（注意 $U\to V$ 就是说 $\Pi q\!:\!U.V$，但是 $q$ 用不到）
$$
\vdash(\Pi A\!:\!*.A\to A):*
$$
从而我们就可以得出这样的项：
$$
\vdash(\lambda A\!:\!*.\lambda x\!:\!A.x):(\Pi A\!:\!*.A\to A)
$$
可以发现这就对应于二阶命题逻辑：对命题本身进行量化。因为 $*:\square$，我们就可以全称量化一个 $*$，也就是一个命题，并且得到一个关于这个命题的命题。比如上例就对应于 $\forall A, (A\to A)$。所以说 $\lambda2$ 就对应了二阶命题逻辑 second-order proposition logic。

类似的，我们记 $\bot\equiv\Pi A\!:\!*.A$ 为表示“所有命题都成立”的命题，显然它是一个伪命题，于是 $A\to\bot$ 就可以表示 $A$ 的否定（看，我们不需要扩充类型系统了）！。

#### 弱高阶命题逻辑

上面的 rule 是比较简单的，那么接下来我们来考虑 $\lambda\underline\omega$，即包含 $(*,*),(\square,\square)$ 的系统。

对于 $(\square,\square)$，我们发现我们可以得到这样的推断：$(*\to*):\square$ 。这说明我们可以定义一个“类型构造子”，即用一个类型构造另一个类型的东西。对应到命题，我们就可以给出“命题到命题”的变换，比如我们可以推导出
$$
\begin{aligned}
\vdash&(\lambda a.a\to a):(*\to *):\square\\
\vdash&(\lambda a.\lambda b.a\to b):(*\to(*\to*)):\square
\end{aligned}
$$
这种东西，从而我们可以得到一个传入命题传出命题的东西。一般情况下这东西不会单独出现，因为我们无法对命题做全称量词，所以即使上下文里给出一个 $f:*\to *$ 也不能对 $f$ 做什么有意义的假设（比如上下文里放一个 $\forall A, A\to f(A)$ 这种假设），至多把一些项简写一下做一些 trivial 的东西。

所以说这个系统对应的逻辑系统中存在高阶的命题，但是不能对任何东西做量化，称为 weak higher-order proposition logic。

#### 高阶命题逻辑

我们把上面两者放在一起，就可以得到高阶命题逻辑 higher-order proposition logic。

这个逻辑系统就可以写出很多有意义的命题，比如
$$
\begin{aligned}
\vdash\Bigl(\lambda a\!:\!*.\lambda b\!:\!*.\bigl(\Pi c\!:\!*.(a\to b\to c)\to c\bigr)\Bigr):(*\to (*\to *))
\end{aligned}
$$
记上面的这个项为 $f$，可以发现 $f\ a\ b$就是逻辑与 $a\land b\equiv\forall C,(A\to B\to C)\to C$）。如果我们要证明 $a\land b\to a$，就可以考虑给出一个 $a\to b\to a$ 的函数 $t$ 然后使用 $(f\ a\ b)\ a\ t$ 来获得一个 $a$，因为根据定义，$a\land b:\Pi c\!:\!*.(a\to b\to c)\to c$，于是我们只需要先传一个参数表示我们想要得到的类型，再传一个参数表示怎么用 a 和 b 得到它。于是我们有
$$
a:*,b:*\vdash\Bigl(\lambda h\!:\!(a\land b).h\ a\ (\lambda x\!:\!a.\lambda y\!:\!b.x)\Bigr):(a\land b\to a)
$$

### 谓词逻辑

我们现在考虑 $\lambda\rm P$ 系统，即仅存在 type depending on term 的系统。这种情况下，我们之后会看到对 type 进行分类是有必要的：一些 type 代表命题 $Prop$，这种 type 的 term 代表命题的证明；而另一些 type 对应一个集合 $Set$，而这种 type 的 term 就代表集合的元素；第三种 type 表示集合间的映射 $Func$。

为了区别这三种 type，我们用 $*^s,*^p,*^f$ 分别表示 set, proposition, function。我们同样把 $\square$ 分成 $\square^s,\square^p$。注意没有 $\square^f$ 这种东西，因为我们想让 $*^f$ 都有一个准确的类型，比如 $(A\to (A\to B)):*^f$，而不能出现上下文里有一个 $x:*^f$ 这种东西（注意到如果 $*^f:\square^f$ 这种东西存在，根据 start rule 我们就可以引入一个类型为 $*^f$ 的类型变量）。

#### $\lambda$PRED

形式化的，我们定义一个新的 PTS，称作 $\lambda$PRED， 如下：
$$
\lambda{\rm PRED}\quad
\begin{array}{|ll|}\hline
\cal S&*^s,*^p,*^f,\square^s,\square^p\\
\cal A&*^s:\square^s,*^p:\square^p\\
\cal R&(*^p,*^p),(*^s,*^p),(*^s,\square^p)\\
&(*^s,*^s,*^f),(*^s,*^f)\\\hline\end{array}
$$
我们来看 $\cal R$ 里面的规则表示什么： $(*^p,*^p)$ 是用来表示命题的蕴含（箭头类型表示蕴含），如下：
$$
P:*^p,Q:*^p\vdash(P\to Q)\equiv(\Pi x\!:\!P.Q):*^p
$$
$(*^s,*^p)$ 用来对集合中的元素做全称量化：
$$
A:*^s,Q:*^p\vdash(\forall x\in A,Q)\equiv(\Pi x\!:\!A.Q):*^p
$$
$(*^s,\square^p)$ 是用来构造一阶谓词的，即定义在集合上的谓词：
$$
A:*^s\vdash (A\to*^p):\square^p
$$
这样就可以引入 $P:(A\to *^p)$ 的变量，从而有
$$
A:*^s,P:(A\to*^p),x:A\vdash (Px):*^p
$$
这样，$P$ 就是一个定义在 $A$ 上的谓词。

当然它也可以用来构造定义在多个变量上的谓词：
$$
A:*^s,B:*^s\vdash(A\to(B\to*^p)):\square^p
$$
剩下的两个规则就比较简单了：$(*^s,*^s,*^f)$ 用来构造两个集合间的映射：
$$
A:*^s,B:*^s\vdash(A\to B):*^f
$$
$(*^s,*^f)$ 用来构造多个参数的映射（柯里化）：
$$
A:*^s,B:*^s,C:*^s\vdash(A\to(B\to C)):*^f
$$
有了这些，我们就可以构造一些一阶谓词逻辑的命题，比如我们有两个集合 $A,B$，一个函数 $f:A\to B$，两个谓词 $P,Q$ 分别定义在 $A$ 和 $B$ 上，那么这样的命题
$$
\Bigl(\forall x\in A,Q(f(x))\to P(x)\Bigr)\to\Bigl(\bigl(\forall y\in B,Q(y)\bigr)\to\bigl(\forall x\in A,P(x)\bigr)\Bigr)
$$
可以（显然的）对应到这样的类型：
$$
\bigl(\Pi x\!:\!A,Q(fx)\to Px\bigr)\to\Bigl(\bigl(\Pi y\!:\!B.Qy\bigr)\to\bigl(\Pi x\!:\!A.Px\bigr)\Bigr)
$$
读者可以自行（trivial 但有一点点麻烦）证明
$$
\begin{aligned}
&A:*^s,B:*^s,f:(A\to B),P:(A\to*^p),Q:(B\to*^p)\\
\vdash&\left(\bigl(\Pi x\!:\!A,Q(fx)\to Px\bigr)\to\Bigl(\bigl(\Pi y\!:\!B.Qy\bigr)\to\bigl(\Pi x\!:\!A.Px\bigr)\Bigr)\right):*^p
\end{aligned}
$$
并且这个命题显然是正确的，这是因为对于任意一个 $x$，因为 $Q(f(x))$ 蕴含 $P(x)$，并且 $Q(y)$ 永真，那么显然 $P(x)$ 也永真。这可以对应这样的 Term：
$$
\lambda H_1.\lambda H_2.\lambda x.H_1\bigl(H_2(f x))\bigr)
$$
读者也可以自行证明
$$
\begin{aligned}
&A:*^s,B:*^s,f:(A\to B),P:(A\to*^p),Q:(B\to*^p)\\
\vdash&\Bigl(\lambda H_1\!:\color{blue}{\bigl(\Pi x\!:\!A,Q(fx)\to Px\bigr)}.\lambda H_2\!:\color{red}{\bigl(\Pi y\!:\!B.Qy\bigr)}.\lambda x\!:\!A.H_1\bigl(H_2(fx)\bigr)\Bigr)\\
:\,&\left(\color{blue}{\bigl(\Pi x\!:\!A,Q(fx)\to Px\bigr)}\to\Bigl(\color{red}{\bigl(\Pi y\!:\!B.Qy\bigr)}\to\bigl(\Pi x\!:\!A.Px\bigr)\Bigr)\right)
\end{aligned}
$$
实际上存在一种映射$[\cdot]$把高阶谓词语言 $L$ 映射到 $\lambda\rm PRED$ 的Term，使得对于每个 $L$ 中的公式 $A$，$A$ 为永真式当且仅当 $\exists x,\Gamma_A\vdash x:[A]$，其中 $\Gamma_A$ （不形式的说）就是 $A$ 中涉及到的集合、函数、谓词等的声明。

那么我们只需要把所有的 $*$ 和 $\square$ 右上角的上标（$s,p,f$）抹去，就可以得到 $\lambda\rm P$ system。这样我们就建立了 $\lambda\rm P$ 与first-order predicate logic，即一阶谓词逻辑的对应，可以发现这里面 $(*,\square)$ 的作用就是用来提供谓词。

#### 更高阶的谓词逻辑

对于 $\rm\lambda P2,\lambda P\underline\omega,\lambda P\omega$，我们可以对应地扩充 $\lambda\rm PRED$ 为 $\rm\lambda PRED2,\lambda PRED\underline\omega,\lambda PRED\omega$，方法是添加 $(\square^p,*^p)$ 和/或 $(\square^p,\square^p$) 进去。

在 $\lambda\rm PRED2$ 中我们可以对命题、谓词做全称量化，因此我们可以得到这种推断：
$$
\begin{aligned}
&A:*^s,B:*^s,f:(A\to B)\\
\vdash&\Bigl(\Pi P\!:\!(A\to*^p).\Pi Q\!:\!(B\to*^p).\\
&\Bigl(\Pi x\!:\!A,Q(fx)\to Px\Bigr)\to\Bigl(\bigl(\Pi y\!:\!B.Qy\bigr)\to\bigl(\Pi x\!:\!A.Px\bigr)\Bigr)\Bigr):*^p
\end{aligned}
$$
或者把 $A,B,f$ 都量化掉，也是可以的。它对应于二阶谓词逻辑 second-order predicate logic。

在 $\lambda\rm PRED\underline\omega$ 中我们不仅可以做命题间的变换，还可以做谓词间的变换，比如把一个定义在 $A$ 上的二元谓词 $P(a_1,a_2)$ 变成一元谓词，方法是把两个元素取成同一个。于是我们可以定义这样的项：
$$
A:*^s\vdash\Bigl(\Pi P\!:\!(A\to*^p).\lambda x\!:\!A.Pxx\Bigr):((A\to(A\to*^p))\to(A\to*^p))
$$
它对应于弱高阶谓词逻辑 weak higher-order predicate logic。

在 $\lambda\rm PRED\omega$ 里我们既可以对谓词全称量化，也可以对“谓词的变换”做全称量化。基本上在这个系统中已经可以做任意你想推断的东西，对应于高阶谓词逻辑 higher-order predicate logic。

## 总结

这篇 Blog 大概讲了 Lambda Cube 及其对应的逻辑系统云云。

之后的 Blog 的数学方面可能会削弱一点，取而代之的是一些更贴近于实际应用的东西，比如数据表示方式、一些命题如何表示；再之后大概就会讲我一开始就想要讲的东西，也就是怎么实现使用一个类型系统的语言（除去很trivial的parse和print，还有evaluation（即reduction）、type-checking等）。

