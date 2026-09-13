---
title: 【高等数值分析】函数逼近
date: 2022-01-07 23:47:30
tags:
  - 多项式逼近
  - Pade逼近
  - Lengdre多项式
  - Chebyshev多项式
categories:
  - 高等数值分析
mathjax: true
---

## 1. 预备理论

函数插值当中我们只有几个离散的的插值节点及其函数值，在函数逼近里我们考虑的是有一个 $f(x),x\in[a,b]$ 已知，但是希望用一组简单的基函数 $\{\phi_0,...,\phi_n\}$ 逼近 $f(x)$。

<!--more-->

首先定义加权内积为 $\langle f,g\rangle_\rho = \int_a^b \rho(x)f(x)g(x)dx$，其中 $\rho(x)\ge0$ 为权函数。相应的可以导出范数定义为 $\Vert f\Vert_\rho = \sqrt{\langle f,f\rangle_\rho}$。后面为了符号简洁都默认省略下标 $\rho$。

一般考虑最佳平方逼近问题，即 $\min_{s\in\Phi} \Vert f-s\Vert$，其中 $\Phi$ 为某个函数空间，一般为代数多项式、三角多项式或有理多项式组。



## 2. 多项式逼近

### 2.1 总体框架

若 $\Phi=\operatorname{span}\{\phi_0,...,\phi_n\}$，$\{\phi_j,j=0,...,n\}$ 为一组线性无关的的基函数，那么 $\forall s\in\Phi$ 可以表示为 $s(x) = \sum_{j=0}^n a_j\phi_j(x)$，最佳平方逼近问题变为
$$
\min_{s\in\Phi} F(a_0,...,a_n) \triangleq \Vert f-\sum a_j\phi_j \Vert^2
$$
令 $\partial F/\partial a_k=0$ 即可得到下面的法方程，再求解线性方程组即可得到 $a_k$
$$
\sum_{i=0}^n \langle \phi_i, \phi_k\rangle a_i = \langle f,\phi_k\rangle, \quad k=0,1,...,n
$$
除了前面的求导得到法方程，还可以利用**Galerkin条件**（实际上与法方程等价）
$$
\langle f-s^{\ast}, s\rangle = 0, \quad s\in\Phi
$$
根据前面的结论，要想求得 $a_k$ 需要解法方程 ${\Psi}\boldsymbol{a}=\boldsymbol{b}$，为了进一步简化该问题，可以选择基函数 $\phi_j$ 相互正交，那么 $\Psi$ 就会退化为对角阵，并且此时有
$$
s^{\ast} = \sum_{k=0}^n \frac{\langle f,\phi_k\rangle}{\Vert \phi_k\Vert^2} \phi_k
$$
后面的关键就是如何获得相互正交的一组基函数？

### 2.2 正交多项式

答案就是Gram-Schmidt正交化。对于多项式基函数 $\phi_k\in{\mathcal P}_k$，只需要对 $\{1,x,x^2,...\}$ 逐次进行Gram-Schmidt正交化即可。
$$
\phi_k = x^k - \sum_{j=0}^{k-1} \frac{\langle x^k,\phi_j\rangle}{\langle \phi_j,\phi_j\rangle} \phi_j
$$
后面将介绍几种不同的正交多项式，他们的区别就在于支撑区间 $[a,b]$ 和权函数 $\rho(x)$ 的不同。

#### 2.2.1 Lengdre多项式

$\rho(x)=1,x\in[-1,1],\phi_0(x)=1$，有如下性质：

- 递推关系：$(n+1)P_{n+1}(x) = (2n+1)xP_n(x) - n P_{n-1}(x)$
- 奇偶性：$P_n(-x) = (-1)^n P_n(x)$
- $[-1,1]$ 上Lengdre多项式与 $f\equiv0$ 的平方误差最小

#### 2.2.2 Chebyshev多项式

$\rho(x)=1/\sqrt{1-x^2},x\in[-1,1],\phi_0=1$，有如下性质：

- 递推关系：$T_{n+1}(x) = 2xT_n(x)-T_{n-1}(x)$
- 奇偶性：$P_n(-x) = (-1)^n P_n(x)$
- $T_n(x)$ 在 $(-1,1)$ 上有 $n$ 个不同的零点 $x_k=\cos \frac{(2k-1)\pi}{2n},k=1,2,..,n$

#### 2.2.3 Lagurre多项式

$\rho(x)=e^{-x},x\in(0,+\infty),\phi_0=1$

#### 2.2.4 Hermite多项式

$\rho(x)=\exp(-x^2),x\in(-\infty,\infty),\phi_0=1$



## 3. Pade逼近

Pade逼近是用有理多项式 $R_{n,m}(x) = \frac{R_n(x)}{Q_m(x)}$ 逼近 $f(x)$，其中 $P_n\in{\mathcal P}_n,Q_m\in{\mathcal P}_m$。如果对 $f(x)$ 做 Taylor 展开的话，能得到
$$
f(x)Q_m(x) - P_n(x) = \sum_{i=0}^\infty c_ix^i 
$$
理论上有 $n+m+1$ 个待定系数，因此可以列出方程 $c_0=c_1=\cdots=c_{n+m}=0$，即可得到 $R_{n,m}$。



## 4. 最小二乘

如果给定了很多个节点函数值 $f(x_i)$，但是不要求满足插值条件 $f(x_i)=\phi(x_i)$，而只要求最小化平方误差 $\sum_i (f(x_i)-\phi(x_i))^2$，那么就是最小二乘问题
$$
\min \Vert b-Ax\Vert \Rightarrow x^{\ast} = (A^TA)^{-1}A^Tb
$$
也可以对 $A$ 做QR分解，那么就有 $Rx^{\ast}=Q^Tb$。



