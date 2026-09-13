---
title: 凸优化笔记14：SDP Representablity
date: 2020-04-05 18:11:25
tags:
  - SDP
categories:
  - Convex Optimization
mathjax: true
---

这一节简单介绍一个 SDP Representablity（SDP-Rep），这个概念的提出主要是为了便于判断某个问题是否可以转化为 SDP 优化问题。

**定义**：**集合** $X\subseteq R^n$ 是 **SDP-Rep** 的，如果他可以表示为
$$
X=\{x| \text{there exist }u\in R^k \text{ such that for some } \\A_i,B_j,C\in R^{m\times m}:\sum_i x_iA_i+\sum_j u_jB_j +C \succeq0 \}
$$
注：它实际上就是下面集合的一个子空间投影
$$
\{(x,u)|\sum_i x_iA_i+\sum_j u_jB_j +C \succeq0\}
$$
这个定义实际上说明了集合 $X$ 可以用一个 LMI 表示，因而如果 $X$ 为优化问题的定义域，则该定义域可以用一个 SDP 约束条件来表示。例如：如果 $X$ 为 SDP-Rep，则 $\min_{x\in X}c^Tx$ 是一个 SDP 问题。

**定义**：如果如果**函数** $f(x)$ 的 epigraph 是 SDP-Rep 的，那么函数 $f(x)$ 就是 **SDP-Rep**
$$
\text{epi}(f)=\{(x_0,x)|f(x)\le x_0\}
$$
该定义表明，如果 $f(x)$ 为 SDP-Rep，则优化问题 $\min_x f(x)$ 是一个 SDP 问题。

对 SDP-Rep 集合进行一些变换之后仍然是 SDP-Rep 的：如果 $X,Y$ 都是 SDP-Rep 的

- Minkowski sum $X+Y$
- intersection $X\cap Y$
- Affine pre-image $A^{-1}(X)$ if $A$ is affine
- Affine map $A(X)$ if $A$ is affine
- Cartesian product $X\times Y=\{(x,y)|x\in X,y\in Y\}$

