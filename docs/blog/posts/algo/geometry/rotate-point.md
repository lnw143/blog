---
title: rotate-point
authors:
  - lnw143
categories:
  - Geometry
date: 2024-06-16
---

# 平面直角坐标系的点绕原点旋转公式及证明

设点 $A(x,y)$ 绕原点 $O(0,0)$ 逆时针旋转 $\beta$，则设在极坐标系下 $A$ 的坐标为 $(r,\alpha)$

> 这意味着 $x=r \cos \alpha, y=r \sin \alpha$

目标点 $A'(x',y')$ 的极坐标即为 $(r,\alpha + \beta)$

展开（其中 $\sin$ 和 $\cos$ 的展开参考 [here](sin-cos-exp.md)）：

---

$$
\begin{aligned}
x' & = r \cos (\alpha + \beta) \\
& = r(\cos \alpha \cos \beta - \sin \alpha \sin \beta) \\
& = r\cos \alpha \cos \beta - r\sin \alpha \sin \beta \\
& = x \cos \beta - y \sin \beta \\
\end{aligned}
$$

---

$$
\begin{aligned}
y' & = r \sin (\alpha + \beta) \\
& = r (\sin \alpha \cos \beta + \cos \alpha \sin \beta) \\
& = r \sin \alpha \cos \beta + r\cos \alpha \sin \beta \\
& = y \cos \beta + x \sin \beta\\
\end{aligned}
$$

---

结论：点 $(x,y)$ 绕原点逆时针旋转 $\theta$ 后坐标为 $(x \cos \theta - y \sin \theta, x \sin \theta + y \cos \theta)$。
