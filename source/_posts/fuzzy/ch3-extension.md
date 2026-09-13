---
title: 模糊数学笔记 3：扩展原理
date: 2020-03-01 17:43:43
tags:
categories:
  - Fuzzy Mathematics
mathjax: true
---

扩展原理实际上是将普通集合中的**函数映射**的概念扩展到了模糊集合上。

<!--more-->

## 1. 映射的象

### 1.1 定义

对于普通集合，设 $f:X\to Y,A\subseteq X$，$f(A)=\{f(x)|x\in A\}$ 称为 $A$ 的象

映射 $f$ 诱导了一个从 $P(X)$ 到 $P(Y)$ 的新映射

### 1.2 性质

该映射有以下性质：

- $A\subseteq B\Longrightarrow f(A)\subseteq f(B)$
- $f(\bigcup_{t\in T}A_t)=\bigcup_{t\in T}f(A_t)$
- $f(\bigcap_{t\in T}A_t)=\bigcap_{t\in T}f(A_t)$

## 2. 扩展原理

### 2.1 定义

对于模糊集合，设 $f:X\to Y,A\in \mathcal{F}(X)$，定义 $f(A)\in\mathcal{F}(Y)$ 为
$$
\forall y\in Y, \quad (f(A))(y)=\vee_{f(x)=y}A(x)
$$
映射 $f$ 诱导了一个从 $\mathcal{F}(X)$ 到 $\mathcal{F}(Y)$ 的新映射

### 2.2 性质

该映射有以下性质：

- $A,B\in \mathcal{F}(X),A\subseteq B\Longrightarrow f(A)\subseteq f(B)$
- $A_t\in\mathcal{F}(X),f(\bigcup_{t\in T}A_t)=\bigcup_{t\in T}f(A_t)$
- $A_t\in\mathcal{F}(X),f(\bigcap_{t\in T}A_t)=\bigcap_{t\in T}f(A_t)$
- $A\in\mathcal{F}(X),\alpha\in[0,1]$，则 $f(A_{\bar\alpha})=(f(A))_\bar\alpha$
- $A\in\mathcal{F}(X),\alpha\in[0,1]$，则 $f(A_{\alpha})\subseteq(f(A))_\alpha$

> **Remarks**：从 $X$ 到 $Y$ 的映射，只是对元素“换了个名称”，并没有改变对应的隶属度，因此映射 $f$ 与对模糊集的操作比如截集是可以交换顺序的