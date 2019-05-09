---
title: ZOJ4058-4070 Solutions for QingDao ACM 2018
categories:
  - Solutions
mathjax: true  
date: 2018-11-07 08:43:17
---

目前 H 不会写，K 不想写...

<!-- more -->

要看某道题的题解的话，左边有目录qwq。

下面每个题数据范围里没有标的东西都是不重要的东西（不影响复杂度且不影响做法）。

反正你也不可能看完我的 Blog 再做题就不看题面了。

# [A] Sequence and Sequence

## Description

给定序列 $P, Q$, （下标从 1 开始）。

$$
P=\{1,1,2,2,2,3,3,3,3,4,4,4,4,4,5,5,5,5,5,5,\dots\}
$$

$$
Q(n)=\begin{cases}1&n=1\\Q(n-1)+Q(P_n)&n>1\end{cases}
$$

给定 $n$，求 $Q(n)$ 。

数组组数 $T\leqslant10000$，$n\leqslant10^{40}$。5s/512MB

## Solution

首先我们可以发现 $P(n)\geq i\iff n\geq\frac{i(i+1)}2$ 。

补充定义 $Q(0)=0$。因为 $Q(n)$ 的差分为 $Q(P(n))$，容易知道

$$
Q(n)=\sum_{i=1}^nQ(P(i))\label{A1}\tag{A1}
$$

将 $\eqref{A1}$ 代入其本身，我们可以得到

$$
\begin{aligned}
Q(n)&=\sum_{i=1}^nQ(P(i))\\
&=\sum_{i=1}^n\left(\sum_{j=1}^{P(i)}Q(P(j))\right)\\
&=\sum_{j=1}^{P(n)}Q(P(j))\sum_{i=\frac{j(j+1)}2}^n1\\
&=n\sum_{j=1}^{P(n)}Q(P(j))-\sum_{j=1}^{P(n)}Q(P(j))\left(\frac{j(j+1)}2-1\right)\\
\end{aligned}
$$

我们发现求和上指标都从 $n$ 变成了 $P(n)$，但是求和的时候可能会多乘一个关于 $j$ 的多项式。

用完全相同的方式我们可以推出


$$
\sum_{i=1}^nQ(P(i))F(i)=S_F(n)\sum_{j=1}^{P(n)}Q(P(j))-\sum_{j=1}^{P(n)}Q(P(j))S_F\left(\frac{j(j+1)}2-1\right)\label{A2}\tag{A2}
$$

式中 $S_F(n)$ 表示 $F$ 的前缀和，即 $S_F(n)=\sum_{i=0}^nF(i)$ （从0从1开始无所谓了）。

我们用 $f_{n,F}$ 表示式 $\eqref{A2}$ 的左边，$T_F(n)=S_F(n(n+1)/2-1)$ ，则我们有 $f_{n,F}=S_F(n)f_{P(n),1}-f_{P(n),T_F}$ 。

我们要求的 $Q(n)=f_{n,1}$ 。如果我们用 $\eqref{A2}$ 展开式子，那么就变成要求 $f_{P(n),1},f_{P(n),T_1}$ ，再展开一次就要求 $f_{P(P(n)),1},f_{P(P(n)), T_1},f_{P(P(n)),T_{T_1}}$，以此类推。

由此我们定义 $F_0(n)=1, F_{i+1}(n)=T_{F_i}(n)$ 为在 $f(n)=1$ 上迭代 $i$ 次 $T$ 得到的多项式；$n_0=n,n_{i+1}=P(n_i)$ 表示在 $n$ 上迭代 $i$ 次 $P$ 得到的数；

再令 $g_{i,k}=f_{n_i,F_k}$，则

$$
g_{i,k}=S_{F_k}(n_i)g_{i+1,0}-g_{i+1,k+1}\label{A3}\tag{A3}
$$

可以验证 $P(P(P(P(10^{40}))))=605$，所以可以预处理出 $F_0\dots F_4$、所有 $n\leq605,k\leq4$ 的 $f_{n,F_k}$ 的值，查询的时候直接用 $\eqref{A3}$ 不断展开即可。

## Code

代码中 $Poly$ 为多项式，表示 $f(x)=(A_0+A_1x+A_2x^2+\dots+A_{deg}x^{deg})\div den$。

```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <cmath>

typedef long long LL;

const int N = 610;

LL gcd(LL a, LL b) {
  while (b) std::swap(b, a %= b);
  return a;
}

struct Poly {
  LL A[100], den;
  int deg;

  Poly(int v = 0) {
    deg = 0;
    den = 1;
    memset(A, 0, sizeof A);
    A[0] = v;
  }

  Poly &reduce() {
    while (deg && !A[deg]) --deg;
    LL g = den;
    for (int i = 0; i <= deg && g > 1; ++i) g = gcd(g, A[i]);
    if (g > 1) {
      den /= g;
      for (int i = 0; i <= deg; ++i) A[i] /= g;
    }
    return *this;
  }

  friend Poly operator*(const Poly &a, const Poly &b) {
    Poly ans; ans.deg = a.deg + b.deg; ans.den = a.den * b.den;
    for (int i = 0; i <= a.deg; ++i)
      for (int j = 0; j <= b.deg; ++j)
        ans.A[i + j] += a.A[i] * b.A[j];
    return ans.reduce();
  }

  friend Poly operator*(const Poly &a, LL b) {
    Poly ans; ans.deg = a.deg; ans.den = a.den;
    for (int i = 0; i <= ans.deg; ++i) ans.A[i] += a.A[i] * b;
    return ans.reduce();
  }

  friend Poly operator/(const Poly &a, LL b) {
    Poly ans; ans.deg = a.deg; ans.den = a.den * b;
    for (int i = 0; i <= ans.deg; ++i) ans.A[i] = a.A[i];
    return ans.reduce();
  }

  friend Poly operator+(const Poly &a, const Poly &b) {
    Poly ans; ans.deg = std::max(a.deg, b.deg);
    ans.den = a.den * b.den / gcd(a.den, b.den);
    LL xa = ans.den / a.den, xb = ans.den / b.den;
    for (int i = 0; i <= ans.deg; ++i)
      ans.A[i] = a.A[i] * xa + b.A[i] * xb;
    return ans.reduce();
  }

  friend Poly operator-(const Poly &a, const Poly &b) {
    Poly ans; ans.deg = std::max(a.deg, b.deg);
    ans.den = a.den * b.den / gcd(a.den, b.den);
    LL xa = ans.den / a.den, xb = ans.den / b.den;
    for (int i = 0; i <= ans.deg; ++i)
      ans.A[i] = a.A[i] * xa - b.A[i] * xb;
    return ans.reduce();
  }

  template <typename T>
  T operator()(const T &x) const {
    T ans(0), ans1(0);
    for (int i = deg; i >= 0; --i) {
      ans = ans * x;
      if (A[i] > 0) ans = ans + A[i];
    }
    for (int i = deg; i >= 0; --i) {
      ans1 = ans1 * x;
      if (A[i] < 0) ans1 = ans1 + (-A[i]);
    }
    ans = (ans - ans1) / den;
    return ans;
  }

};

const int W = 1000000;

struct Integer {
  LL A[15], len;
  Integer(LL v = 0) {
    len = 0;
    memset(A, 0, sizeof A);
    while (v) A[len++] = v % W, v /= W;
  }

  friend Integer operator+(const Integer &a, const Integer &b) {
    Integer ans;
    ans.len = std::max(a.len, b.len) + 1;
    for (int i = 0, t = 0; i < ans.len; ++i) {
      t = (ans.A[i] = a.A[i] + b.A[i] + t) / W;
      ans.A[i] %= W;
    }
    while (ans.len && !ans.A[ans.len - 1]) --ans.len;
    return ans;
  }

  friend Integer operator-(const Integer &a, const Integer &b) {
    Integer ans;
    ans.len = a.len;
    for (int i = 0, t = 0; i < ans.len; ++i) {
      t = (ans.A[i] = a.A[i] - b.A[i] - t) < 0;
      if (t) ans.A[i] += W;
    }
    while (ans.len && !ans.A[ans.len - 1]) --ans.len;
    return ans;
  }

  friend Integer operator*(const Integer &a, const Integer &b) {
    Integer ans;
    ans.len = a.len + b.len;
    for (int i = 0; i < a.len; ++i)
      for (int j = 0; j < b.len; ++j)
        ans.A[i + j] += a.A[i] * b.A[j];
    for (int i = 0; i < ans.len; ++i) {
      ans.A[i + 1] += ans.A[i] / W;
      ans.A[i] %= W;
    }
    while (ans.len && !ans.A[ans.len - 1]) --ans.len;
    return ans;
  }

  friend Integer operator/(const Integer &a, LL b) {
    Integer ans;
    ans.len = a.len;
    LL t = 0;
    for (int i = a.len - 1; i >= 0; --i) {
      LL k = a.A[i] + t * W;
      ans.A[i] = k / b;
      t = k % b;
    }
    while (ans.len && !ans.A[ans.len - 1]) --ans.len;
    return ans;
  }

  friend bool operator!=(const Integer &a, const Integer &b) {
    if (a.len != b.len) return true;
    for (int i = 0; i < a.len; ++i)
      if (a.A[i] != b.A[i]) return true;
    return false;
  }

  friend bool operator<=(const Integer &a, const Integer &b) {
    if (a.len != b.len) return a.len < b.len;
    for (int i = a.len; i >= 0; --i)
      if (a.A[i] != b.A[i])
        return a.A[i] <= b.A[i];
    return true;
  }

  void put(char c) const {
    if (!len) putchar('0'), putchar(c);
    else {
      printf("%lld", A[len - 1]);
      for (int i = len - 2; i >= 0; --i)
        printf("%06lld", A[i]);
      putchar(c);
    }
  }
};

Poly X, K, pX1[35], Sk[35], F[5], S[5];

Poly getS(const Poly &a) {
  Poly ans(0);
  for (int i = 0; i <= a.deg; ++i)
    ans = ans + Sk[i] * a.A[i];
  ans = ans / a.den;
  return ans;
}

void InitF() {
  X.deg = 1;
  X.A[1] = 1;
  pX1[0] = 1;
  for (int i = 1; i < 35; ++i) pX1[i] = pX1[i - 1] * (X + 1);
  for (int i = 0; i < 34; ++i) {
    Sk[i] = pX1[i + 1];
    for (int j = 0, C = 1; j < i; ++j) {
      Sk[i] = Sk[i] - Sk[j] * C;
      C = C * (i + 1 - j) / (j + 1);
    }
    Sk[i] = Sk[i] / (i + 1);
  }
  K = X * (X + 1) / 2 - 1;
  S[0] = getS(F[0] = 1);
  for (int i = 1; i < 5; ++i)
    S[i] = getS(F[i] = S[i - 1](K).reduce());
}

int P[N];
LL Q[N];
Integer SS[5][N];

void InitPQ() {
  for (int i = 1; i * (i + 1) / 2 < N; ++i)
    for (int j = i * (i + 1) / 2; j < N && j < (i + 1) * (i + 2) / 2; ++j)
      P[j] = i;
  Q[1] = 1;
  for (int i = 2; i < N; ++i)
    Q[i] = Q[i - 1] + Q[P[i]];
  for (int i = 0; i < 5; ++i) {
    SS[i][0] = 0;
    for (int j = 1; j < N; ++j)
      SS[i][j] = SS[i][j - 1] + Q[P[j]] * F[i]((Integer)j);
  }
}

inline LL k(LL x) { return x * (x + 1) / 2; }

Integer getP(Integer n) {
  // x^2+x-2n<=0
  // x<=(sqrt(1+8n)-1)/2
  Integer l = 0, r = n, q = n * 2, mid;
  while (l != r) {
    mid = (l + r + 1) / 2;
    if (mid * (mid + 1) <= q) l = mid;
    else r = mid - 1;
  }
  return l;
}

Integer mem[5][5];
bool vis[5][5];
Integer nn[5];

Integer calc(int i, int k = 0) {
  const Integer &n = nn[i];
  if (n.len <= 1 && n.A[0] < N) return SS[k][n.A[0]];
  if (vis[i][k]) return mem[i][k];
  vis[i][k] = true;
  return mem[i][k] = S[k](n) * calc(i + 1, 0) - calc(i + 1, k + 1);
}

char s[100];

int main() {
  InitF();
  InitPQ();
  int T;
  scanf("%d", &T);
  while (T--) {
    scanf("%s", s);
    nn[0] = 0;
    for (int i = 0; s[i]; ++i)
      nn[0] = nn[0] * 10 + (s[i] - '0');
    for (int i = 1; i < 5; ++i) nn[i] = getP(nn[i - 1]);
    memset(vis, 0, sizeof vis);
    calc(0).put('\n');
  }
}
```

# [B] Kawa Exam

## Description

给定一张无向图，每个点有一个目标颜色。现在对于每条边求出：

* 删除这条边之后，给每个点染色，要求单个联通块内颜色相同，且满足目标颜色的点最多，求最多多少个点满足目标颜色。

$n,m\leqslant10^5$，$\sum n,\sum m\leqslant10^6$。2s/64MB

## Solution

显然删边之后每个连通块取其众数；而且只有割边删掉答案才会变。

所以边双连通分量缩成一个点之后只需要在树上做这个操作，容易发现要求的其实就是子树众数和子树外众数。

可以直接 dsu on tree 解决，复杂度 $O(n\log n+m)$。附：[Tutorial (dsu on tree)](http://codeforces.com/blog/entry/44351)

## Code

```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <vector>

const int N = 100050;
typedef std::vector<int> VI;

namespace Tree {
VI C[N];
int n, pre[N], nxt[N * 2], to[N * 2], cnt;
int siz[N], wson[N], ans1[N], ans2[N], root[N];
int sumAns;

inline void addEdge(int x, int y) {
  nxt[cnt] = pre[x];
  to[pre[x] = cnt++] = y;
  nxt[cnt] = pre[y];
  to[pre[y] = cnt++] = x;
}

void dfs0(int x, int f) {
  root[x] = f ? root[f] : x;
  siz[x] = C[x].size();
  wson[x] = 0;
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f) {
      dfs0(to[i], x);
      siz[x] += siz[to[i]];
      if (siz[to[i]] > siz[wson[x]]) wson[x] = to[i];
    }
}

struct SomeDS {
  int A[N], num[N], maxv;

  inline void add1(int x) { --num[A[x]]; ++num[++A[x]]; maxv += num[maxv + 1] > 0; }
  inline void minus1(int x) { --num[A[x]]; ++num[--A[x]]; maxv -= num[maxv] == 0; }
} D;

void forall(int x, int f, void (SomeDS::*fun)(int)) {
  for (VI::iterator i = C[x].begin(); i != C[x].end(); ++i)
    (D.*fun)(*i);
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f) forall(to[i], x, fun);
}

void dfs1(int x, int f, bool keep) {
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f && to[i] != wson[x])
      dfs1(to[i], x, false);
  if (wson[x]) dfs1(wson[x], x, true);
  for (VI::iterator i = C[x].begin(); i != C[x].end(); ++i)
    D.add1(*i);
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f && to[i] != wson[x])
      forall(to[i], x, &SomeDS::add1);
  ans1[x] = D.maxv;
  if (!keep)
    forall(x, f, &SomeDS::minus1);
}

void dfs2(int x, int f, bool keep) {
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f && to[i] != wson[x])
      dfs2(to[i], x, false);
  if (wson[x]) dfs2(wson[x], x, true);
  for (VI::iterator i = C[x].begin(); i != C[x].end(); ++i)
    D.minus1(*i);
  for (int i = pre[x]; ~i; i = nxt[i])
    if (to[i] != f && to[i] != wson[x])
      forall(to[i], x, &SomeDS::minus1);
  ans2[x] = D.maxv;
  if (!keep)
    forall(x, f, &SomeDS::add1);
}

void Init(int nn) {
  n = nn;
  for (int i = 1; i <= n; ++i) {
    pre[i] = -1;
    C[i].clear();
    root[i] = 0;
  }
  cnt = 0;
}

void Solve() {
  sumAns = 0;
  for (int i = 1; i <= n; ++i)
    if (!root[i]) {
      dfs0(i, 0);
      dfs1(i, 0, true);
      dfs2(i, 0, true);
      sumAns += ans1[i];
    }
}

inline int getAns(int a, int b) {
  if (a == b) return sumAns;
  if (siz[a] < siz[b]) return sumAns - ans1[root[a]] + ans1[a] + ans2[a];
  else                 return sumAns - ans1[root[b]] + ans1[b] + ans2[b];
}

};

namespace Graph {
int n, pre[N], nxt[N * 2], to[N * 2], cnt;
int color[N], dfn[N], bcc[N], cnt2, cnt3;
int stack[N], ans[N], top;

inline void addEdge(int x, int y) {
  nxt[cnt] = pre[x];
  to[pre[x] = cnt++] = y;
  nxt[cnt] = pre[y];
  to[pre[y] = cnt++] = x;
}

void Init(int nn) {
  n = nn;
  for (int i = 1; i <= n; ++i) pre[i] = -1, dfn[i] = 0;
  cnt = cnt2 = cnt3 = top = 0;
}

int dfs(int x, int f) {
  stack[top++] = x;
  int lowx = dfn[x] = ++cnt2;
  for (int i = pre[x]; ~i; i = nxt[i]) if ((i ^ 1) != f) {
    if (!dfn[to[i]]) lowx = std::min(lowx, dfs(to[i], i));
    else lowx = std::min(lowx, dfn[to[i]]);
  }
  if (lowx == dfn[x]) {
    bcc[x] = ++cnt3;
    while (stack[--top] != x) bcc[stack[top]] = cnt3;
  }
  return lowx;
}

void Solve() {
  for (int i = 1; i <= n; ++i) if (!dfn[i]) dfs(i, -1);
  Tree::Init(cnt3);
  for (int i = 1; i <= n; ++i)
    Tree::C[bcc[i]].push_back(color[i]);
  for (int i = 0; i < cnt; i += 2) {
    int x = to[i], y = to[i ^ 1];
    if (bcc[x] != bcc[y]) Tree::addEdge(bcc[x], bcc[y]);
  }
  Tree::Solve();
  for (int i = 0; i < cnt; i += 2)
    ans[i >> 1] = Tree::getAns(bcc[to[i]], bcc[to[i ^ 1]]);
}

};

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    int n, m;
    scanf("%d%d", &n, &m);
    Graph::Init(n);
    for (int i = 1; i <= n; ++i) scanf("%d", &Graph::color[i]);
    for (int i = 0; i < m; ++i) {
      int x, y;
      scanf("%d%d", &x, &y);
      Graph::addEdge(x, y);
    }
    Graph::Solve();
    for (int i = 0; i < m; ++i)
      printf("%d%c", Graph::ans[i], " \n"[i == m - 1]);
  }
}
```

# [C] Flippy Sequence

## Description

给出两个长为 $n$ 的 01 串，你可以（且必须）在两个 01 串中各选择一个区间反转（$0\to1,1\to0$），问有多少种方案使操作后两个串相同。

$n\leqslant10^6,\sum n\leqslant10^7$。1s/64MB

## Solution

先把两个串异或起来（假设结果串是 $C$），相当于选择两个（有顺序）的区间使得这两个区间异或起来得到两个串的异或。

当 $C$ 中只含 0 的时候，任意两个相同的区间都可以，方案数是 $\frac{n(n+1)}2$；

当 $C$ 中有一段连续的 1 而其他都是 0 的时候，手玩一下就可以知道恰好有 $2n-2$ 种方案（注意两个区间有顺序）；

当 $C$ 中恰有两段连续的 1 的时候，方案数是 $6$。

当 $C$ 中有三段或以上连续的 1 的时候，显然方案数是 $0$。

## Code

```cpp
#include <algorithm>
#include <cstdio>

const int N = 1000050;

char s[N], t[N];

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    int n;
    scanf("%d%s%s", &n, s, t);
    int k = 0, l = 0;
    for (int i = 0; i < n; ++i) {
      int x = s[i] != t[i];
      if (x && !l) ++k;
      l = x;
    }
    if (k == 0) printf("%lld\n", (long long)n * (n + 1) / 2);
    else if (k == 1) printf("%d\n", 2 * n - 2);
    else if (k == 2) printf("%d\n", 6);
    else printf("%d\n", 0);
  }
  return 0;
}
```

# [D] Magic Multiplication

## Description

对于两个正整数 $A, B$，若 $A$ 的各个数位（从高到低，无前导零）为 $a_1,a_2,\dots,a_n$，$B$ 的各个数位为 $b_1,b_2,\dots,b_m$，则定义

$$
A\otimes B=a_1b_1+a_1b_2+a_1b_3+\dots+a_1b_m+a_2b_1+\dots+a_2b_m+\dots+a_nb_m
$$

但是式子中的 $+$ 表示拼接操作，比如

$12+34=1234,0+2=02,23\otimes45=8101215,20\otimes20=4000$。

给出 $n,m$ 及 $C=A\otimes B$，求 $A$ 和 $B$。若有多解输出 $A$ 最小的，还有多解则输出 $B$ 最小的。

$n,m,|C|\leqslant2\times10^5,\sum|C|\leqslant2\times10^6$。1s/64MB

## Solution

假设我们知道了 $a_1$。那么如果 $c_1<a_1$，显然 $c_1$ 和 $c_2$ 两位拼起来才是 $a_1\times b_1$，由此可以求出 $b_1$。

否则 $c_1\leq a_1$，而如果 $a_1b_1$ 进了位也不会进到 $a_1$ 那么多，所以 $a_1b_1=c_1$，也可以求出 $b_1$。

按照这种方式继续求 $b_2\dots b_m$（注意如果某一次开头一位是 $0$ 的话 $b_i$ 也一定是 $0$），然后反过来用 $b_1$ 求 $a_2,a_3\dots a_n$，在中间的部分直接用 $a_ib_j$ 去跟当前的位置比较即可。

所以可以枚举 $a_1=1\dots9$ 并找到第一个合法的（这样字典序最小）即可。

## Code

```cpp
#include <algorithm>
#include <cstdio>

const int N = 200050;

char C[N];

int get(int t, char *&s) {
  if (!*s) return -1;
  int x = *s - '0'; ++s;
  if (x == 0) return 0;
  else if (x >= t) return x % t == 0 ? x / t : -1;
  else {
    x = x * 10 + *s - '0'; ++s;
    return x % t == 0 ? x / t : -1;
  }
}

int n, m, A[N], B[N];

bool Check(int a0) {
  A[0] = a0;
  char *s = C;
  for (int i = 0; i < m; ++i)
    if ((B[i] = get(a0, s)) == -1) return false;
  for (int i = 1; i < n; ++i) {
    if ((A[i] = get(B[0], s)) == -1) return false;
    for (int j = 1; j < m; ++j) {
      int k = A[i] * B[j];
      if (k < 10) {
        if (k + '0' != *s) return false;
        ++s;
      } else {
        if (k / 10 + '0' != *s) return false;
        ++s;
        if (k % 10 + '0' != *s) return false;
        ++s;
      }
    }
  }
  if (*s) return false;
  return true;
}

void Work() {
  scanf("%d%d%s", &n, &m, C);
  for (int i = 1; i < 10; ++i) if (Check(i)) {
    for (int j = 0; j < n; ++j) putchar(A[j] + '0');
    putchar(' ');
    for (int j = 0; j < m; ++j) putchar(B[j] + '0');
    putchar('\n');
    return;
  }
  puts("Impossible");
}

int main() {
  int T;
  scanf("%d", &T);
  while (T--) Work();
  return 0;
}
```

# [E] Plants vs. Zombies

## Description

直线上有 $n$ 个植物，第 $i$ 棵植物坐标为 $i$，浇一次水会长 $a_i$ 高。

你最开始在 $0$ 点，执行 $m$ 次操作。每次操作必须往左或右走一步并给走到的那棵植物（如果有）浇一次水。

最大化 $m$ 次操作后最矮的植物的高度。

$n\leqslant10^5,m\leqslant10^{12},\sum b\leqslant10^6$。2s/64MB

## Solution

先二分一个高度并求出每个位置至少浇水多少次。

首先我们证明，每种结尾在 $n$ 或者 $n-1$ 的方案都能换成（步数不变，每棵植物浇水次数也不变）

$$
1\to2\to1\to\dots\to2\to3\to2\to\dots\to3\to4\to\dots\to n-1\to n\to n-1
$$

的形式（如果停在 $n$ ，那最后就少走一步 $n-1\to n$），即“左右横跳”（雾

这个结论是显然的：既然从 $1$ 走到了 $n$ 或 $n-1$ 那么把走过的（有向）边拿出来就是一条欧拉路。不停地贪心“若可以往左走就往左走，否则往右走”一定能走成刚才那种形式。

接下来的结论是：任何方案都可以换成（同上）从 $n$ 或 $n-1$ 结尾的方案，从而换成上面那种样子。

如果最后停在了 $i$ 而 $i<n-1$ ，那么由于它一定到过 $n$，所以存在两步是 $i+2\to i+1$ 和 $i+1\to i$ 。

把 $i+2\to i+1$ 这一步换成 $i\to i+1$（同时交换一下走顺序），显然这还可以构成欧拉路，并且终点变成了 $i+2$。

重复这个步骤可以让我们最后停在 $n$ 或者 $n-1$，且按上面那种走法走。

那么贪心一下就可以了（每次如果左边的没浇水就走到左边，否则走到右边）。

## Code

```cpp
#include <algorithm>
#include <cstdio>

typedef long long LL;

const int N = 100050;

int n;
LL m, A[N], need[N], b[N], S[N];

bool Check(LL x) {
  for (int i = 1; i <= n; ++i)
    if ((need[i] = (A[i] + x - 1) / A[i]) > m) return false;
  b[0] = S[0] = 0;
  for (int i = 1; i <= n; ++i) b[i] = std::max(need[i] - 1 - b[i - 1], 0LL);
  for (int i = 1; i <= n; ++i) S[i] = b[i] + S[i - 1];
  if ((S[n - 2] + need[n] + std::max(need[n - 1] - 1 - b[n - 2] - need[n], 0LL)) * 2 + n - 1 <= m) return true;
  if ((S[n - 1] + std::max(need[n] - 1 - b[n - 1], 0LL)) * 2 + n <= m) return true;
  return false;
}

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    scanf("%d%lld", &n, &m);
    for (int i = 1; i <= n; ++i) scanf("%lld", &A[i]);
    LL l = 0, r = 1000000000000000000LL;
    while (l < r) {
      LL mid = (l + r + 1) / 2;
      if (Check(mid)) l = mid;
      else r = mid - 1;
    }
    printf("%lld\n", l);
  }
  return 0;
}
```

# [F] Tournament

## Description

有 $n$ 个骑士进行 $k$ 轮比赛。每轮比赛中每个骑士都要与另一名骑士对决，并且有两个条件：

* 两名相同骑士只能对决一次。
* 如果在第 $i$ 轮中 `a` 与 `b` 对决，`c` 与 `d` 对决，而在第 $j$ 轮中 `a` 与 `c` 对决，那么在第 $j$ 轮中 `b` 必须与 `d` 对决。

问有没有合法方案。如果有，输出字典序最小的方案。

$n,k\leqslant1000,\sum n,\sum k\leqslant5000$。1s/64MB

## Solution

可以构造对决至多 $2^t-1$ 场的方案，其中 $2^t\mid n, 2^{t+1}\not\mid n$。

构造方式很简单：第 $j$ 轮 $i$ 匹配 $i{\,\rm xor\,}j$，其中 $0\leq i<n,1\leq j\leq k$。容易验证这是所有 $k\leq2^t-1$ 的字典序最小的方案（实际上相当于贪心构造）。

所以我们现在只需要做的事情是：证明确实最多只能对决 $2^t-1$ 场。

首先我们证明：$k$ 轮过后，如果两个骑士对决过就连一条边的话，得到的图里每个连通块大小都是 $2$ 的幂。

可以归纳证明：$k=0$ 是显然的（连通块大小都是 $2^0=1$）；假设前 $k-1$ 轮之后每个连通块大小都是 $2$ 的幂。

我们注意到，如果我确定了第 $k$ 轮某一个人的对手，那么可以用题面中第二条规则确定他所在连通块（前 $k-1$ 轮构成的图中的连通块，下同）所有人的对手；

并且如果是 `a` 和 `b` 对决的话，那么所有 `a` 连通块里的人都会与 `b` 连通块里的人对决，从而这两个连通块里的人一样多。

那么如果 `a` 和 `b` 对决，而它们本来就在一个连通块，之后还会在同一个连通块，于是根据归纳假设，这个连通块的大小还是 $2$ 的幂；

否则，`a` 和 `b` 不在一个连通块，但是他们所在的连通块大小相等。这两个连通块会合并成一个，从而变成了原来的两倍，还是 $2$ 的幂。

于是我们证明了结论。而如果对决了 $2^t$ 场，每个连通块大小必定大于 $2^t$，而又是 $2$ 的幂，从而是 $2^{t+1}$ 的倍数，与 $2^{t+1}\not\mid n$ 不符。

所以最多只能对决 $2^t-1$ 场。

## Code

注意要求输出时从 1 开始编号。

```cpp
#include <algorithm>
#include <cstdio>

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    int n, k;
    scanf("%d%d", &n, &k);
    if (k >= (n & -n)) puts("Impossible");
    else
      for (int i = 1; i <= k; ++i)
        for (int j = 0; j < n; ++j)
          printf("%d%c", (j ^ i) + 1, " \n"[j == n - 1]);
  }
}
```

# [G] Repair the Artwork

## Description

给定一个长为 $n$ 的仅包含 0,1,2 的串，要求你用 $m$ 个区间覆盖掉所有的 $2$ 但不覆盖任何一个 $1$。问方案数。

区间有顺序，答案 $\bmod 10^9+7$。

$n\leqslant100,m\leqslant10^9,T\leqslant1000$，且 $n>50$ 的不超过 $50$ 组。5s/64MB

## Solution

显然可以容斥：枚举哪些 $2$ 不被覆盖，若剩下的区间数是 $k$，则就把 $k^m$ 乘以 $1$ 或 $-1$ 加到答案里去。

那么可以用 dp 加速容斥：令 $f_{i,j}$ 表示若 $i$ 一定不被覆盖（$a_i=1,2$），则 $1\dots i$ 这段中，剩下的区间数是 $j$ 的方案对应的 $\pm1$ 的和是多少。

（换句话说就是，枚举 $1$ 到 $i$ 所有的 $2$ 里哪些不填，如果有 $t$ 个不填，而剩下的区间有 $k$ 个，那么 $f_{i,k}+=(-1)^t$）

dp 转移只需要枚举上一个必定不填的位置，可能是上一个 1 ，或者是当前位置与上一个 1 之间的所有 2。

为了方便可以在数列两头加上两个 1。

这样复杂度是 $O(n^4)$ （因为 $i=1\dots n,j=1\dots\frac{i(i+1)}2$）。但是仔细算一算转移总复杂度是 $\sum_{i=1}^n(n-i)\frac{i(i+1)}2\approx\frac{n^4}{12}$，还是能过的。

## Code

```cpp
#include <algorithm>
#include <cstdio>

typedef long long LL;

const int N = 105;
const int mod = 1000000007;

int n, m, A[N], f[N][N * N], nxt[N];

LL pow_mod(LL x, int p) {
  LL ans = 1;
  for (x %= mod; p; p >>= 1, x = x * x % mod)
    if (p & 1) ans = ans * x % mod;
  return ans;
}

void Work() {
  scanf("%d%d", &n, &m);
  int l = 0;
  for (int i = 1; i <= n; ++i) {
    scanf("%d", &A[i]);
    if (A[i]) l = nxt[l] = i;
  }
  A[0] = A[nxt[l] = ++n] = 1;
  for (int i = 1; i <= n; ++i)
    for (int j = 0; j <= i * (i + 1) / 2; ++j)
      f[i][j] = 0;
  nxt[n] = n + 1;
  f[0][0] = 1;
  for (int i = 0; i <= n; i = nxt[i])
    for (int j = 0; j <= i * (i + 1) / 2; ++j) if (f[i][j]) {
      if (A[i] == 2) f[i][j] *= -1;
      for (int k = nxt[i]; k <= n; k = nxt[k]) {
        int t = k - i - 1;
        (f[k][j + t * (t + 1) / 2] += f[i][j]) %= mod;
        if (A[k] == 1) break;
      }
    }
  int ans = 0;
  for (int i = 0; i <= n * (n + 1) / 2; ++i)
    ans = (ans + f[n][i] * pow_mod(i, m)) % mod;
  printf("%d\n", (ans + mod) % mod);
}

int main() {
  int T;
  scanf("%d", &T);
  while (T--) Work();
  return 0;
}
```

# [I] Soldier Game

## Description

$n$ 名士兵排成一排，第 $i$ 名士兵的能力值是 $a_i$。现在要把他们划成很多队伍，每支队伍要么是一个人，要么是连续的两个人。

一个人的队伍能力值就是这个人的能力值，两个人的队伍的能力值是他们两个能力值的和。

最小化队伍能力值的极差。

$n\leqslant10^5,\sum n\leqslant10^6$。2s/64MB

## Solution

可能的队伍显然只有 $2n-1$ 种。我们把队伍处理出来按能力值排序，那么只需要双指针扫一遍，同时用数据结构维护双指针之间的这些队伍能不能拼成整个 $1$ 到 $n$ 的区间。

由于队伍只可能是一个人或者相邻两个人，我们可以用线段树维护每个区间：

* 能不能被目前可行的队伍拼出来；
* 去掉最左边一个人（他可能和更左边的组队）后能不能被拼出来；
* 去掉最右边的一个人之后能不能被拼出来；
* 两头的人都去掉之后能不能被拼出来。

合并的时候考虑要不要把 $mid$ 和 $mid+1$ 组成一队即可。

## Code

```cpp
#include <algorithm>
#include <cstdio>

typedef long long LL;

const int N = 100050;

struct Msg {
  bool f[2][2];
  Msg() {}

  Msg(bool a) {
    f[0][0] = a;
    f[0][1] = f[1][0] = true;
    f[1][1] = false;
  }

  friend Msg Merge(const Msg &l, const Msg &r, bool b) {
    Msg ans;
    for (int i = 0; i < 2; ++i)
      for (int j = 0; j < 2; ++j)
        ans.f[i][j] = (l.f[i][0] && r.f[0][j]) || (l.f[i][1] && r.f[1][j] && b);
    return ans;
  }
} msgv[N * 4];

bool A[N], B[N];

inline void upd(int o, int l, int r) {
  if (l == r) msgv[o] = Msg(A[l]);
  else {
    int mid = (l + r) / 2;
    msgv[o] = Merge(msgv[o << 1], msgv[o << 1 | 1], B[mid]);
  }
}

void Build(int o, int l, int r) {
  if (l == r) A[l] = false;
  else {
    int mid = (l + r) / 2;
    B[mid] = false;
    Build(o << 1, l, mid);
    Build(o << 1 | 1, mid + 1, r);
  }
  upd(o, l, r);
}

void ModifyA(int o, int l, int r, int x, bool a) {
  if (l > x || r < x) return;
  if (l == r) A[l] = a;
  else {
    int mid = (l + r) / 2;
    ModifyA(o << 1, l, mid, x, a);
    ModifyA(o << 1 | 1, mid + 1, r, x, a);
  }
  upd(o, l, r);
}

void ModifyB(int o, int l, int r, int x, bool b) {
  if (l > x || r <= x) return;
  int mid = (l + r) / 2;
  ModifyB(o << 1, l, mid, x, b);
  ModifyB(o << 1 | 1, mid + 1, r, x, b);
  if (mid == x) B[mid] = b;
  upd(o, l, r);
}

int n, V[N];

struct Team {
  int ty, i, v; // 0 : A, 1 : B
  Team() {}
  Team(int ty, int i) : ty(ty), i(i) { v = V[i] + ty * V[i + 1]; }
  bool operator<(const Team &r) const { return v < r.v; }
} T[N * 2];

void Work() {
  scanf("%d", &n);
  for (int i = 1; i <= n; ++i) scanf("%d", &V[i]);
  int m = 0;
  for (int i = 1; i <= n; ++i) T[m++] = Team(0, i);
  for (int i = 1; i < n; ++i) T[m++] = Team(1, i);
  Build(1, 1, n);
  std::sort(T, T + m);
  int ans = 0x7fffffff;
  for (int i = 0, j = 0; i < m; ++i) {
    while (!msgv[1].f[0][0] && j < m) {
      (T[j].ty ? ModifyB : ModifyA)(1, 1, n, T[j].i, true);
      ++j;
    }
    if (!msgv[1].f[0][0]) break;
    if ((LL)T[j - 1].v - T[i].v < ans)
      ans = T[j - 1].v - T[i].v;
    (T[i].ty ? ModifyB : ModifyA)(1, 1, n, T[i].i, false);
  }
  printf("%d\n", ans);
}

int main() {
  int T;
  scanf("%d", &T);
  while (T--) Work();
  return 0;
}
```

# [J] Books

## Description

商店里有 $n$ 本书，第 $i$ 本书的价格是 $a_i$。

一个人拿着钱来到了商店，从 $1$ 到 $n$ 依次看过所有 $n$ 本书，每次如果自己还够买下当前的书就买下来。

已知他最后买了恰好 $m$ 本书，问他开始最多持有多少钱（钱是整数）。

无解输出 `Impossible`，答案无上界输出 `Richman`。

$0\leqslant m\leqslant n\leqslant10^5,\sum n\leqslant10^6,0\leqslant a_i\leqslant10^9$。1s/64MB

## Solution

所有 $a_i=0$ 的书他会无条件买下来。把这些 $a_i$ 都去掉，$n,m$ 都减去 $0$ 的个数。剩下的 $a_i>0$。

那么结论是他持有钱最多的时候会买下前 $m$ 本书，并且买完前 $m$ 本书之后剩下的钱不够买任何一本书（但是再加 1 就够再买一本）。

如果他最优解中买的不是前 $m$ 本书，那么我可以让他继续多拿钱直到从前 $m$ 本书里再买下一本。这时候他刚刚多买下一本，因此这本后面的都没有买。

显然我们可以继续让他多拿钱直到他买了 $m$ 本为止。这说明刚才那个解肯定不是最优解。

因此最优解中他一定买下了前 $m$ 本书，剩下的钱数只需要在后面的书的价格里取 $\min$ 再减一即可。

如果 $m=n$ 输出 `Richman`，如果 $m<0$ （意味着最开始 $a_i=0$ 的书多于 $m$ 本）输出 `Impossible`。

## Code

```cpp
#include <algorithm>
#include <cstdio>

typedef long long LL;

const int N = 100050;

int n, m, A[N];

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; ++i) {
      scanf("%d", &A[i]);
      if (!A[i]) --n, --m, --i;
    }
    if (m < 0) puts("Impossible");
    else if (n == m) puts("Richman");
    else {
      LL S = 0;
      int mn = 0x7fffffff;
      for (int i = 0; i < m; ++i) S += A[i];
      for (int j = m; j < n; ++j) mn = std::min(mn, A[j]);
      printf("%lld\n", S + mn - 1);
    }
  }
}
```

# [L] Sub-cycle Graph

## Description

$n$ 个点 $m$ 条边的带标号无向图中有多少图是某个 $n$ 个点的环的子图。答案对 $10^9+7$ 取模。

$3\leqslant n\leqslant10^5,m\leqslant\frac{n(n-1)}2,\sum n\leqslant3\times10^7,T\approx2\times10^4$。2s/64MB

## Solution

$n<m$ 时答案是 $0$，$n=m$ 时答案是 $\frac{(n-1)!}2$。下面假设 $n>m$。

显然这个图的每个连通块都是一条链。我们先把链看成有顺序的，或者说给链标个号。因为链一共有 $n-m$ 条，最后的答案要除以 $(n-m)!$。

如果一个链有 $i$ 个点（已知有哪 $i$ 个点）那么方案数就是 $\begin{cases}1&i=1\\\frac{i!}2&i>1\end{cases}$，其指数型生成函数是 $\frac x2\left(1+\frac1{1-x}\right)$。

所以 $n-m$ 条链 $n$ 个点方案数就是

$$
\begin{aligned}
&\quad \frac{n!}{(n-m)!}[x^n]\left(\frac x2\left(1+\frac1{1-x}\right)\right)^{n-m}\\
&=\frac{n!}{(n-m)!2^{n-m}}[x^m]\left(1+\frac1{1-x}\right)^{n-m}\\
&=\frac{n!}{(n-m)!2^{n-m}}\sum_{i=0}^{n-m}\binom{n-m}i[x^m]\frac1{(1-x)^{n-m}}\\
&=\frac{n!}{(n-m)!2^{n-m}}\sum_{i=0}^{n-m}\binom{n-m}i\binom{m+i-1}m
\end{aligned}
$$

除以 $(n-m)!$ 即为答案。注意这里认为 $\binom{-1}0=1$，解决这个问题可以特判一下 $m=0$ 的情况。

如果你需要这个公式的组合意义的话这里给出一个诡异的组合意义：

如果我们把 $n$ 个点排成一个链并把它切成 $n-m$ 部分，我们得到了 $n-m$ 条“有向链”，而且每种方案都会被得到 $(n-m)!$ 次。

理想的情况下我们把这个方案直接除以 $2^{n-m}$ 就行了。但是问题在于：单个点的“有向链”也只有 1 种方案。

那么我们强制指定每个单个点的链的方向。

我们从所有 $n-m$ 条链中选出若干条（注意现在链是有顺序的），强行令这些链是单点链且是“向左”的，剩下的链随便安排，但是其中单点链是“向右”的。

（换一种说法，对于多于一个点的链，我们可以说如果它左端点比右端点标号大就是“向左”的，反之就是“向右”的，而单点链的方向如上规定，则每条链的每个方向都被算过了）

那么这样的方案数和就是每条链的方向都有两种的方案了。除以 $(n-m)!2^{n-m}$ 即可。

这样的式子是（其中 $\binom{n-m}i$ 是选出 $i$ 条向左的单点链的方案数，$\binom{n-i-1}m$ 是从剩下 $n-i$ 个点中形成 $n-m-i$ 条链的方案数）

$$
\frac{n!}{(n-m)!2^{n-m}}\sum_{i=0}^{n-m}\binom{n-m}i\binom{n-i-1}m
$$

把 $i$ 换成 $n-m-i$ 就得到了刚才的式子。

## Code

```cpp
#include <algorithm>
#include <cstdio>

const int N = 100050;
const int mod = 1000000007;

typedef long long LL;

LL fac[N], ifac[N], inv[N], pv2[N];

LL C(int n, int m) {
  return m > n || m < 0 ? 0 : fac[n] * ifac[m] % mod * ifac[n - m] % mod;
}

int main() {
  inv[1] = 1;
  for (int i = 2; i < N; ++i) inv[i] = -(mod / i) * inv[mod % i] % mod;
  fac[0] = ifac[0] = pv2[0] = 1;
  for (int i = 1; i < N; ++i) {
    fac[i] = fac[i - 1] * i % mod;
    ifac[i] = ifac[i - 1] * inv[i] % mod;
    pv2[i] = pv2[i - 1] * inv[2] % mod;
  }
  int T;
  scanf("%d", &T);
  while (T--) {
    int n;
    LL m;
    scanf("%d%lld", &n, &m);
    if (n == m) printf("%lld\n", (fac[n - 1] * inv[2] % mod + mod) % mod);
    else if (m > n) puts("0");
    else if (m == 0) puts("1");
    else {
      LL ans = 0;
      for (int i = 1; i <= n - m; ++i)
        ans = (ans + C(n - m, i) * C(m + i - 1, m)) % mod;
      ans = ans * fac[n] % mod * ifac[n - m] % mod * pv2[n - m] % mod;
      printf("%lld\n", (ans + mod) % mod);
    }
  }
}
```

# [M] Function and Function

## Description

定义 $f(x)$ 为 $x$ 的十进制阿拉伯数字写出来的闭合区域个数。

或者说，

$f(0)=f(4)=f(6)=f(9)=1,f(1)=f(2)=f(3)=f(5)=f(7)=0,f(8)=2$，

而其他情况下 $f(x)$ 等于其每一位数字 $f$ 的和。令

$$
g^k(x)=\begin{cases}x&k=0\\f(g^{k-1}(x))&k>0\end{cases}
$$

给定 $k,x$，求 $g^k(x)$。1s/64MB

## Solution

由于 $f(x)=O(\log x)$，所以 $O(\log^*(x))$ 次之后就会变成 $0$。这时候判一下 $k$ 的奇偶性即可。

## Code

```cpp
#include <algorithm>
#include <cstdio>

const int A[] = { 1, 0, 0, 0, 1, 0, 1, 0, 2, 1 };

int f(int x) { return (x >= 10 ? f(x / 10) : 0) + A[x % 10]; }

int main() {
  int T;
  scanf("%d", &T);
  while (T--) {
    int x, k;
    scanf("%d%d", &x, &k);
    while (x && k) x = f(x), --k;
    if (!k) printf("%d\n", x);
    else printf("%d\n", k & 1);
  }
  return 0;
}
```