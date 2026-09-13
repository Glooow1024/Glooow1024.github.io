---
title: 统计推断(十) Elimination algorithm
date: 2020-02-03 20:04:00
tags:
  - 消去算法
categories: 
  - 统计推断
mathjax: true
---

消去算法

<!--more-->

---

## 1. Elimination algorithm

- 主要目的是为了计算边缘分布

- Reconstituted graph: 若消去的随机变量为 $x_k$，则所有与 $x_k$ 连接的随机变量会形成一个新的 clique

  ![elimination](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/elimination_1.PNG)

- 复杂度

  - Brute-force marginalization：$O\left(|\mathcal{X}|^N\right)$
  - Zig-zag elimination：$O\left(|\mathcal{X}|^{\sqrt{N}}\right)$

![elimination](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/elimination_2.PNG)

## 2. MAP elimination algorithm 

- 计算 MAP $\boldsymbol{x}^{*} \in \arg \max _{\boldsymbol{x} \in \mathcal{X}^{N}} p_{\mathbf{x}}(\boldsymbol{x})$
  $$
  \begin{array}{l}{\max _{x, y} f(x) g(x, y)=\max _{x}\left(f(x) \max _{y} g(x, y)\right)} \\ {\sum_{x, y} f(x) g(x, y)=\sum_{x}\left(f(x) \sum_{y} g(x, y)\right)}\end{array}
  $$

- example
  $$
  p_{\mathbf{x}}(\boldsymbol{x}) \propto \exp \left(x_{1} x_{2}-x_{1} x_{3}-x_{2} x_{4}+x_{3} x_{4}+x_{3} x_{5}\right)
  $$
  ![map_elimination](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/map_elimination_1.PNG)

- algorithm

  ![map_elimination](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2019/map_elimination_2.PNG)

- complexicity
  $$
  \text { overall cost } \leq|\mathcal{C}| \sum_{i}|\mathcal{X}|^{\left|S_{i}\right|+1} \leq N|\mathcal{C}||\mathcal{X}|^{\max _{i}\left|S_{i}\right|+1}
  $$
  