---
title: fwt
authors:
  - lnw143
categories:
  - Poly
date: 2024-04-30
---

# FWT(Fast Walsh Transform) 学习笔记

> 本文中使用 $\cap$ 表示按位与，用 $\cup$ 表示按位或

## 与/或 卷积

### 问题引入

给定长度为 $2^n$ 的数列 $A,B$，求 $C_i = \sum_{j \cup k = i} A_j \times B_k$

显然有 $O(4^n)$ 的暴力

### 正变换

这一部分可以参考 [FMT 中的 Zeta 变换](fmt.md#zeta-变换) ，即定义 $\hat A_i$ 为序列 $A$ 中 $i$ 的子集和

### 快速变换

考虑分治

对于长度为 $2^n$ 的待求解的 $\hat A$，我们把它分成独立的两部分，即 $[0,2^{n-1})$ 和 $[2^{n-1},2^n)$ 两个区间分别求解

然后考虑这两个区间之间的贡献，发现只有 $[0,2^{n-1})$ 对 $[2^{n-1},2^n)$ 有贡献，于是对于 $i \in [0,2^{n-1})$，将 $\hat A_{i}$ 加到 $\hat A_{i+2^{n-1}}$ 中即可

时间复杂度 $O(n 2^n)$

### 逆变换

我们只需要对照正变换中的操作步骤，一步一步撤销变换即可

即对于 $i \in [0,2^{n-1})$，将 $\hat A_{i+2^{n-1}}$ 减去 $\hat A_{i}$ 的贡献，然后再递归地求解

---

当然还有另外的做法，即我们要将长度为 $2^n$ 的 $\hat A_i$ 在全集为 $2^n-1$ 的情况下（即不考虑目前的长度外的子集）只包括自己的 $A_i$，那么我们递归地求解完左右两个区间后，肯定有 $\forall i\in [0,2^{n-1}),\hat A_{i+2^{n-1}}=\hat A_i+A_{i+2^{n-1}}$，因此减去贡献即可

in brief，可以先递归再减贡献，这样逆变换与正变换只有一个 `+/-` 的变化

### 推广

对于 $\cap$ 的情况，与 $\cup$ 十分相似，请读者尽量自己构造变换，推一下式子，可以加深理解

如果没有思路，[here](https://oi-wiki.org/math/poly/fwt/#%E4%B8%8E%E8%BF%90%E7%AE%97)

## 异或 卷积

### 问题引入

给定长度为 $2^n$ 的数列 $A,B$，求 $C_i = \sum_{j \oplus k = i} A_j \times B_k$

### 原理

设 $\operatorname{popcnt}(i)$ 表示 $i$ 在二进制下 `1` 的个数，则有

$$
\operatorname{popcnt}(i \cap k) + \operatorname{popcnt}(j \cap k) \equiv \operatorname{popcnt}((i\oplus j)\cap k) \pmod 2
$$

> 证明：显然 $k$ 为 $0$ 的位上，$i,j$ 的取值不影响结果，那么设 $i'=i\cap k,j'=j\cap k$，那么问题转化为
> $$
> \operatorname{popcnt}(i') + \operatorname{popcnt}(j') \equiv \operatorname{popcnt}(i'\oplus j') \pmod 2
> $$
> 对于 $i',j'$ 每一位分开讨论，原命题易证

即异或不会改变 $1$ 的总数的奇偶性

### 正变换

我们尝试构造一个变换
$$
\hat A_i = \sum_j g(i,j) A_j
$$
例如对于 $\cup$ 卷积 ，$g(i,j)=[i\cup j=i]$

这个 $g$ 函数满足以下性质：

$$
\begin{aligned}
& \because \hat C_i = \hat A_i \times \hat B_i \\
& \therefore \sum_{p} g(i,p)C_p = \sum_{j,k} g(i,j)\times g(i,k)\times A_j \times B_k \\
& \therefore \sum_{p} g(i,p) \sum_{j \oplus k = p} A_j \times B_k = \sum_{j,k} g(i,j)\times g(i,k)\times A_j \times B_k \\
& \therefore \sum_{j,k} g(i,j \oplus k) A_j \times B_k = \sum_{j,k} g(i,j)\times g(i,k)\times A_j \times B_k
\end{aligned}
$$

即我们要使得 $g(i,j)\times g(i,k) = g(i,j \oplus k)$

因为 $\operatorname{popcnt}(i \cap k) + \operatorname{popcnt}(j \cap k) \equiv \operatorname{popcnt}((i\oplus j)\cap k) \pmod 2$，我们发现 $g(i,j) = (-1)^{\operatorname{popcnt}(i \cap j)}$ 满足这个性质

$$
\hat A_i = \sum_j (-1)^{\operatorname{popcnt}(i \cap j)} A_j
$$

手动推一下式子：

$$
\begin{aligned}
\hat A_i \times \hat B_i & = \sum_j g(i,j)A_j \times \sum_k g(i,k) B_k\\
& = \sum_j (-1)^{\operatorname{popcnt}(i \cap j)} A_j \times \sum_k (-1)^{\operatorname{popcnt}(i \cap k)} B_k\\
& = \sum_{j,k} (-1)^{\operatorname{popcnt}(i \cap j) + \operatorname{popcnt}(i \cap k)} A_j \times B_k \\
& = \sum_{j,k} (-1)^{\operatorname{popcnt}(i \cap (j \oplus k))} C_{j \oplus k} \\
& = \sum_{p} (-1)^{\operatorname{popcnt}(i \cap p)} C_p \\
& = \sum_p g(i,p) C_p \\
& = \hat C_i
\end{aligned}
$$

### 快速变换

有点抽象，画个 $n=3$ 的图来模拟一下

![快速变换图解](https://images.cnblogs.com/cnblogs_com/blogs/798566/galleries/2328994/o_240428080011_fwt_xor.svg)

假设现在我们要考虑从 $n=2$ 到 $n=3$ 的变换，我们发现左边是 `0??`，右边是 `1??`，分别多出一个最高位

设原变换为 $F_i$ ，目标变换为 $G_i$

- 左 $\to$ 左

  我们发现左边内部之间的 $\cup$ 结果不变，因此 $\forall i<4, G_i \gets F_i$

- 左 $\to$ 右 / 右 $\to$ 左

  我们发现 `0??` $\cup$ `1??` = `0??`，因此其 `1` 的个数不变，因此 $\forall i<4,G_i \gets F_{i+4}, G_{i+4} \gets F_i$。

- 右 $\to$ 右

  我们发现之前是 `??` $\cup$ `??` = `??`，但是现在在前面加了一个 `1`，因此 $-1$ 的指数加一，所以 $\forall i<4,G_{i+4} \gets -F_{i+4}$

综上，我们有 $\forall i<4,G_i=F_i+F_{i+4}, G_{i+4}=F_i-F_{i+4}$

generally，对于从 $n-1$ 到 $n$ 的变换，我们有 

$$
\forall i<2^{n-1}, G_i=F_i+F_{i+2^{n-1}}, G_{i+2^{n-1}}=F_i-F_{i+2^{n-1}}
$$

时间复杂度 $O(n2^n)$。

### 逆变换

对照上面式子易得 ~~（留给读者思考）~~

## [模板题](https://www.luogu.com.cn/problem/P4717)

核心代码如下

```cpp
void fwtOr(ll *a,int n,int type) {
	for(int i=1; i<(1<<n); i<<=1)
		for(int j=0; j<(1<<n); j+=(i<<1))
			for(int k=0; k<i; ++k)
				(a[j+k+i]+=type*a[j+k])%=P;
}
void fwtAnd(ll *a,int n,int type) {
	for(int i=1; i<(1<<n); i<<=1)
		for(int j=0; j<(1<<n); j+=(i<<1))
			for(int k=0; k<i; ++k)
				(a[j+k]+=type*a[j+k+i])%=P;
}
void fwtXor(ll *a,int n,int type) {
	for(int i=1; i<(1<<n); i<<=1)
		for(int j=0; j<(1<<n); j+=(i<<1))
			for(int k=0; k<i; ++k) {
				ll u=a[j+k],v=a[j+k+i];
				a[j+k]=(u+v)%P;
				a[j+k+i]=(u-v)%P;
				if(type==-1) {
					(a[j+k]*=Pi2)%=P;
					(a[j+k+i]*=Pi2)%=P;
				}
			}
}
```