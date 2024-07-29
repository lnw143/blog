---
title: fmt
authors:
  - lnw143
categories:
  - Poly
date: 2024-04-24
---

# FMT(Fast Möbius Transform) 学习笔记

> 小 Tips：在计算机语言中 $\cap$ = `&` / `and`， $\cup$ = `|` / `or`

## 定义

定义长度为 $2^n$ 的序列的 `and` 卷积 $A = B * C$ 为 $A_i=\sum_{j \cap k = i}{B_j \times C_k}$

考虑快速计算

## Zeta 变换

定义长度为 $2^n$ 的序列的 `Zeta 变换` 为

$$
\hat A_i = \sum_{j \subseteq i}{A_j}
$$

即子集和

它具有一下性质：

$$
\hat B_i \times \hat C_i
= \sum_{j \subseteq i} B_j \times \sum_{k \subseteq i} C_k
= \sum_{p \subseteq i} \sum_{j \cap k = p} B_j \times C_k
= \sum_{p \subseteq i} A_p = \hat A_i
$$

相当于 `FFT` 中的点值表示法，用以加速卷积过程

## 快速变换

暴力求解 $\hat A$ 最优时间复杂度为 $3^n$，考虑加速

考虑 `子集DP` ，有一篇很好的博客[^1]，这里大概讲解一下。

其实 $\hat A$ 本质上是一个高维（$n$ 维）前缀和

如果我们要求二维前缀和，显然可以通过一下代码实现：

```cpp
// 1st
for(int i=1; i<=n; ++i)
    for(int j=1; j<=m; ++j)
        	a[i][j]+=a[i-1][j];
// 2nd
for(int i=1; i<=n; ++i)
    for(int j=1; j<=n; ++j)
        	a[i][j]+=a[i][j-1];
```

我们发现 `1st` 完成后 `a[i][j]` 表示的是 $\sum_{k \le i} a_{k,j}$，即 $(i,j)$ 上方的元素和

那么 `2nd` 便是将 $(i,j)$ 左边操作后的 $\sum_{k \le j} a_{i,k}$ 累加到 `a[i][j]` 中，即完成二维前缀和

---

下面拓展到三维前缀和

```cpp
// 1st
for(int i=1; i<=n; ++i)
    for(int j=1; j<=n; ++j)
        for(int k=1; k<=n; ++k)
            a[i][j][k]+=a[i-1][j][k];
// 2nd
for(int i=1; i<=n; ++i)
    for(int j=1; j<=n; ++j)
        for(int k=1; k<=n; ++k)
            a[i][j][k]+=a[i][j-1][k];
// 3rd
for(int i=1; i<=n; ++i)
    for(int j=1; j<=n; ++j)
        for(int k=1; k<=n; ++k)
            a[i][j][k]+=a[i][j][k-1];
```

类似地，`1st,2nd` 中处理除了第 `k` 平面中的二维前缀和，`3rd` 在立体空间中把他们加起来，成为三维前缀和

---

回到 `Zeta 变换`：

$$
\hat A_i = \sum_{j \subseteq i}{A_j}
$$

如果我们用二进制 $(x_{n-1}x_{n-2}x_{n-3}\ ...\ x_2x_1x_0)_2$ 表示集合 $i$，二进制 $(y_{n-1}y_{n-2}y_{n-3}\ ...\ y_2y_1y_0)_2$ 表示集合 $j$

那么 $\hat A_i$ 可以被视为 $n$ 维前缀和，即 
$$
\hat A_i = \sum_{y_{n-1}\le x_{n-1},y_{n-2}\le x_{n-2},\ ...,\ y_1\le x_1,y_0\le x_0} A_j
$$

当然，下标都为 `0/1`

因此，我们有如下 Code：

```cpp
for(int i=0; i<n; ++i)
    for(int j=0; j<(1<<n); ++j)
        if(j&(1<<i))
            a[j]+=a[j^(1<<i)];
```

以上 Code 的 naive 形式为：

```cpp
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(i1>0) a[i1][i2][...][in]+=a[i1-1][i2][...][in];
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(i2>0) a[i1][i2][...][in]+=a[i1][i2-2][...][in];
...
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(in>0) a[i1][i2][...][in]+=a[i1][i2][...][in-1];
```

~~不要质疑我代码编译不通过~~

因此，我们用 $O(n2^n)$ 的时间复杂度解决了 `Zeta 变换`

## 逆变换

> warning：此处不是 `莫比乌斯反演`！

众所周知，前缀和的逆运算即为差分

我们发现，只需要先将最后一维差分，即可将序列处理为 $n-1$ 维前缀和

即

```cpp
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(in>0) a[i1][i2][...][in]-=a[i1][i2][...][in-1];
...
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(i2>0) a[i1][i2][...][in]-=a[i1][i2-2][...][in];
for(int i1=0; i1<2; ++i1) for(int i2=0; i2<2; ++i2) ... for(int in=0; in<2; ++in) if(i1>0) a[i1][i2][...][in]-=a[i1-1][i2][...][in];
```

但实际上因为前缀和都是无序的，因此我们直接正着做就可以啦

只需要把正变换的 `+=` 改为 `-=` 就可以了

## 扩展

有时候，题目给的不是 $\cap$ ，而是 $\cup$ 怎么办？

那我们重定义 `Zeta 变换` 为：
$$
\hat A_i = \sum_{i \supseteq j} A_j
$$
再推一下式子：
$$
\hat B_i \times \hat C_i
=\sum_{i\supseteq j} B_j \times \sum_{i\supseteq k} C_k
=\sum_{i\supseteq j,i\supseteq k} B_j \times C_k
=\sum_{i\supseteq p} \sum_{j \cup k=p} B_j \times C_k
=\sum_{i\supseteq p} A_p
=\hat A_i
$$
变换即超集和，高维后缀和

求法也很简单，只用将源代码中的 `if(j&(1<<i))` 该为 `if(!(j&(1<<i)))` 即可

---

是 $\oplus$ 怎么办？

~~快去学 [FWT](fwt.md) 吧~~

## 例题

- [ABC349F](https://atcoder.jp/contests/abc349/tasks/abc349_f)

> 再送你一个并/交集的小口诀：下并或，上交与

[^1]: [高维前缀和总结(sosdp) - heyuhhh - 博客园 (cnblogs.com)](https://www.cnblogs.com/heyuhhh/p/11585358.html)