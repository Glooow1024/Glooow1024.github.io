---
title: 凸优化笔记 9：广义凸函数
date: 2020-03-11 23:05:27
tags:
  - 广义凸函数
categories:
  - Convex Optimization
mathjax: true
---

有时候函数 $f$ 为向量，此时怎么定义凸函数呢？根据广义不等式引入。

<!--more-->

## 1. 广义单调函数

对于 $f:R^n\to R$，定义其单调性为存在正常锥 $K\subseteq R^n$，有

- **K-nondecreasing** $\iff \forall x\preceq_K y,\quad f(x)\le f(y)$
- **K-nonincreasing** $\iff $

实际上相当于在定义域中规定了一个序关系，但是注意这种序关系并不是完全的，也就是说有可能有 $x\preceq_K y, y\preceq_K x$ 都不成立，意味着有可能定义域中的两个元素之间是不可“比大小的”。

对于广义单调函数的判定有以下性质

> $f$ K-nondecreasing $\iff \nabla f(x)\succeq_{K^*}0,\forall x\in \text{dom}f$

证明只需要将其转化为一维情形即可，可以用反证法。

## 2. 广义凸函数

对于 $f:R^n\to R^m$，凸函数定义为关于正常锥 $K\subseteq R^m$
$$
f(\theta x+(1-\theta)y) \preceq_K \theta f(x)+(1-\theta)f(y)
$$
***例子***

- $f:S^n_+\to S^n_+,f(X)=X^2$

广义凸函数有下面的性质

> $f$ 为 K-convex $\iff \omega^T f(x)$ convex，对任意 $\omega\succeq_{K^*}0$
>
> **Remarks**：只需要考虑 $g(x)=\omega^T f(x)$ 就转化为了普通凸函数，实际上相当于 $\omega\in K^*$ 确定了沿某个方向上的序关系。

