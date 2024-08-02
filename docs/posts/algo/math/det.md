---
date: 
  created: 2024-07-31
  updated: 2024-08-02
authors:
  - lnw143
categories:
  - Math
---

# 行列式

## 定义

定义 $n$ 阶方阵 $A$ 的行列式为：

$$
\det A=\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
=\sum_{p_1,p_2,\cdots,p_n} (-1)^{\tau(p_1,p_2,\cdots,p_n)} \prod_{i=1}^n a_{i,p_i}
$$

其中 $\{p_1,p_2,\cdots,p_n\}$ 是一个 $n$ 的排列，$\tau(p_1,p_2,\cdots,p_n)$ 表示 $\{p_1,p_2,\cdots,p_n\}$ 的逆序数。

## 引理

### Lemma 1

$$
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
ka_{i,1} & ka_{i,2} & \cdots & ka_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
=
k
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
$$

读者自证不难。

### Lemma 2

$$
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1} & a_{j,2} & \cdots & a_{j,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
=
-\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1} & a_{j,2} & \cdots & a_{j,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
$$

读者自证不难。

### Lemma 3

若矩阵有相同的两行，则行列式为零。

*Proof:* 交换相同的两行后矩阵不变，且由 [Lemma 2](#lemma-2) 得行列式变号，即 $|A|=-|A|$，所以 $|A|=0$。$\square$

### Lemma 4

若矩阵某一行是另一行的倍数，则行列式为零。

*Proof:* 由 [Lemma 1](#lemma-1) & [Lemma 3](#lemma-3) 易证。

### Lemma 5

若 $\forall j,a_{i,j} = b_j + c_j$，则

$$
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
=
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
b_1 & b_2 & \cdots & b_n \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
+
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
c_1 & c_2 & \cdots & c_n \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
$$

读者自证不难。

### Lemma 6

$$
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1} & a_{j,2} & \cdots & a_{j,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
=
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1}+ka_{i,1} & a_{j,2}+ka_{i,2} & \cdots & a_{j,n}+ka_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
$$

*Proof:* 设 

$$
A = 
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1} & a_{j,2} & \cdots & a_{j,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix},
B = \begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
ka_{i,1} & ka_{i,2} & \cdots & ka_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix} \\
C = \begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
a_{2,1} & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots &  & \vdots \\
a_{i,1} & a_{i,2} & \cdots & a_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{j,1}+ka_{i,1} & a_{j,2}+ka_{i,2} & \cdots & a_{j,n}+ka_{i,n} \\
\vdots & \vdots &  & \vdots \\
a_{n,1} & a_{n,2} & \cdots & a_{n,n} \\
\end{vmatrix}
$$

由 [Lemma 4](#lemma-4) 得 $|B|=0$，由 [Lemma 5](#lemma-5) 得 $|C| = |A| + |B| = |A|$。$\square$

### Lemma 7

$$
\begin{vmatrix}
a_{1,1} & a_{1,2} & \cdots & a_{1,n} \\
0 & a_{2,2} & \cdots & a_{2,n} \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & a_{n,n} \\
\end{vmatrix}
= \prod_i a_{i,i}
$$

即上三角矩阵的行列式为主对角线元素的乘积。

*Proof:* 由行列式的定义得若 $\exists i, a_{i,p_i} = 0$，那么整个排列对行列式无贡献。

因此要想对行列式有贡献，$p_n$ 必等于 $n$，同理得 $\forall i, p_i = i$。$\square$

## 求解

显然有 $\mathrm O(n! n)$ 的 naive 方法，但如果这样上面的 $\LaTeX$ 就白写了（

考虑把矩阵转化为上三角矩阵并由 [Lemma 7](#lemma-7) $\mathrm O(n)$ 计算行列式。

这个过程形如高斯消元，于是考虑用高斯消元法来求解行列式。

假设目前处理到第 $i$ 行，要使得 $\forall j > i, a_{j,i} = 0$。

显然可以让第 $j$ 行加上第 $i$ 行乘 $- \frac{a_{i,j}}{a_{i,i}}$，但对于任意模数情况下 $a_{i,i}$ 不一定有逆元。

因此，我们考虑用辗转相除法。

每次使得 $a_{j,i} \gets a_{j,i} \bmod a_{i,i}$，并交换第 $i$ 行与第 $j$ 行。

即让第 $j$ 行加上第 $i$ 行乘 $- \lfloor \frac{a_{i,j}}{a_{i,i}} \rfloor$，并交换第 $i$ 行与第 $j$ 行。

时间复杂度看似是 $\mathrm O(n^3 \log P)$，但可以证明是 $\mathrm O(n^3)$，其中 $P$ 是模数。

??? note "参考代码"

    === "C++"
		```cpp
		int n,p,a[N + 2][N + 2];
		int det() {
			int sgn=0;
			for(int i=1; i<=n; ++i)
				for(int j=i+1; j<=n; ++j) {
					while(a[i][i]) {
						ll d=a[j][i]/a[i][i];
						for(int k=1; k<=n; ++k)
							a[j][k]=(a[j][k]-d*a[i][k]%p)%p;
						swap(a[i],a[j]);
						sgn^=1;
					}
					swap(a[i],a[j]);
					sgn^=1;
				}
			ll ans=sgn?-1:1;
			for(int i=1; i<=n; ++i) ans=ans*a[i][i]%p;
			return (ans+p)%p;
		}
		```

## 应用

- 矩阵树定理

