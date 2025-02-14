---
authors:
  - lnw143
date:
  created: 2025-02-14
  updated: 2025-02-14
categories:
  - Theory
---

# Hall 定理

## 定理内容

对于二分图 $G=(V,E)$，设其左、右部点集大小分别为 $n,m(n \le m)$。

对于左部点集的任意子集 $S$，定义其邻域 $N(S)$ 为点集内点与右部点相连的点集，即 $N(S) = \{v|\exists u \in S, (u,v) \in E\}$。

则该图有完美匹配的充要条件为 $\forall S,|S| \le |N(S)|$。

???note "证明"

    必要性显然，充分性可递归证明。

    设当前左部点为 $S$，对于 $u \in S$，考虑为其寻找一个匹配点。

    若 $N(S\setminus \{u\}) = N(S)$，任选 $v \in N(S)$ 作为 $u$ 的匹配点即可。

    否则选择在 $N(S)$ 中但不在 $N(S\setminus \{u\})$ 中的点，这些点接下来也没法被匹配，所以不影响下面的过程。

    于是我们将问题集合减小了 1，条件不变，递归构造即可。

## 扩展

二分图 $G=(V,E)$ 的最大匹配为 $|V| - \max_{S \subseteq V} \{ |S| - |N(S)| \}$。

证明参考 <https://www.zhihu.com/question/622760742>

## 例题

- [CF1519F](https://codeforces.com/problemset/problem/1519/F)
- [ARC076F](https://atcoder.jp/contests/arc076/tasks/arc076_d)

## 参考

- <https://www.cnblogs.com/yspm/p/15184873.html>
