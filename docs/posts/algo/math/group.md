---
authors:
  - lnw143
categories:
  - Math
date: 
  created: 2025-07-13
  updated: 2025-07-13
---

# 群论基础

## 定义

定义集合 $G$ 与在 $G$ 上的二元运算 $\times$，若其满足以下条件，则称 $(G,\times)$ 为一个**群**。

1. **封闭性**。$\forall a,b \in G,a \times b \in G$
2. **结合性**。$\forall a,b,c \in G,(a \times b) \times c = a \times (b \times c)$
3. **单位元存在性**。$\exists e \in G,\forall a \in G,e \times a = a \times e = a$
4. **逆元存在性**。$\forall a \in G,\exists b \in G,a \times b = b \times a = e$

## 性质

??? note "单位元唯一"

	$e_1 \times e_2 = e_1 = e_2$

??? note "逆元唯一"

	设 $a$ 有逆元 $b,c$，$c \times a \times b = e \times b = c \times e = b = c$

## 关于子群 & 陪集

对于群 $(G,\times)$，若 $H \subseteq G$，且 $(H,\times)$ 构成一个群，则称 $(H,\times)$ 为 $(G,\times)$ 的**子群**，简记为 $H \le G$。

对于 $H \le G,g \in G$，定义 $gH=\{g \times h \mid h \in H\}$，称为 $H$ 关于 $g$ 的**左陪集**。右陪集同理。

下面是陪集的若干性质。

1. $\forall g \in G, |gH| = |H|$
2. $\forall g \in G, g \in gH$ （这是因为 $e \in H$）
3. $gH = H \iff g \in H$
4. $aH = bH \iff b ^{-1} \times a \in H$
5. $aH \cap bH \not = \emptyset \Rightarrow aH = bH$
6. $H$ 所有的陪集并为 $G$。

??? note "Proof"

	(3) $g \in H \Rightarrow gH = H$，$g \not \in H, g \in gH \Rightarrow gH \not = H$

	(4) $aH = bH \Rightarrow b^{-1} \times a H = H \Rightarrow b^{-1} \times a \in H$

	(5) $aH \cap bH \not = \emptyset \Rightarrow \exists x,y \in H, ax = by \Rightarrow x \times y^{-1} = b\times a^{-1} \in H \Rightarrow aH = bH$


一些表述：

对于 $H \le G$，$G / H$ 表示 $G$ 中 $H$ 所有的左陪集 $\{gH \mid g \in G\}$。

对于 $H \le G$，$[G : H]$ 表示 $G$ 中 $H$ 不同陪集的数量。

???+ tip "关于记号"

	以下将群 $(G,\times)$ 简记为 $G$。

## 拉格朗日定理

对于有限群 $G$ 及其子群 $H$，有

$$
|H| \times [G:H] = |G|
$$

即 $H$ 的阶乘 $G$ 中 $H$ 不同陪集的数量等于 $G$ 的阶。

理解：因为 $H$ 的所有陪集不交，大小都为 $|H|$，且并为 $G$，故不同的陪集有 $\frac { |G| } { |H| }$ 个。

## 群作用

对于群 $G$ 和集合 $X$，$G$ 中每个元素都是 $X$ 到自身的映射，记 $g \in G$ 将 $x \in X$ 映射为 $g(x)$，若所有映射满足以下条件，那么称群 $G$ **作用** 于集合 $X$ 上。

1. 结合律。$\forall g_1,g_2 \in G,x \in X,g_1(g_2(x)) = (g_1 \times g_2)(x)$
2. 单位元是恒等变换。$\forall x \in X,e(x) = x$

## 轨道-稳定子定理

对于群 $G$ 作用在集合 $X$ 上，称 $x \in X$ 的稳定子为 $G^x = \{g \mid g \in G, g(x) = x\}$，轨道为 $G(x) = \{g(x) \mid g \in G\}$。

定理内容：

$$
\forall x \in X, |G^x| \times |G(x)| = |G|
$$

即任意 $X$ 中的元素，其轨道和稳定子的大小之积为群 $G$ 的阶。


??? note "考虑证明 $G^x$ 为 $G$ 的子群"

	首先有 $e \in G^x$

	封闭性：对于 $\forall a,b \in G^x$，有 $a(x) = b(x) = x$，再根据 $a(b(x)) = (a \times b) (x)$，有 $a \times b \in G^x$。

	结合性显然。

	逆元：对于 $g \in G^x$，有 $g^{-1}(g(x))=(g^{-1} \times g)(x) = e(x) = x$，故 $g^{-1} \in G^x$。

故根据拉格朗日定理我们只需证明 $|G(x)|=[G:G^x]$。
