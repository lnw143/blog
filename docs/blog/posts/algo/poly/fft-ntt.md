---
authors:
  - lnw143
categories:
  - poly
date: 2024-06-22
draft: false
pin: false
---

# FFT & NTT 复习笔记

> 默认文中的形如 $[l,r)$ 的区间为其与整数集的交集。

## 快速变换

设原多项式为 $F(x) = \sum_{i \in [0,n)} a_i x ^ i$，其中 $n = 2 ^ k, k \in \mathbb Z ^ +$。

我们要求 $\forall i \in [0,n),\hat a_i = F(t_i)$，其中 $t$ 是一个长度为 $n$ 且两两互不相同的序列。

显然 $F$ 可以被一组 $\hat a,t$ 唯一确定，即点值表示法。

---

另设两个多项式

$$
G_0(x)=a_0 + a_2 x + \dots + a_{n - 2} x^{\frac n 2 - 1} \\
G_1(x)=a_1 + a_3 x + \dots + a_{n - 1} x^{\frac n 2 - 1} \\
$$

则有

$$
\begin{aligned}
F(x) & = \sum_{i \in [0,n)} a_i x ^ i \\
& = a_0 + a_1 x + a_2 x^2 + \dots + a_{n-1} x^{n-1} \\
& = (a_0 + a_2 x^2 + \dots + a_{n-2} x^{n-2}) + (a_1 x + a_3 x^3 + \dots + a_{n-1} x^{n-1}) \\
& = G_0 (x ^ 2) + x G_1 (x ^ 2) \\
\end{aligned}
$$

---

考虑构造单位根 $\omega _n ^k$ 满足 $\omega _n ^{\frac n 2} = -1, \omega _{2n} ^ {2k} = \omega _n ^k$。

显然也有 $\omega _n ^n = \omega _n ^0 = 1$。

设 $\forall i \in [0,n), t_i = \omega _n ^i$。

当 $x = \omega _n ^k, k \in [0,\frac n 2)$ 时显然有

$$
\begin{aligned}
F(\omega _n ^k) & = G_0(\omega _n ^{2k}) + \omega _n ^k G_1(\omega _n ^{2k}) \\
& = G_0(\omega _ {\frac n 2} ^ k) + \omega _n ^k G_1(\omega _{\frac n 2} ^k) \\
\end{aligned}
$$


当 $x = \omega _n ^{k + \frac n 2}, k \in [0,\frac n 2)$ 时有

$$
\begin{aligned}
F(\omega _n ^{k + \frac n 2}) & = G_0(\omega _n ^{2k + n}) + \omega _n ^{k + \frac n 2} G_1(\omega _n ^{2k + n}) \\
& = G_0(\omega _n ^{2k} \cdot \omega _n ^n) - \omega _n ^k G_1(\omega _n ^{2k} \cdot \omega _n ^n) \\
& = G_0(\omega _n ^{2k}) - \omega _n ^k G_1(\omega _n ^{2k}) \\
& = G_0(\omega _{\frac n 2} ^k) - \omega _n ^k G_1(\omega _{\frac n 2} ^k) \\
\end{aligned}
$$

---

由于两者只有一个符号的差异，于是 $F$ 的点值可以直接 $\mathrm O(n)$ 从 $G_0, G_1$ 的点值得到。

递归解决，时间复杂度 $\mathrm O(n \log n)$。

## 逆变换

设变换后的点值序列为 $\hat a$，即

$$
\begin{aligned}
\forall i \in [0,n), \hat a_i & = F(\omega _n ^i) \\
& = \sum _{j \in [0,n)} a_j (\omega _n ^i)^j \\
& = \sum _{j \in [0,n)} a_j \omega _n ^{ij} \\
\end{aligned}
$$

设多项式 $\hat F(x) = \sum _{i \in [0,n)} \hat a_i x^i$。

对 $\hat F$ 进行点值变换（$\forall i \in [0,n),t_i = \omega _n ^{-i}$），设点值序列为 $s$。

则有

$$
\begin{aligned}
\forall i \in [0,n), s_i & = \hat F(\omega _n ^{-i}) \\
& = \sum _{j \in [0,n)} \hat a_j (\omega _n ^{-i}) ^j \\
& = \sum _{j \in [0,n)} \omega _n ^{-ij} \hat a_j \\
& = \sum _{j \in [0,n)} \omega _n ^{-ij} \sum _{k \in [0,n)} a_k \omega _n ^{jk} \\
& = \sum _{j \in [0,n), k \in [0,n)} \omega _n ^{-ij} a_k \omega _n ^{jk} \\
& = \sum _{j \in [0,n), k \in [0,n)} \omega _n ^{j(k-i)} a_k \\
& = \sum _{k \in [0,n)} a_k \sum _{j \in [0,n)} \omega _n ^{j(k-i)} \\
& = \sum _{k \in [0,n)} a_k \sum _{j \in [0,n)} (\omega _n ^{k-i}) ^j \\
\end{aligned}
$$

---

显然第二个求和是一个等比数列，由等比数列求和公式 $\sum _{i \in [m,n)} p^i = \frac {p^m - p^n} {1 - p}$ 得：

- 当 $\omega _n ^{k-i} \not = 1 \iff i \not = k$

$$
\begin{aligned}
\sum _{j \in [0,n)} (\omega _n ^{k-i}) ^j & = \frac {1 - \omega _n ^{(k-i) n}} {1 - \omega _n ^{k-i}} \\
& = \frac {1 - (\omega _n ^{k-i}) ^n} {1 - \omega _n ^{k-i}} \\
& = \frac {1 - 1} {1 - \omega _n ^{k-i}} \\
& = 0
\end{aligned}
$$

- 当 $\omega _n ^{k-i} = 1 \iff i = k$

$$
\sum _{j \in [0,n)} (\omega _n ^{k-i}) ^j = \sum _{j \in [0,n)} 1 = n
$$

---

因此

$$
\begin{aligned}
\forall i \in [0,n), s_i & = \sum _{k \in [0,n)} a_k \sum _{j \in [0,n)} (\omega _n ^{k-i}) ^j \\
& = n a_i \\
\end{aligned}
$$

于是我们有

$$
\forall i \in [0,n), a_i = \frac {s_i} n
$$

## 构造单位根

- **FFT**

在复数域下，有 $\omega _n = \cos \frac {2 \pi} n + \mathrm i \sin \frac {2 \pi} {n}$。

其中 $\mathrm i = \sqrt {-1}$ 是 虚数单位，可以用 `C++` 中的 `complex` 库中的 `std::complex<double/long double>` 存储复数。

- **NTT**

对于模数 $P \in \mathbb P, \exists n,k \in \mathbb Z^+, P=2^nk+1$，在模 $P$ 意义下有 $\omega _n \equiv g ^ {\frac {P-1} n}$，其中 $g$ 是原根。

$g$ 是模 $P$ 意义下的原根当且仅当 $g ^i \not \equiv 1 \pmod P,\forall i \in [1,\phi(P))$ 且 $g ^{\phi(P)} \equiv 1 \pmod P$。

specially，$\forall P \in \mathbb P$，其原根 $g$ 满足 $\forall i \in [1,P-1), g ^i \not \equiv 1 \pmod P$ 且 $g^{P-1} \equiv 1 \pmod P$。

于是对 $n = 2 ^m, m \in \mathbb Z ^+$，我们有 $\omega _n ^n \equiv g ^{\frac {P - 1} {n} \cdot n} \equiv g ^{P - 1} \equiv 1, \pmod P$，且 $\omega _n ^{\frac n 2} \equiv g ^{\frac {P - 1} 2} \equiv \pm \sqrt {g ^ {P - 1}} \equiv \pm 1 \pmod P$，又 $g ^ {\frac {P-1} 2} \not \equiv 1 \pmod P$，所以 $\omega _n ^{\frac n 2} \equiv -1 \pmod P$。

还有 $\omega _{2n} ^{2k} \equiv g ^{\frac {2k(P-1)} {2n}} \equiv g ^{\frac{k(P-1)} n} \equiv \omega _n ^k \pmod P$

由于原根的特殊性，模数 $P \in \mathbb P$ 有特殊的限制，一般有 $P = k 2 ^m + 1, k,m\in \mathbb Z ^+$。

常见的模数有

$$
\begin{aligned}
167772161 = 5 \times 2 ^{25} + 1, g = 3 \\
469762049 = 7 \times 2 ^{26} + 1, g = 3 \\
754974721 = 45 \times 2 ^{24} + 1, g = 11 \\
998244353 = 119 \times 2 ^{23} + 1, g = 3 \\
1004535809 = 479 \times 2 ^{21} + 1, g = 3 \\
\end{aligned}
$$