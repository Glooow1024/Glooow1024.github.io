---
title: 统计推断(三) Exponential Family
date: 2020-02-03 19:57:00
tags:
  - 指数族
categories: 
  - 统计推断
mathjax: true
---

指数族

<!--more-->

## 1. Exponential family

- Definition
  
  - PDF: $p(y;x)=\exp(\lambda(x)^T t(y)-\alpha(x)+\beta(y))$
    $y\sim \varepsilon(x;\lambda(\cdot),t(\cdot),\beta(\cdot))$
  - nature statistic: $t(y)$
  - nature parameter: $\lambda(x)$
  - log-partition function: $\alpha(x)$
  - partition function: $Z(x)=\exp(\alpha(x))$
  - distribution: $\exp(\beta(y))$
  
- **正则条件(regular)**：若分布族中的任意一个分布 $p(y;x)$ 都有其支集(support)与 x 无关，则为正则

  - 实质上是要求 CRB 正则条件中**求导和积分可换序** 
    $$
    \mathbb{E}\left[\frac{\partial}{\partial x}\ln p(y;x)\right]=\int\frac{\partial}{\partial x}p(y;x)dy = \frac{\partial}{\partial x}\int_a^b p(y;x)dy = 0
    $$

- 指数分布族可以有多种获得方式

  - 很多分布本身可以写成指数分布族形式
    - Bernulli distribution: $y\sim \mathcal{B}(x)$

    $$
    p(y;x)=x^y (1-x)^{(1-y)} \\
    \ln p(y;x)=\left(\ln(\frac{x}{1-x})\right)y-(-\ln(1-x))
    $$
    
    - Gaussian $y=[y_1,y_2]^T\sim \mathcal{N}(x,1)$
    $$
    p(y;x)=\frac{1}{\sqrt{2\pi}}\exp\left((y_1+y_2)x-x^2-\frac{y_1^2+y_2^2}{2}\right)
    $$
    
  - 多个分布的**几何均值**
    $$
    p(y;x)=\frac{p_1^x(y)*p_2^{(1-x)}(y)}{Z(x)} \\
    \ln p(y;x)=x\ln\left(\frac{p_1(y)}{p_2(y)}\right)-\ln Z(x)+\ln p_2(y)
    $$
  
    - 例如 $p_1(y)\sim \mathcal{B}(\frac{1}{1+e^{-1}}),  p_2(y)\sim \mathcal{B}(1/2)$
      $$
      p(y;x)=(\frac{1}{1+e^{-1}})^{xy}(\frac{e^{-1}}{1+e^{-1}})^{x(1-y)}(1/2)^{(1-x)}\sim \mathcal{B}(\frac{1}{1+e^{-x}}) \\
    \frac{p(y=1;x)}{p(y=0;x)}=e^x
      $$
    
  - **Tilting**
    $$
    p(y;x)=\frac{p(y)e^{xy}}{Z(x)} \\
    \ln p(y;x)=xy - \ln Z(x) + \ln p(y)
    $$
  
    - 例如 $p(y)\sim \mathcal{N}(0,1)$，$p(y;x)\sim \mathcal{N}(x,1)$
  
- **linear exponential family**

  - 定义：$t(x)=x$，$\ln p(y;x)=x\ t(y) - \alpha(x)+\beta(y)$
  - 性质：$\dot{\alpha}(x)=\mathbb{E}[t(y)], \ \ \dot{\dot{\alpha}}(x)=\mathbb{E}[t^2(y)]-\mathbb{E}[t(y)]^2=Var(t(y)) = J_y(x)$

  > **Proof**：
  > $$
  > \begin{align}
  > Z(x) &= e^{\alpha(x)}=\int e^{x t(y)+\beta(y)}dy \\
  > \frac{\partial}{\partial x}Z(x) &= e^{\alpha(x)}\cdot \dot\alpha(x) = \int t(y)e^{xt(y)+\beta(y)}dy \\
  > \dot{\alpha}(x) &= \int t(y)p(y;x)dy = \mathbb{E}[t(y)]
  > \end{align}
  > $$
  >
  > $$
  > \dot{\dot{\alpha}}(x)=\int t(y)\cdot p(y;x)\cdot (t(y)-\dot{\alpha}(x))dy \\
  > J_y(x) = \mathbb{E}\left[-\frac{\partial^2}{\partial x^2} \ln p(y;x)\right]=\dot{\dot{\alpha}}(x)
  > $$
  >
  > 

- 指数族分布与有效统计量(efficient statistics)

  - **必要条件**：若有效统计量存在，则可以写成指数族分布形式，且有
    $$
    t(x)=\int^x J_y(u)du, \ \ \ \alpha(x)=\int^x u J_y(u) du
    $$

  > **Proof**：
  > $$
  > \begin{align}
  > \hat {x}_{eff}(y) &= x+\frac{1}{J_y(x)}\frac{\partial}{\partial x}\ln p(y;x) \\
  > \frac{\partial}{\partial x}\ln p(y;x) &= J_y(x)\hat{x}_{eff}(y) - x J_y(x) \\
  > \ln p(y;x) &= \int^x J_y(u)du \cdot \hat{x}_{ML}(y) - \int^x u J_y(u) du
  > \end{align}
  > $$
  
  - **充分条件**：对于**线性指数分布族**，若有 $J_y(x)$ 不依赖于 x，也即 $J_y(x)$ 等于一个常数时，有效统计量存在
  
  > **Proof**：$J_y(x)=J$
  > $$
  > \dot{\dot{\alpha}}(x)=J, \ \ \ \dot{\alpha}(x)=Jx-c \\
  > \hat x_{eff}(y) = x + \frac{1}{J}\frac{\partial}{\partial x}\ln p(y;x) = x + \frac{1}{J} (t(y)-\dot{\alpha}(x)) = x + \frac{1}{J}(t(y)-Jx+c)=\frac{t(y)}{J}+\frac{c}{J}
  > $$
  > 由于 
  > $$
  > \frac{\partial}{\partial x}\ln p(y;x)|_{x=\hat x_{ML}} = 0 = t(y) - \dot{\alpha}(x)|_{x=\hat x_{ML}}
  > $$
  > 有
  > $$
  > \hat x_{eff}(y) = c/J + \frac{1}{J}\dot{\alpha}(x)|_{x=\hat x_{ML}} = \hat x_{ML}(y)
  > $$
  

## 2. Sufficient statistics

### 2.1 Non-Bayesian case

- Definition：t(y) 是关于分布 $p_{\mathsf{y}}(\cdot;x)$ 的充分统计量，如果 $p(y|t(y);x)$ 与 x 无关

> **Theorem 1**(likelihood characterization)：
>
> $t(y)$ is sufficient w.r.t $p(y;x)$ $\iff \ \frac{p_{y}(y;x)}{p_t(t(y);x)}$ doesn't depend on x, for all x and y
>
> **Proof**：omit...
>
> **Theorem 2**(Neyman Factorization theorem)：
>
> $t(y)$ is sufficient w.r.t $p(y;x)$ $\iff \ 存在a(\cdot,\cdot)和b(\cdot)使得 \ \  p(y;x)=a\left(t(y),x\right) \cdot b(y)$
>
> **Proof**：omit...

- **minimum sufficient statistic**：$t^*$ 是 minimal 的，如果对任意其他充分统计量 t ，都存在 g() 使得 $t^*=g(t)$
- **complete**：$t^*$ 是 complete 的如果对任意函数 $\phi(\cdot)$，有 $E[\phi(t^*(y))]=0 \ \ \forall x \iff \phi(\cdot) \equiv 0$

> **Theorem**：complete $\Longrightarrow$ minimal
>
> **Proof**：假设 t 为complete，s 为 minimal，存在 $s=g(t)$，$E[t]=E\left[E\left[t|s=s\right]\right]$
>
> $E[t|s=s]=f(s)=f(g(t))=\tilde{f}(t)$
>
> 取 $\phi(t)=t-\tilde{f}(t)$，有 $E[\phi(t)] = 0$
>
> 根据 complete 的定义，有 $\phi(t)\equiv0 \Longrightarrow t = \tilde{f}(t)=f(s)$
>
> 故 t 也是 minimal

### 2.2 Bayesian case

- Definition：t(y) 是关于分布 $p_{\mathsf{y,x}}(\cdot,\cdot)$ 的充分统计量，如果 $p_{\mathsf{y|t,x}}(y|t(y),x)=p_\mathsf{y|t}(y|t(y))$ 与 x 无关

> **Theorem**(Belief characterization)：
>
> $t(y)$ is sufficient w.r.t $p(y,x)$ $\iff \ p(x|y)=p(x|t(y))$, for all x and y
>
> **Proof**：omit...
>
> **Theorem**(Neyman Factorization theorem)：
>
> $t(y)$ is sufficient w.r.t $p(y,x)$ $\iff \ p(y|x)=p(t(y)|x)\cdot p(y|t(y))$, for all x and y
>
> **Proof**：omit...

## 3. Conjugate priors

- Idea: Given a model $p_\mathsf{y|x}$, look for a family of prior $p_\mathsf{x}$ such that the induced posterior $p_\mathsf{x|y}$ also in this family
- Definition: a family of distribution $q(\cdot;\theta)$ is **conjugate** to a model $p_{y|x}$ if 
  - $p_{y|x}(y_1,...,y_N|x) \propto q(x;\theta)$
  - $q(x;\theta_1)q(x;\theta_2)\propto q(x;\theta_3)$
- **Theorem**: 对于采样数 N，联合分布 $p^N_{y|x}()$ 有充分统计量，且其维度不依赖于 N，则对该模型存在共轭先验分布

