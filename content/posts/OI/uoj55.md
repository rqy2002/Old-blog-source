---
title: UOJ55 [WC2014]紫荆花之恋
categories:
  - 题解
tags:
  - 动态点分治
math: true
date: 2018-03-14 18:59:25
---

### Description

有一棵树，最开始是一棵空树。有$n$个操作，每次操作给出$a,c,r$，表示新的节点父亲编号为$a$，其与父亲之前的边权为$c$，其“感受能力”为$r$。

每次操作后询问满足$dist(i, j)\\leqslant r\_i+r\_j$的$(i,j)$有多少对，其中$dist$表示两点间唯一路径上的边权和。

所有$a\_i$与上次的答案$last\\\_ans$（初始为$0$）异或后对$10^9$取模。

<!--more-->

### Solution

首先，我们发现实际上每次只需要统计新加进的结点有多少合法解。那么，我们将$dist(i, j)$写成$dist(i,p)+dist(j,p)$，其中$p$是$i$和$j$的LCA。

那么，

$$\\begin{aligned}dist(i,j)&\\leqslant r\_i + r\_j\\\\
\\Leftrightarrow dist(i,p)+dist(j,p)&\\leqslant r\_i+r\_j\\\\
\\Leftrightarrow r\_i-dist(i,p)&\\geq dist(j,p)-r\_j\\end{aligned}$$

所以，我们枚举新加的结点的所有祖先$p$，计算以$p$为LCA的满足条件的点对$(i,j)$有多少个即可。

计算时，先查询$p$子树在添加$i$之前有多少$j$满足$r\_i-dist(i,p)\\geq dist(j,p)-r\_j$，再减去其中LCA不是$p$的（即，$i$和$j$在$p$的同一棵子树里，这样我们就要查询在这棵子树里有多少$j$满足此条件）。

然而，要高效地维护这个信息，就需要在每个结点上维护一个Treap（记录所有的$dist(j,p)-r\_j$），但这样当树退化为链，单次加入时间复杂度增加为$O(nlogn)$，空间复杂度增加为$O(n^2)$。

于是，借鉴替罪羊树的思想，我们在某个结点子树内的点个数大于其父亲子树内点的个数的$\\alpha\\in(0,1)$倍的时候暴力重构其父亲的子树。既然要求重构之后尽量平衡，理所当然地选用点分治。

这样，上面的分析中所有的“父亲”和“祖先”都应理解为点分治树上的祖先（LCA当然也是），但是答案仍是正确的，因为我们仍然不重不漏地枚举了所有可能的点。

代码实现上，还要写一个倍增LCA以查询$dist$，因为点分治之后的祖先不一定是原树上的祖先，不能直接用$dep$相减。

哦，还有，要写垃圾回收，因为有重构treap，之前的内存必须要重复利用。

### Code

```cpp
#include <algorithm>
#include <cstdio>
#include <cstdlib>
#include <stack>
#include <set>
typedef long long LL;
const int N = 100050;
int pre[N], to[N * 2], nxt[N * 2], c[N * 2], cnt = 0;
int r[N];
inline void add_edge(int x, int y, int v) {
  nxt[cnt] = pre[x];
  c[cnt] = v;
  to[pre[x] = cnt++] = y;
  nxt[cnt] = pre[y];
  c[cnt] = v;
  to[pre[y] = cnt++] = x;
}
namespace Tree{
  int dep[N], dep2[N], fa[N][17];
  void insert(int x, int c, int f) {
    add_edge(x, f, c);
    dep[x] = dep[fa[x][0] = f] + 1;
    dep2[x] = dep2[f] + c;
    for (int i = 1; i < 17; ++i)
      fa[x][i] = fa[fa[x][i - 1]][i - 1];
  }
  int LCA(int x, int y) {
    if (dep[x] > dep[y]) std::swap(x, y);
    for (int i = 16; ~i; --i)
      if (dep[fa[y][i]] >= dep[x])
        y = fa[y][i];
    for (int i = 16; ~i; --i)
      if (fa[x][i] != fa[y][i]) {
        x = fa[x][i];
        y = fa[y][i];
      }
    return x == y ? x : fa[x][0];
  }
  int dis(int x, int y) {
    int l = LCA(x, y);
    return dep2[x] + dep2[y] - 2 * dep2[l];
  }
};
struct Treap;
typedef Treap* PTreap;
struct Treap{
  static std::stack<PTreap> bin;
  PTreap lch, rch;
  int val, key, cnt, siz;
  void* operator new(size_t, int v) {
    Treap *res;
    res = bin.top();
    bin.pop();
    res->val = v; res->key = rand();
    res->cnt = 1; res->siz = 1;
    res->lch = res->rch = NULL;
    return res;
  }
  void operator delete(void *t) {
    bin.push((PTreap)t);
  }
  void update() {
    siz = cnt;
    if (lch != NULL) siz += lch->siz;
    if (rch != NULL) siz += rch->siz;
  }
  friend void Zig(PTreap &t) { //右旋
    PTreap l = t->lch;
    t->lch = l->rch;
    l->rch = t;
    t->update();
    l->update();
    t = l;
  }
  friend void Zag(PTreap &t) { //左旋
    Treap *r = t->rch;
    t->rch = r->lch;
    r->lch = t;
    t->update();
    r->update();
    t = r;
  }
  friend int query(PTreap o, int x) {
    if (o == NULL) return 0;
    if (o->val > x) return query(o->lch, x);
    else return query(o->rch, x) + (o->lch == NULL ? 0 : o->lch->siz) + o->cnt;
  }
  friend void insert(PTreap &o, int x) {
    if (o == NULL)
      o = new (x)Treap;
    else if (o->val == x)
      ++o->cnt;
    else if (o->val > x) {
      insert(o->lch, x);
      if (o->lch->key > o->key)
        Zig(o);
    } else {
      insert(o->rch, x);
      if (o->rch->key > o->key)
        Zag(o);
    }
    o->update();
  }
  friend void remove(PTreap &x) {
    if (x == NULL) return;
    remove(x->lch);
    remove(x->rch);
    delete x; x = NULL;
  }
};
std::stack<PTreap> Treap::bin;
namespace Dynamic_TreeDivision{
  PTreap tree[N], sonTree[N];
  int time, vise[N * 2];
  int fa[N], vis[N];
  std::set<int> son[N];
  void remove(int x) {
    vis[x] = time;
    for (std::set<int>::iterator i = son[x].begin(); i != son[x].end(); ++i) {
      remove(*i);
      remove(sonTree[*i]);
    }
    son[x].clear();
    remove(tree[x]);
  }
  int getCentre(int x, int f, int siz, int &ct) {
    int res = 1;
    bool ok = true;
    for (int i = pre[x]; ~i; i = nxt[i]) {
      if (vise[i] == time) continue;
      if (to[i] == f) continue;
      if (vis[to[i]] != time) continue;
      int ss = getCentre(to[i], x, siz, ct);
      if (ss > siz / 2) ok = false;
      res += ss;
    }
    if (siz - res > siz / 2) ok = false;
    if (ok) ct = x;
    return res;
  }
  void insertAll(int x, int f, int dep, PTreap &p) {
    insert(p, dep - r[x]);
    for (int i = pre[x]; ~i; i = nxt[i]) {
      if (vise[i] == time) continue;
      if (to[i] == f) continue;
      if (vis[to[i]] != time) continue;
      insertAll(to[i], x, dep + c[i], p);
    }
  }
  int divide(int x) {
    getCentre(x, 0, getCentre(x, 0, 1000000000, x), x);
    insertAll(x, 0, 0, tree[x]);
    for (int i = pre[x]; ~i; i = nxt[i]) {
      if (vise[i] == time) continue;
      if (vis[to[i]] != time) continue;
      vise[i] = vise[i ^ 1] = time;
      PTreap p = NULL;
      insertAll(to[i], 0, c[i], p);
      int s = divide(to[i]);
      fa[s] = x;
      son[x].insert(s);
      sonTree[s] = p;
    }
    return x;
  }
  void rebuild(int x) {
    ++time;
    remove(x);
    int ff = fa[x];
    PTreap p = sonTree[x];
    sonTree[x] = NULL;
    if (ff != 0) son[ff].erase(x);
    x = divide(x);
    fa[x] = ff;
    sonTree[x] = p;
    if (ff != 0) son[ff].insert(x);
  }
  LL insert(int x, int f) {
    LL ans = 0;
    son[f].insert(x);
    fa[x] = f;
    for (int i = x; i; i = fa[i]) {
      if (fa[i] != 0) {
        int d = Tree::dis(fa[i], x);
        ans += query(tree[fa[i]], r[x] - d);
        ans -= query(sonTree[i], r[x] - d);
        insert(sonTree[i], d - r[x]);
      }
      int d = Tree::dis(i, x);
      insert(tree[i], d - r[x]);
    }
    int rebuildx = 0;
    for (int i = x; fa[i]; i = fa[i])
      if (tree[i]->siz > tree[fa[i]]->siz * 0.88)
        rebuildx = fa[i];
    if (rebuildx) rebuild(rebuildx);
    return ans;
  }
};
Treap node[N * 100];
int main() {
  for (int i = 0; i < N * 100; ++i)
    Treap::bin.push(node + i);
  int n, a, cc, v;
  scanf("%*d%d", &n);
  LL lastans = 0;
  pre[0] = -1;
  for (int i = 1; i <= n; ++i) {
    scanf("%d%d%d", &a, &cc, &v);
    r[i] = v;
    a ^= lastans % 1000000000;
    pre[i] = -1;
    Tree::insert(i, cc, a);
    lastans += Dynamic_TreeDivision::insert(i, a);
    printf("%lld\n", lastans);
  }
  return 0;
}
```
