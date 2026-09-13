---
title: 模糊数学笔记 4：模糊关系
date: 2020-03-01 17:43:56
tags:
  - 模糊关系
  - 模糊矩阵
categories:
  - Fuzzy Mathematics
mathjax: true
---

## 1. 模糊关系

**定义**：模糊关系 $R$ 的隶属函数 $\mu_R:U\times V\to[0,1]$，其中 $\mu_R(x,y)$ 表示 $(x,y)$ 具有关系 $R$ 的程度

> **Remarks**：实际上模糊关系 $R$ 就是定义在一个笛卡尔积的论域 $U\times V$ 上的模糊关系，与之前介绍的普通的模糊关系并无太大差别。

<!--more-->

基本运算定义为：

- **并**：$\boldsymbol{\mu}_{R \cup S}(\boldsymbol{x}, \boldsymbol{y})=\boldsymbol{\mu}_{R}(\boldsymbol{x}, \boldsymbol{y}) \vee \boldsymbol{\mu}_{S}(\boldsymbol{x}, \boldsymbol{y})$
- **交**：$\boldsymbol{\mu}_{R \cap S}(\boldsymbol{x}, \boldsymbol{y})=\boldsymbol{\mu}_{R}(\boldsymbol{x}, \boldsymbol{y}) \wedge \boldsymbol{\mu}_{S}(\boldsymbol{x}, \boldsymbol{y})$
- **补**：$\mu_\bar{R}(x,y)=1-\mu_R(x,y)$
- **包含**：$\boldsymbol{R} \subseteq \boldsymbol{S} \Rightarrow \boldsymbol{\mu}_{R}(\boldsymbol{x}, \boldsymbol{y}) \leq \boldsymbol{\mu}_{S}(\boldsymbol{x}, \boldsymbol{y})$
- **相等**：$\boldsymbol{R} = \boldsymbol{S} \Rightarrow \boldsymbol{\mu}_{R}(\boldsymbol{x}, \boldsymbol{y}) = \boldsymbol{\mu}_{S}(\boldsymbol{x}, \boldsymbol{y})$

一些模糊关系有：

- **恒等模糊关系**：$R(x,y)=\mathbb{I}_{x=y}$
- **零模糊关系**：$O(x,y)=0$
- **全称模糊关系**：$E(x,y)=1$

## 2. 模糊矩阵

### 2.1 定义

对于有限论域 $U,V$，模糊矩阵的定义很容易可以获得 $R_{ij}=\mu_R(x_i,y_j)$

当 $R$ 的对角元素全部为 1 时，称为**模糊自反矩阵**

模糊矩阵对应于集合的运算定义为：

- **并**：$\boldsymbol{R} \cup \boldsymbol{S} \Leftrightarrow \boldsymbol{R} \cup \boldsymbol{S}=\left(\boldsymbol{r}_{i j} \vee \boldsymbol{s}_{i j}\right)$
- **交**：$\boldsymbol{R} \cap \boldsymbol{S} \Leftrightarrow \boldsymbol{R} \cup \boldsymbol{S}=\left(\boldsymbol{r}_{i j} \wedge \boldsymbol{s}_{i j}\right)$
- **补**：$R^c=(1-r_{ij})$
- **包含**：$\boldsymbol{R} \subseteq \boldsymbol{S} \Leftrightarrow\left(\boldsymbol{r}_{i j}\right) \leq\left(\boldsymbol{s}_{i j}\right)$
- **相等**：$\boldsymbol{R} = \boldsymbol{S} \Leftrightarrow\left(\boldsymbol{r}_{i j}\right) =\left(\boldsymbol{s}_{i j}\right)$

### 2.2 运算性质

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/4-prop1.PNG)

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/4-prop2.png)

### 2.3 截矩阵

**截矩阵**的定义为 $R_\lambda=(r_{ij}(\lambda))$，其中 $r_{ij}(\lambda)=\mathbb{I}_{r_{ij}\ge\lambda}$

> **Remarks**：截矩阵的定义对应着截集的概念，截集得到的是普通集合，响应的截矩阵也是布尔矩阵，完全没有不确定度。

### 2.4 模糊关系合成

**转置**：略

**模糊乘积**：设 $Q=(q_{ij})_{n\times m},R=(r_{ij})_{m\times t}$，定义 $S=QR\in\mathcal{F}_{n\times t}$，有 $S_{ik}=\vee_{j=1}^m(q_{ij}\wedge r_{jk})$

> **Remarks**：模糊乘积实际上表示了两个模糊关系的复合，即 $Q\in\mathcal{F}(U\times V),R\in\mathcal{F}(V\times W)$，最后合成了模糊关系 $S\in\mathcal{F}(U\times W)$。从公式上来看，模糊矩阵的乘积跟普通矩阵的乘积很像，只不过乘法换成了 $\wedge$，加法换成了 $\vee$。

模糊关系的合成具有以下性质：

![](https://raw.githubusercontent.com/Glooow1024/ImgHosting/master/hexo/2020/4-prop3.png)

## 3. 模糊关系性质

### 3.1 自反性、对称性、传递性

就像普通集合的关系一样，模糊集合有三个重要性质：自反性、对称性、传递性。

**自反性**：若 $\forall x\in U,\mu_R(x,x)=1$，则称 $R$ 满足**自反性**，相应的有模糊矩阵 $I\subseteq R$

**定理 1**：若 $A$ 为自反矩阵，则有
$$
I\subseteq A \subseteq A^2 \subseteq \cdots \subseteq A^n \subseteq \cdots
$$
**对称性**：若 $\forall x,y\in U,\mu_R(x,y)=\mu_R(y,x)$，则称 $R$ 满足**对称性**，相应的有模糊矩阵 $R^T=R$

**传递性**：$\mu_R(x,z)\ge\vee_y (\mu_R(x,y)\wedge\mu_R(y,z))$，则称 $R$ 满足**传递性**，相应的有模糊矩阵 $R^2\subseteq R$

**定理 2**：若 $Q$ 为传递矩阵，则有
$$
Q \supseteq Q^{2} \supseteq Q^{3} \supseteq \cdots \supseteq Q^{\mathbf{n}-1} \supseteq Q^{\mathbf{n}} \supseteq \cdots
$$

### 3.2 模糊相似关系与等价关系

**模糊相似关系**：$R\in F(U\times U)$，满足自反性和对称性 $\Longrightarrow I\subseteq R\subseteq R^2\subseteq \cdots\subseteq R^n\subseteq \cdots$

**模糊等价关系**：$R\in F(U\times U)$，满足自反性、对称性和传递性 $\Longrightarrow R=R^2=\cdots=R^n=\cdots$

> **定理**：$R$ 为等价关系 $\iff R_\lambda$ 为等价关系 $\forall \lambda\in[0,1]$
>
> **Proof**：若 $R_\lambda$ 为等价关系，则意味着 $\forall i,j,k$，若 $r_{ij}(\lambda)=1,r_{jk}(\lambda)=1 \Longrightarrow r_{ik}(\lambda)=1$。因此对于模糊矩阵来说，应有 $r_{ij}\ge\lambda,r_{jk}\ge\lambda \Longrightarrow r_{ik}\ge\lambda$。在此基础上易证充分必要性。
>
> **Remarks**：这个定理将模糊等价关系转化为普通等价关系，而普通等价关系可以很容易分类。

### 3.3 对称闭包与传递闭包

**对称闭包**：设 $A,\hat{A},B\in\mathcal{F}(U\times U)$，若 $A\subseteq\hat{A},A^T\subseteq\hat{A}$，且对任意包含 $A$ 的对称关系 $B$，都有 $\hat{A}\subseteq B$，则 $\hat{A}$ 为 $A$ 的对称闭包，记为 $s(A)=\hat{A}$。

> 实际上对称闭包就是包含 $A$ 的**最小的**对称关系，很容易的有 $s(A)=A\cup A^T$

**传递闭包**：$A\subseteq\hat{A},A^2\subseteq\hat{A}$，且任意包含 $A$ 的传递关系 $B$ 都有 $\hat{A}\subseteq B$，则 $\hat{A}$ 为 $A$ 的传递闭包，记为 $t(A)=\hat{A}$。

**传递闭包定理 1**：$t(A)=A\cup A^2 \cup\cdots\cup A^n\cup\cdots=\bigcup_{k=1}^\infty A^k$

**传递闭包定理 2**：$t(A)=\bigcup_{k=1}^n A^k$ （可以使用鸽巢原理，证明 $A^{n+1}\subseteq A^m,m\le n$）

**传递闭包定理 3**：相似矩阵 $R\in U_{n\times n}$ 的传递闭包是等价矩阵，且 $t(R)=R^n$

**传递闭包定理 4**：相似矩阵 $R\in U_{n\times n}$，则 $\forall m\ge n,t(R)=R^m$

**传递闭包定理 5**：相似矩阵 $R\in U_{n\times n}$，则 $\exist k\le n,t(R)=R^k$

> **Remarks**：
>
> - 定理 2 证明了传递闭包在实际中是可计算的
> - 定理 3-5 中对相似矩阵求传递闭包就得到了等价矩阵，对后面的模糊据类很有用，因为模糊等价矩阵可以与普通等价矩阵联系起来，而若想进行分类，则必须依托于等价关系。

