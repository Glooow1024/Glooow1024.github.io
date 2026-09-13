---
title: 凸优化笔记 1：Convex Sets
date: 2020-02-22 19:14:03
tags:
  - 凸集
categories:
  - Convex Optimization
mathjax: true
---

## 1. 凸集

区分两种集合的定义（下面的描述并不是严格的数学语言，领会意思就行了）：

- **仿射集(Affine set)**：$x=\theta x_1 + (1-\theta)x_2,\quad \theta\in\mathbb{R}$
- **凸集(Convex set)**：$x=\theta x_1 + (1-\theta)x_2,\quad \theta\in[0,1]$

主要的区别就在于后面 $\theta$ 的取值范围，简单理解就是说仿射集类似**直线**，凸集类似**线段**。

更一般的，仿射集都可以表示为线性方程组的解集，也即 $\{x|Ax=b\}$

<!--more-->

## 2. 常见凸集

### 2.1 凸包(Convec hull)

假如集合 $S=\{x_1,...,x_k\}$，则其**凸包**可以表示为
$$
\left\{\sum_{i=1}^k\theta_i x_i \vert \sum\theta_i=1, \theta_i\ge0\right\}
$$
![cvx hull](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/cvx_hull.PNG)

### 2.2 超平面(Hyperplanes)

类比三维空间中的平面，可以有**超平面**的定义
$$
\left\{x\vert a^Tx=b\right\}(a\ne0)
$$
其中 $a$ 就是该平面的法向量。

### 2.3 半空间(Halfspaces)

类似的，有**半空间**定义为
$$
\left\{x\vert a^Tx\le b\right\}(a\ne0)
$$

### 2.4 多面体(Polyhedra)

高维空间中的**多面体**定义为
$$
\left\{x\vert A x \preceq b, \quad C x=d \right\}
$$
其中 $\preceq$ 表示对每个元素都作比较。实际上就是求很多个半空间以及半平面的交集，与三维空间是类似的。

### 2.5 欧几里得球与椭球(Euclidean balls and ellipsoids)

高维空间中的**欧几里得球**的定义为
$$
B\left(x_{c}, r\right)=\left\{x |\left\|x-x_{c}\right\|_{2} \leq r\right\}=\left\{x_{c}+r u |\|u\|_{2} \leq 1\right\}
$$
**椭球**的定义为
$$
\left\{x |\left(x-x_{c}\right)^{T} P^{-1}\left(x-x_{c}\right) \leq 1\right\} = \left\{x_{c}+A u |\|u\|_{2} \leq 1\right\}
$$
其中 $P \in \mathbf{S}_{++}^{n}$ (也即 $P$ 为对称正定矩阵)。中间的矩阵 $P$ 的作用就相当于在各个特征向量方向上进行了放缩。

> **Remarks**：关于矩阵性质，可以参考我的矩阵论学习笔记，这里复习一个知识点。
>
> - **正规矩阵**的定义为满足 $A^HA=AA^H$ 的矩阵 $A$ 即为正规矩阵，因此**对称矩阵、Hermit矩阵、酉矩阵**都是正规矩阵。而正规矩阵有什么性质呢？正规矩阵可以**对角化**，且**存在一组正交的特征向量**！
> - **正定矩阵**的定义为满足 $x^TAx>0$ 的矩阵 $A$，实际上也就是说矩阵 $A$ 的**特征值均大于 0**！
> - 因此对称正定矩阵的性质有：所有特征向量正交，所有特征值大于 0。

### 2.6 范数球(norm balls)

**范数** $\Vert\cdot\Vert$ 需要满足以下性质

- $\Vert x \Vert\ge0;\ \Vert x\Vert=0 \iff x=0$
- $\|t x\|=|t|\|x\|$ for $t \in \mathbf{R}$
- $\|x+y\| \leq\|x\|+\|y\|$

向量范数如 $\Vert x\Vert_0, \Vert x\Vert_1, \Vert x\Vert_2, \Vert x\Vert_p, \Vert x\Vert_\infty$

矩阵范数如 $\Vert X\Vert_2, \Vert X\Vert_p$

**范数球**的定义为
$$
\left\{x |\left\|x-x_{c}\right\| \leq r\right\}
$$

### 2.7 凸锥(Convex cone)

我们先来看看锥的定义

- **锥(cone)**：$x\in C\Rightarrow \theta x\in C, \forall \theta\ge0$
- **凸锥(Convex cone)**：$x_1,x_2\in C \Rightarrow x=\theta_1 x_1+\theta_2 x_2 \in C,\forall \theta_1,\theta_2\ge0$

注意锥一定包含原点 0。锥不一定是凸的，反例如下，这是一个锥，但不是凸锥

![cone](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/cone.PNG)

### 2.8 范数锥(norm cone)

**范数锥**定义如下
$$
\{(x, t) |\|x\| \leq t\}
$$
也被称为 Ice cream cone。其中欧几里得范数锥被称为**二阶锥**(second-order cone)

![norm cone](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/norm_cone.PNG)

### 2.9 半正定锥(Positive semidefinite cone)

定义几个符号

- $\mathbf{S}^{n}$ 为 $n$ 阶对称矩阵
- $\mathbf{S}_{+}^{n}=\left\{X \in \mathbf{S}^{n} | X \succeq 0\right\}$ 为对称半正定矩阵，为凸锥
- $\mathbf{S}_{++}^{n}=\left\{X \in \mathbf{S}^{n} | X \succ 0\right\}$ 为对称正定矩阵

例如给定二阶矩阵
$$
\left[\begin{array}{ll}
{x} & {y} \\
{y} & {z}
\end{array}\right] \in \mathrm{S}_{+}^{2}
$$
其坐标满足如下图

![positive semidefinite cone](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/semidef_cone.PNG)

## 3. 保凸变换

上面是一些常见的凸集，对于更复杂的情况，怎么判断是否为凸集呢？

- 根据定义 $x_{1}, x_{2} \in C, \quad 0 \leq \theta \leq 1 \quad \Longrightarrow \quad \theta x_{1}+(1-\theta) x_{2} \in C$
- 凸集经过**保凸变换**以后仍然是凸集，如
  - 凸集的交集
  - 仿射变换
  - 投影变换
  - 分式线性映射

### 3.1 凸集的交集

任意个(可以是无数个)凸集的交集仍然是凸集

***例子 1***：$S=\left\{x \in \mathbf{R}^{m}|| p(t) | \leq 1 \text { for }|t| \leq \pi / 3\right\}$，其中 $p(t)=x_{1} \cos t+x_{2} \cos 2 t+\cdots+x_{m} \cos m t$

### 3.2 仿射变换

若映射 $f: \mathbf{R}^{n} \rightarrow \mathbf{R}^{m}$ 是仿射变换
$$
f(x)=A x+b \text { with } A \in \mathbf{R}^{m \times n}, b \in \mathbf{R}^{m}
$$
则有

- $S \subseteq \mathbf{R}^{n}$ convex $\Longrightarrow f(S)=\{f(x) | x \in S\}$ convex
- $C \subseteq \mathbf{R}^{m}$ convex $\Longrightarrow f^{-1}(C)=\left\{x \in \mathbf{R}^{n} | f(x) \in C\right\}$ convex

***例子 2***：双曲锥 $\left\{x | x^{T} P x \leq\left(c^{T} x\right)^{2}, c^{T} x \geq 0\right\}\left(\text { with } P \in \mathbf{S}_{+}^{n}\right)$，因为其可以转化为二阶锥

***例子 3***：$\left\{x | x_{1} A_{1}+\cdots+x_{m} A_{m} \preceq B\right\}$(with $A_i,P\in S^p$)**？？？**

### 3.3 投影变换

**投影函数** $P: \mathbf{R}^{n+1} \rightarrow \mathbf{R}^{n}$
$$
P(x, t)=x / t, \quad \operatorname{dom} P=\{(x, t) | t>0\}
$$
**Proof**：略。应用凸集定义

### 3.4 分式线性函数

分式线性映射 $f: \mathbf{R}^{n} \rightarrow \mathbf{R}^{m}$ 为
$$
f(x)=\frac{A x+b}{c^{T} x+d}, \quad \text { dom } f=\left\{x | c^{T} x+d>0\right\}
$$
其可以看作先仿射变换再投影变换。