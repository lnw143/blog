---
authors:
  - lnw143
categories:
  - Record
date: 
  created: 2025-07-11
  updated: 2025-07-11
---

# 计数 DP 技巧

## [Cat Jumps](https://atcoder.jp/contests/wtf22-day2-open/tasks/wtf22_day2_d)

> sort: 组合意义

将形如 $\prod _{i=1} ^{n} (x+i)$，其中 $x = \sum_i^n a_i$ 的式子转化为对每个点连一条出边 $j$，权值为 $c_i = a_j + [j \le i]$，所有方案中边权乘积之和。这是因为 $\sum \prod_i^n c_i = \prod_i^n \sum c_i$，而一个 $i$ 的所有方案中边权和恰为 $i+\sum a$，故相等。

之后用各类容斥技巧，以及第一、二类斯特林数进行计数。
