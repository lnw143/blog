---
date: 2024-07-31
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

## 性质

1. $$
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

2. $$
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

3. 若矩阵有相同的两行，则行列式为零。

	*Proof:* 交换相同的两行后矩阵不变，且由 (2) 得行列式变号，即 $|A|=-|A|$，所以 $|A|=0$。

4. 若矩阵某一行是另一行的倍数，则行列式为零。

	*Proof:* 由 (1) & (3) 易证。

5. 若 $\forall j,a_{i,j} = b_j + c_j$，则

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

6. $$
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

	由 (4) 得 $|B|=0$，由 (5) 得 $|C| = |A| + |B| = |A|$。

7. $$