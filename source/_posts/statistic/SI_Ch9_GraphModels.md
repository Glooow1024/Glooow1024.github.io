---
title: 统计推断(九) Graphical models
date: 2020-02-03 20:03:00
tags:
  - 图模型
  - 因子图
  - DAG
categories: 
  - 统计推断
mathjax: true
---

## 1. Undirected graphical models(Markov random fields)

- 节点表示随机变量，边表示与节点相关的势函数
  $$
  p_{\mathbf{x}}(\mathbf{x}) \propto \varphi_{12}\left(x_{1}, x_{2}\right) \varphi_{13}\left(x_{1}, x_{3}\right) \varphi_{25}\left(x_{2}, x_{5}\right) \varphi_{345}\left(x_{3}, x_{4}, x_{5}\right)
  $$
  ![undirected_graph](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/undirected_graph.PNG)

- **clique**：全连接的节点集合

- **maximal clique**：不是其他 clique 的真子集

> **Theorem (Hammersley-Cliﬀord) **: A strictly positive distribution $p_{\mathsf{x}}(\mathbf{x})>0$ satisﬁes the graph separation property of undirected graphical models if and only if it can be represented in the factorized form 
> $$
> p_{\mathsf{x}}(\mathbf{x}) \propto \prod_{\mathcal{A} \in \mathcal{C}} \psi_{\mathbf{x}_{\mathcal{A}}}\left(\mathbf{x}_{\mathcal{A}}\right)
> $$

- **conditional independence**：$\mathbf{x}_{\mathcal{A}_{1}} \perp \mathbf{x}_{\mathcal{A}_{2}} | \mathbf{x}_{\mathcal{A}_{3}}$

<!--more-->

## 2. Directed graphical models(Bayesian network)

- 节点表示随机变量，有向边表示条件关系
  $$
  p_{\mathrm{x}_{1}, \ldots, \mathrm{x}_{n}}=p_{\mathrm{x}_{1}}\left(x_{1}\right) p_{\mathrm{x}_{2} | \times_{1}}\left(x_{2} | x_{1}\right) \cdots p_{\mathrm{x}_{n} | x_{1}, \ldots, x_{n-1}}\left(x_{n} | x_{1}, \ldots, x_{n-1}\right)
  $$
  ![directed_graph](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/directed_graph.PNG)

- Directed acyclic graphs (**DAG**) 

- Fully-connected DAG 

- **conditional independence**：$\mathbf{x}_{\mathcal{A}_{1}} \perp \mathbf{x}_{\mathcal{A}_{2}} | \mathbf{x}_{\mathcal{A}_{3}}$

  ![conditional_independence](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/conditional_independence.PNG)

- Bayes ball algorithm 

  - primary shade: $\mathcal{A_3}$ 中的节点
  - secondary shade: primary shade 的节点，以及 secondary shade 的父节点

  ![1574319393095](C:\Users\1\AppData\Roaming\Typora\typora-user-images\1574319393095.png)

## 3. Factor graph

- 有 variable nodes 和 factor nodes，是 bipartitie graph
  $$
  p_{\mathbf{x}}(\mathbf{x}) \propto \prod_{j} f_{j}\left(\mathbf{x}_{f_{j}}\right)
  $$
  ![factor_graph](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/factor_graph.PNG)

- 因子图比 directed graph 和 undirected graph 的表示能力更强，比如 $p(x)=\frac{1}{Z}\phi_{12}(x_1,x_2)\phi_{13}(x_1,x_3)\phi_{23}(x_2,x_3)$

- 因子图可以与 DAG 相互转化(根据 $x_1,...,x_n$ 依次根据 conditional independence 决定父节点)，DAG又可以转化为 undirected graph

## 4. Measuring goodness of graphical representations 

- 给定分布 D 和图 G，他们之间没必要有联系
- $CI(D)$：the set of conditional independencies satisﬁed by $D$
- $CI(G)$： the set of all conditional independencies implied by $G$
- **I-map**：$C I(\mathcal{G}) \subset C I(D)$
- **D-map**: ：$C I(\mathcal{G}) \supset C I(D)$
- **P-map**：$C I(\mathcal{G}) = C I(D)$
- minimal I-map: Aminimal I-mapisanI-mapwiththepropertythatremovinganysingle edge would cause the graph to no longer be an I-map.
  **Remarks**: G 中去掉一个边会使该 map 中有更多的 conditional independence，也即 $CI(G)$ 更大，更不易满足 I-map条件。I-map 可以表示分布 D，但是 D-map 不能

