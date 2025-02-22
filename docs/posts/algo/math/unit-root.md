---
authors:
  - lnw143
categories:
  - Math
date: 
  created: 2025-02-22
  updated: 2025-02-22
---

# 单位根相关

复数域下方程 $x^n = 1$ 恰好有 $n$ 个解，记 $\omega_n = e^{\frac {2 \text{i} \pi} n}$，则这 $n$ 个解恰好为 $1,\omega_n,\omega_n^2,\cdots,\omega_n^{n-1}$。我们将 $\omega_n$ 称作 $n$ 次单位根。

在运算中，有 $\omega_n = e^{\frac {2 \text{i} \pi} n} = \cos \frac {2 \pi} n + \text{i} \sin \frac{2 \pi} n$。

特别地，在模 $P$ 意义的数域下，有 $\omega_n \equiv g^{\frac {P-1} n} \pmod P$，其中 $g$ 是模 $P$ 意义下的原根。

## 基础公式

### Lemma 1

$$
\sum _{i=0} ^{n-1} \omega_n^i = [n=1]
$$

这是因为根据等比数列求和公式有原式 $= \frac{1 - \omega_n^n} {1 - \omega_n}$，当 $n>1$ 时该式分子为零分母不为零，当 $n=1$ 时原式等于 $1$。

### Lemma 2

$$
[n|k] = \frac 1 n \sum _{i=0} ^{n-1} \omega _n ^{ik}
$$

??? note "证明"
    - 当 $n|k$ 时，原式显然成立。
    
    - 当 $n \nmid k$ 时，有 $\omega_n ^k \not = 1$，根据等比数列求和公式有
    $$
    \sum _{i=0} ^{n-1} \omega _n ^{ik} = \frac {1 - \omega_n ^{kn}} {1 - \omega_n ^k} = 0
    $$

注意到 [Lemma 1](#lemma-1) 是 Lemma 2 $k=1$ 时的特例。

### Lemma 3

$$
[k \equiv a \pmod n] = \frac 1 n \sum_{i=0}^{n-1} \omega_n^{i(k-a)}
$$

这是因为 $k \equiv a \pmod n \iff n \mid (k-a)$。

[Lemma 2](#lemma-2) 和 [Lemma 3](#lemma-3) 是 [**单位根反演**](#单位根反演) 的重要公式。

## 单位根反演

设有多项式 $f(x)$，现在我们要求它在 $d$ 的倍数处的系数和，即 $\sum_{d \mid i} [x^i]f(x)$，但是难以求出这个多项式的系数序列，甚至是无限长的。这是我们就要用到**单位根反演**。

### 基本形式

$\sum_{d \mid i} [x^i]f(x) = \frac 1 d \sum_{i=0}^{d-1} f(\omega_d ^i)$。

证明考虑设 $f(x) = \sum _{i \ge 0} a_i x^i$，考虑对于一位 $a_j$，它贡献答案的系数为 $\sum _{i=0} ^{d-1} \omega_d ^{ij}$，根据 [Lemma 2](#lemma-2) 这个式子等于 $d[d|j]$，因此原式易得。

由此以及 [Lemma 3](#lemma-3) 我们容易得到更一般的形式：

$$
\sum _{i \equiv a \pmod n} [x^i] f(x) = \frac 1 n\sum _{i=0} ^{n-1} \omega_n ^{-ia} f(\omega_n ^i)
$$

### 例题

#### [LOJ 6485](https://loj.ac/p/6485)

给定 $n,s,a_{[0,4)}$，求

$$
\sum _{i=0} ^n \binom {n} {i} s^i a_{i \bmod 4}
$$

??? note "solution"
    容易想到对于 $i \in [0,4)$ 分开统计贡献，我们由系数序列 $f_i = \binom n i s^i$ 得到函数 $f(x) = (1+xs)^n$，由单位根反演的基本形式容易在 $O(\log n)$ 内解决。

推荐例题：[QOJ5370](https://qoj.ac/problem/5370)。

#### [Luogu P5591](https://www.luogu.com.cn/problem/P5591)

给定 $n,p,k$，询问

$$
\sum_{i=0}^n \binom n i \times p^{i} \times \left\lfloor \frac{i}{k} \right\rfloor \bmod 998244353
$$

保证 $k$ 是 $2$ 的 $[0,20]$ 次幂。

??? note "hint"
    $\left\lfloor \frac{i}{k} \right\rfloor = \frac {i - (i \bmod k)} k$

??? note "solution"
    将答案拆成 $\sum_i \binom n i p^i i$ 和 $\sum_i \binom n i p^i (i \bmod k)$ 两部分。

    对于第一部分，我们有 $\binom n m = \frac n m \binom {n-1} {m-1}$，因此

    $$
    \begin{aligned}
    \sum_{i=0} ^n \binom n i p^i i & = n \sum_i \binom {n-1} {i-1} p ^i \\
    & = n p (1 + p) ^{n-1} \\
    \end{aligned}
    $$

    最后一步由二项式定理易得。

    第二部分使用单位根反演，对于 $i \in [0,k)$ 计算系数：

    $$
    \begin{aligned}
    \sum_{i=0} ^n \binom n i p^i (i \bmod k) & = \sum _{i=0} ^{k-1} i \sum_{j=0} ^n [i \equiv j \pmod k] \binom n j p^j \\
    & = \sum _{i=0} ^{k-1} i \sum_{j=0} ^n \sum_{t=0} ^{k-1} \omega_k^{t(j-i)} \binom n j p^j \\
    & = \sum _{i=0} ^{k-1} i \sum_{t=0} ^{k-1} \omega_k^{-it}\sum_{j=0} ^n \omega_k^{tj} \binom n j p^j \\
    & = \sum _{i=0} ^{k-1} i \sum_{t=0} ^{k-1} \omega_k^{-it}\sum_{j=0} ^n \binom n j (\omega_k^tp)^j \\
    & = \sum _{i=0} ^{k-1} i \sum_{t=0} ^{k-1} \omega_k^{-it} (1 + \omega_k^t p)^n \\
    & = \sum _{i=0} ^{k-1} i \sum_{t=0} ^{k-1} \omega_k^{-it} g(t) & \bigg(g(x) = (1 + \omega_k^x p)^n \bigg) \\
    & = \sum _{i=0} ^{k-1} i f(\omega_k^{-i}) & \bigg(f(x) = \sum_{i=0} ^{k-1} g(i)x^i\bigg) \\
    \end{aligned}
    $$

    算式中的 $g(x)$ 我们可以在 $O(k \log n)$ 的时间内求出，而对于求 $f$ 在单位根处的点值，我们可以通过 NTT 在 $O(k \log k)$ 时间内求出，总时间复杂度 $O(k(\log k + \log n))$。

    当然本题还有更简单的做法，即在公式倒数第二步交换 $i,t$ 的求和顺序，然后就是求一个 $S(n,x) = \sum_{i=0} ^n i x^i$，可以用类似等比数列求和的技巧做到 $O(\log n)$ 时间内求解。

#### [Luogu P10664](https://www.luogu.com.cn/problem/P10664)

给定整数 $n,k,p$，要求计算下列式子对 $p$ 取模的值：

$$\sum_{i=0}^n [k|i] \binom n {i} F_{i}$$

其中：

- $p$ 为质数，且 $p$ 除以 $k$ 的余数为 $1$。

- $F_n$ 为斐波那契数列，即 $F_0=1$，$F_1=1$，$F_n=F_{n-1}+F_{n-2}(n\geq 2)$。

??? note "hint"
    Fibonacci 数列看起来很棘手，难以将其转化为多项式函数。

    但是别忘了它可以用矩阵快速幂计算，我们可以将函数变为从矩阵到矩阵的映射，从而将 $F_i$ 变成 $x^i$。

??? note "solution"
    先忽略 $k$ 的限制，我们要求 $\sum_i \binom n i F_i$。

    我们设 Fibonacci 转移矩阵为 $X$，则原式为 $(I + X)^n$。

    最后设 $f(x)=(I+Xx)^n$ 即可使用单位根反演快速计算。

    注意 $n$ 要开到 `long long` 范围。

## 参考

- [Xun-Xiaoyao 浅谈单位根反演](https://www.cnblogs.com/Xun-Xiaoyao/p/17683862.html)

- [nekko （也许是）冷门算法——单位根反演](https://www.luogu.com.cn/article/j0zigjbf)

- [command_block 单位根反演小记](https://www.luogu.com/article/4l2bmvkz)