---
title: 凸优化笔记 0：绪论
date: 2020-02-22 19:13:19
tags:
categories:
  - Convex Optimization
mathjax: true
---

## 0. 绪论

首先明确凸优化这门课的主要目的：

1. 判断一个问题是否为凸的
2. 将一个问题转化为凸的
3. 求解凸优化问题，给出算法性能

<!--more-->

## 1. 优化问题与凸优化

### 1.1 一般优化问题

一般优化问题的形式为
$$
\begin{aligned}
\text{min}\quad & f_{0}(x)\\
\text { s.t. } \quad & f_{i}(x) \leq b_{i}, \quad i=1, \dots, m
\end{aligned}
$$
其中 $f_0$ 为优化目标，$f_i$ 为约束函数。

一般的优化问题都很难求解，可能无法给出一个解析解，甚至数值解法也不能给出最优解。而在众多复杂的优化问题中，有一些问题则较为容易：

- 最小二乘问题(least-squares problems)：有解析解
- 线性规划问题(linear programming problems )
- 凸优化问题(convex optimization problems)：通常没有解析解，但是有有效的数值解法

### 1.2 凸优化问题

如果目标函数和约束函数都是**凸函数**，则被称为凸优化问题。满足以下条件的函数称为凸函数
$$
f(\alpha x+\beta y) \leq \alpha f(x)+\beta f(y),\quad \alpha+\beta=1,\alpha>0,\beta>0
$$
凸优化中经常遇到的困难为：

- 难以判断一个问题是否为凸的
- 将一个问题转化为凸问题需要很多的技巧(tricks)

