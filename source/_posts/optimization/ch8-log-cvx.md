---
title: 凸优化笔记 8：对数凹函数 Log-concave Function
date: 2020-03-11 23:05:14
tags:
  - 对数凹函数
categories:
  - Convex Optimization
mathjax: true
---

对数凹函数，顾名思义即取完对数以后 $\log f(x)$ 是凹函数，其应用比如在求最大后验 MAP 时，往往会对联合概率密度函数取对数。

<!--more-->

## 1. 定义

函数 $f$ 被称为**对数凹函数(log-concave)**，如果 $\log f$ 是凹的，也即
$$
f(\theta x+(1-\theta)y)\ge f(x)^\theta f(y)^{1-\theta}
$$
***对数凹函数的例子***：

- 幂函数 $x^\alpha$
- 正态分布
- 高斯分布的累积分布 $\Phi(x)=1/\sqrt{2\pi}\int_{-\infty}^{x}e^{-u^2/2}du$
- **指示函数** $I(x)=1_{x\in C}$，其中 $C$ 为凸集
- 行列式 $\det X$ 是 log-concave，关于 $S^n_{++}$
- $\det X/\text{tr}X$ on $S^n_{++}$

根据前面提到的复合函数凹凸性，由于 $\log$ 本身是凹函数，且为单调递增，因此可以有 $f$ concave $\Longrightarrow \log f$ concave。

另外，指数函数取对数以后，变成了线性函数，而高斯分布取对数以后变成了二次函数，是凸的。外面这一层取对数的操作可以直观的想想为把原始函数向下掰弯(划掉)了，比如高斯分布就把接近于 0 的值经过 $\log$ 操作掰到了 $-\infty$，从而变成了二次函数。

## 2. 性质

> 函数 $f$ 的定义域为凸集，$f$ 为 log-concave **当且仅当**
> $$
> f(x)\nabla^2 f(x)\preceq \nabla f(x) \nabla f(x)^T,\forall x\in \text{dom}f
> $$

另外

- 两个 log-concave 函数的**乘积**仍然是 log-concave
- 若 $f(x,y)$ 是 log-concave，则 $g(x)=\int f(x,y)dy$ 也是 log-concave
- 若 $f(x)$ 为 log-concave，则 $f(x-y)$ 关于 $(x,y)$ 都是 log-concave 的

但是注意，两个 log-concave 函数的**和不一定**是 log-concave，反例比如 $f_1=\lambda_1 \exp(-\lambda_1 x),f_2=\lambda_2 \exp(-\lambda_2 x)$。

类似的，对于**对数凸函数(log-convex)**也有一些性质：

- $\log f$ convex $\Longrightarrow f$ convex（since $f=\exp(\log f)$）
- 对数凸函数的和也是对数凸函数，即 $f_1,f_2$ log-convex $\Longrightarrow \log(f_1+f_2)$ convex（注意对数凹函数并没有这个性质）
- 给定 $y$，$f(x,y)$ 关于 $x$ 是 log-convex，则 $g(x)=\int f(x,y)dy$ 是 log-convex

