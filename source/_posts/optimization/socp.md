---
title: Second-Order Cone Programming(SOCP) 二次锥规划
date: 2020-02-17 08:42:13
tags:
  - SOCP
categories: 
  - Convex Optimization
mathjax: true
---

二次锥规划是凸优化的一部分，可以应用**内点法**快速求解，相比于**半正定规划**更有效。

<!--more-->

## 1. 二阶锥

### 1.1 二阶锥定义

在此之前，先给出**二阶锥**的定义。

在 $k$ 维空间中二阶锥 (Second-order cone) 的定义为
$$
\mathcal{C}_{k}=\left\{\left[\begin{array}{l}
{u} \\
{t}
\end{array}\right] | u \in \mathbb{R}^{k-1}, t \in \mathbb{R},\|u\| \leq t\right\} \notag
$$
其也被称为 quadratic，ice-cream，Lorentz cone。

### 1.2 二阶锥约束

在此基础上，**二阶锥约束**即为
$$
\|A x+b\| \leq c^{T} x+d \Longleftrightarrow\left[\begin{array}{c}
{A} \\
{c^{T}}
\end{array}\right] x+\left[\begin{array}{l}
{b} \\
{d}
\end{array}\right] \in \mathcal{C}_{k}
$$
其中 $x\in \mathbb{R}^{n}, A\in\mathbb{R}^{(k-1)\times n}, b\in\mathbb{R}^{k-1},c\in\mathbb{R}^{n},\mathbb{R}$。实际上是对 $x$ 进行了仿射变换，由于仿射变换不改变凹凸性，因此二阶锥也是凸锥。

## 2. 优化问题建模

优化目标如下，其中 $f \in \mathbb{R}^{n}, A_{i} \in \mathbb{R}^{n_{i} \times n}, b_{i} \in \mathbb{R}^{n_{i}}, c_{i} \in \mathbb{R}^{n}, d_{i} \in \mathbb{R}, F \in \mathbb{R}^{p \times n},$ and $g \in \mathbb{R}^{p}, x \in \mathbb{R}^{n}$
$$
\begin{align}
\text{minize}\quad& f^{T} x \notag\\
\text{subject to}\quad& {\left\|A_{i} x+b_{i}\right\|_{2} \leq c_{i}^{T} x+d_{i}, \quad i=1, \ldots, m} \notag\\
&{F x=g}\notag
\end{align}\notag
$$
上述问题被称为**二次锥规划**是因为其约束，要求仿射函数 $(Ax+b,c^T x+d)$ 为 $\mathbb{R}^{k+1}$ 空间中的二阶锥。

## 3. 类似问题转化

一些其他优化问题也可以转化为 SOCP，例如

### 3.1 二次规划

考虑二次约束
$$
x^{T} A^{T} A x+b^{T} x+c \leq 0  \notag
$$
可以等价转化为 SOC 约束
$$
\left\|\begin{array}{c}\left(1+b^{T} x+c\right) / 2 \\ Ax\end{array}\right\|_{2} 
\leq\left(1-b^{T} x-c\right) / 2 \notag
$$

### 3.2 随机线性规划

问题模型为
$$
\begin{align}
\text{minize}\quad& c^{T} x \notag\\
\text{subject to}\quad& \mathbb{P}\left(a_{i}^{T} x \leq b_{i}\right) \geq p, \quad i=1, \ldots, m \notag
\end{align}\notag
$$
问题转化可参考[维基百科](https://en.wikipedia.org/wiki/Second-order_cone_programming)

## 4. 问题求解

二阶锥规划可以应用**内点法**快速求解，且比**半正定规划**(semidefinite programming)更有效。

Matlab 有专门的凸优化工具包，[下载地址](http://cvxr.com/cvx/)在这里，安装教程在官网上有。使用方法如下，只需要修改优化目标和约束条件即可

```matlab
m = 20; n = 10; p = 4;
A = randn(m,n); b = randn(m,1);
C = randn(p,n); d = randn(p,1); e = rand;
cvx_begin
    variable x(n)
    minimize( norm( A * x - b, 2 ) )
    subject to
        C * x == d
        norm( x, Inf ) <= e
cvx_end
```

