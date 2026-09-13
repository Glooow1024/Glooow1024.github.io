---
title: 统计推断(十一)  Sum-product algorithm
date: 2020-02-03 20:05:00
tags:
  - 信念传播算法
  - 和积算法
categories: 
  - 统计推断
mathjax: true
---

和积算法/信念传播算法

<!--more-->

## 1. Sum-product(Message passing) on trees

- 目的是为了计算边缘分布，相比于 elimination 的优势在于可以用较少的计算次数计算所有随机变量的边缘分布，关键在于复用 message

- algorithm

  - Step 1: Compute messages 
    $$
    m_{i \rightarrow j}\left(x_{j}\right)=\sum_{x_{i}} \phi_{i}\left(x_{i}\right) \psi_{i j}\left(x_{i}, x_{j}\right) \prod_{k \in N(i) \backslash\{j\}} m_{k \rightarrow i}\left(x_{i}\right)
    $$

  - Step 2: Compute marginals 
    $$
    p_{\times i}\left(x_{i}\right) \propto \phi_{i}\left(x_{i}\right) \prod_{j \in N(i)} m_{j \rightarrow i}\left(x_{i}\right)
    $$

- Remarks

  - 什么是 message？

  - tree 的一枝表示什么？实际上就是一个**条件分布**，如下图中实际上就是 $m_2(x_1)=\sum_{x_2,x_4,x_5}p(x_2,x_4,x_5|x_1)$

    ![message](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/message.PNG)

## 2. Sum-product algorithm on factor trees

- algorithm

  - Message from variable to factor 
    $$
    m_{i \rightarrow a}\left(x_{i}\right)=\prod_{b \in N(i) \backslash\{a\}} m_{b \rightarrow i}\left(x_{i}\right)
    $$

  - Message from factor to variable 
    $$
    m_{a \rightarrow i}\left(x_{i}\right)=\sum_{\mathbf{x}_{N(a) \backslash\{i\}}} f_{a}\left(x_{i}, \mathbf{x}_{N(a) \backslash\{i\}}\right) \prod_{j \in N(a) \backslash\{i\}} m_{j \rightarrow a}\left(x_{j}\right)
    $$

## 3. Max-Product for undirected tree/factor tree

## 4. Parallel Max-Product 

- 所有节点同时运算，至多需要 d(最长path的length) 次迭代即可

- trick: 整体的减少乘法次数
  $$
  \begin{align}
  \tilde{m}_{i}^{t}\left(x_{i}\right)&=\prod_{k \in N(i)} m_{k \rightarrow i}^{t}\left(x_{i}\right) \\
  m_{i \rightarrow j}^{t+1}\left(x_{j}\right)&=\max _{x_{i} \in \mathcal{X}} \phi_{i}\left(x_{i}\right) \psi_{i j}\left(x_{i}, x_{j}\right) \frac{\tilde{m}_{i}^{t}\left(x_{i}\right)}{m_{j \rightarrow i}^{t}\left(x_{i}\right)}
  \end{align}
  $$
  