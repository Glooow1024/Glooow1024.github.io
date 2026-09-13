---
title: 模糊数学笔记 2：贴近度
date: 2020-03-01 17:42:59
tags:
  - 贴近度
categories:
  - Fuzzy Mathematics
mathjax: true
---

## 1. 贴近度

给定 $A,B,C\in \mathcal{F}(U)$，$\sigma(*,*)$ 满足以下几个条件时，被称为**贴近度**

1. $\sigma(A,A)=1$
2. $\sigma(A,B)=\sigma(B,A)$
3. 若 $A\subset B\subset C$，则 $\sigma(A,B)\ge\sigma(A,C),\sigma(B,C)\ge\sigma(A,C)$
4. $\sigma(U,\varnothing)=0$

<!--more-->

**严格贴近度**的定义为

1. $\sigma(A,B)=1 \iff A=B$
2. 上述 2.-4. 条

贴近度的例子：

- 严格贴近度：$\sigma(A, B)=\frac{\sum_{n} a_{n} \wedge b_{n}}{\sum_n a_{n} \vee b_{n}}$
- $\sigma(A, B)=1-t\left(\sum_{1}^{n}\left|a_{k}-b_{k}\right|^{p}\right)^{q}$
- $\sigma(A, B)=\frac{\sum_{n} a_{n} \wedge b_{n}}{\sum_n (a_{n} + b_{n})/2}$
- $\sigma(A, B)=\exp\left(-t\left(\sum_{1}^{n}\left|a_{k}-b_{k}\right|^{p}\right)^{q}\right)$

## 2. 内外积

### 2.1 定义

**内积**：$A,B\in\mathcal{F}(U)$，称 $A\circ B=\underset{u \in U}{\vee}(A(u) \wedge B(u))$ 为 $A,B$ 的内积

**外积**：$A,B\in\mathcal{F}(U)$，称 $A \hat{\circ} B=\underset{u \in U}{\wedge}(A(u) \vee B(u))$ 为 $A,B$ 的外积

> **Remarks**：内外积本身并不是用来表述两个集合的相似程度的，就像向量的内外积，还与向量自身的模长有关。

### 2.2 性质

- $A \hat{\circ} B = A^c\circ B^c,(A\circ B)^c=A^c \hat{\circ} B^c$
- $A {\circ} B\le \bar{a}\wedge\bar{b},A \hat{\circ} B\ge\underline{a}\vee\underline{b}$
- $A\circ A=\bar{a},A \hat{\circ} A=\underline{a}$
- $\underset{B \in \mathcal{F}(U)}{\vee}(A \circ B)=\bar{a}, \quad \underset{B \in \mathcal{F}(U)}{\wedge}\left(A {\hat{\circ}} B\right)=\underline{a}$
- $A \subseteq B \Rightarrow A \circ B=\bar{a}, A{\hat{\circ}} B=\underline{b}$
- $A \circ A^{c} \leq \frac{1}{2}, \quad A{\hat{\circ}} B \geq \frac{1}{2}$
- $A \subseteq B \Rightarrow A \circ C \leq B \circ C, A{\hat{\circ}} C \leq B{\hat{o}} C$

## 3. 格贴近度

给定F集A，让F集B靠近A，会使内积增大而外积减少。即当内积较大且外积较小时，A与B比较贴近。 所以，以内外积相结合的“格贴近度”来刻 划两个F集的贴近程度。

**格贴近度**：$N_{1}(A, B)=(A \circ B) \wedge\left(A\hat{\circ} B\right)^{c}$

格贴近度有以下性质

- $0\le N_1(A,B)\le1$
- $N_1(A,B)=N_1(B,A)$
- $N_1(A,A)=\bar{a}\wedge(1-\underline{a})$
- $A \subseteq B \subseteq C \Rightarrow N_1(A, C) \leq N_1(A, B) \wedge N_1(B, C)$

> **Remarks**：注意根据第 3 条性质可知，格贴近度并不适合描述两个模糊集的相似程度，比如 $N_1(U,U)=0$

