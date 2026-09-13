---
title: 模糊数学笔记 6：模糊综合评判
date: 2020-03-13 11:08:31
tags:
  - 模糊综合评判
categories:
  - Fuzzy Mathematics
mathjax: true
---

假如我们现在设计了一种服装，想要调研一下这种服装的受欢迎程度，该怎么办呢？

首先是怎么表示受欢迎程度呢？我们可以简单分为三个等级：受欢迎、一般欢迎、不受欢迎，由于不同的人感受可能不同，结合前面模糊集的概念，我们可以引入隶属度，比如调查后得到 50% 的人喜欢，30%一般，20%不喜欢，我们就可以取隶属度分别为 0.5，0.3，0.2。

当然这样太粗糙了，我们知道评价一个衣服的因素有很多，比如样式、耐穿性、价格等，而不同因素即使对同一个人的值观感受也是不一样的，比如有的人很喜欢这个样式，但是摸了摸钱包，发现这个衣服是这么的难看！所以要想更好的评价，我们需要对每一个指标分别评估一下受欢迎程度，然后把各种因素综合起来。这就是模糊综合评判要做的事情。

<!--more-->

## 1. 一级模糊综合评判

现在我们把上面描述的过程规范化。我们有**因素集** $U=\{u_1,...,u_n\}$，**评语集** $V=\{v_1,...,v_m\}$。

综合评判的步骤是什么呢？

1. 首先要对每个因素分别进行单因素评判，也就是获得 $r_i = (r_{i1},\cdots,r_{im})$
2. 然后把 $n$ 个因素的评判指标拼接成一个 $n\times m$ 的矩阵，可以获得综合评判矩阵 $R=(r_1^T,\cdots,r_n^T)^T$
3. 之后需要对所有因素进行综合评判，因此可以设置一个权重 $A=(a_1,...,a_n)$，再根据算子 $M(\cdot,\cdot)$ 计算 $B=A\circ R=M(A,R)$，这个过程实际上就类似于一个加权的过程
4. 上一步结束后实际上得到的还是一个向量，要想最后给一个总的评价指标，可以对该向量再进行一次加权或者其他操作，获得一个总的指标。

下面我对第 3，4 条再详细解释一下。

算子 $M(\cdot,\cdot)$ 可以取很多种形式，比如

- $M(\wedge,\vee) \Longrightarrow \boldsymbol{b}_{j}=\max \left\{\left(\boldsymbol{a}_{i} \wedge \boldsymbol{r}_{i j}\right), \mathbf{1} \leq \boldsymbol{i} \leq \boldsymbol{n}\right\}(\boldsymbol{j}=\mathbf{1}, \boldsymbol{2}, \cdots, \boldsymbol{m})$
- $M(\cdot,\vee) \Longrightarrow \boldsymbol{b}_{j}=\max \left\{\left(\boldsymbol{a}_{i} \cdot \boldsymbol{r}_{i j}\right), \mathbf{1} \leq \boldsymbol{i} \leq \boldsymbol{n}\right\}(\boldsymbol{j}=\mathbf{1}, \boldsymbol{2}, \cdots, \boldsymbol{m})$
- $M(\cdot,+) \Longrightarrow \boldsymbol{b}_{j}=\sum_i \left(\boldsymbol{a}_{i} \cdot\boldsymbol{r}_{i j}\right), (\boldsymbol{j}=\mathbf{1}, \boldsymbol{2}, \cdots, \boldsymbol{m})$
- $M(\wedge,\bigoplus)=\boldsymbol{b}_{j}=\min \left\{1, \sum_{i=1}^{n}\left(a_{i} \wedge r_{i j}\right)\right\}(j=1,2, \cdots, m)$
- $M(\wedge,+) \Longrightarrow \boldsymbol{b}_{j}=\sum_i \left(\boldsymbol{a}_{i} \wedge\frac{\boldsymbol{r}_{i j}}{r_0}\right), (\boldsymbol{j}=\mathbf{1}, \boldsymbol{2}, \cdots, \boldsymbol{m}),r_0=\sum_k r_{kj}$

不同的算子形式有不同特点，也适用于不同情况。

第 3 步经过 $M(,)$ 算子之后，我们得到了模糊评判向量 $B$，要获得综合结论也可以有不同的方式

- $u=\max{(b_1,...,b_n)}$
- $u=\sum_i \mu_i b_i / \sum_i b_i$

## 2. 多级模糊综合评判

有时候对一件事物的评价有很多指标，这些指标又可以分为几大类，比如对高校的评分，可以包括

- 教学：教学下面又可以分为师资力量、教学设施、学生质量等等
- 科研：...... 可以有很多指标
- 图书馆、后勤 ......

这个时候我们可以划分为多个层次依此评判综合，其步骤可以描述为以下形式：

1. 将原始因素集 $U=\{u_1,...,u_n\}$ 划分为若干组 $U=\bigcup_i U_i,U_i\cap U_j=\varnothing$
2. 对每组 $U_i$ 分别进行模糊评判，得到 $B_i$
3. 对总的评判矩阵 $R=(B_1^T,...,B_k^T)^T$ 再次进行综合评判，得到 $B=A\circ R$，然后可以得到总的综合评语

