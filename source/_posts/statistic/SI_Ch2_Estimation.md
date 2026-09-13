---
title: 统计推断(二) Estimation Problem
date: 2020-02-03 19:56:00
tags:
  - 参数估计
categories: 
  - 统计推断
mathjax: true
---

参数估计

<!--more-->

## 1. Bayesian parameter estimation

- Formulation
  - Prior distribution $p_{\mathsf{x}}(\cdot)$
  - Observation $p_{\mathsf{y|x}}(\cdot|\cdot)$
  - Cost $C(a,\hat a)$

- Solution
  - $\hat x(\cdot) = \arg\min_{f(\cdot)} \mathbb E[C(x,f(y))]$
  - $\hat{\mathbf{x}}(\mathbf{y})=\underset{\mathbf{a}}{\arg \min } \int_{\mathcal{X}} C(\mathbf{x}, \mathbf{a}) p_{\mathbf{x} | \mathbf{y}}(\mathbf{x} | \mathbf{y}) \mathrm{d} \mathbf{x}$

- Specific case

  - **MAE**(Minimum absolute-error)

    - $C(a,\hat a)=|a-\hat a|$
    - $\hat x$ is the **median** of the belief $p_{\mathsf{x|y}}(x|y)$

  - **MAP**(Maximum a posteriori)

    - $$C(a,\hat a) = \left\{
      \begin{array}{ll}{1,} & {|a-\hat a|>\varepsilon} \\ 
      {0,} & {otherwise}\end{array}\right.$$
    - $\hat x_{MAP}(y) = \arg \max_a p_{\mathsf{x|y}}(a|y)$

  - **BLS**(Bayes’ least-squares)

    - $C(a,\hat a)=||a-\hat a||^2$

    - $\hat x_{BLS}(y) = \mathbb E [\mathsf{x|y}]$

    - proposition

      - unbiased:  $b = \mathbb E[\mathsf{e(x,y)}]=E[\mathsf{\hat x(y)-x}]=0$

      - 误差的协方差矩阵就是 **belief**（后验分布？）的协方差阵的期望
        $$
        \Lambda_{BLS}=\mathbb E[\mathsf{\Lambda_{x|y}(y)}]
        $$
  
- Orthogonality
    $$
        \hat x(\cdot)\ is\ BLS \iff \mathbb E\left[ \mathsf{[\hat x(y)-x]g^T(y)}\right]=0
    $$
    
      > **Proof**:  omit

## 2. Linear least-square estimation

- Drawback of BLS $\hat x_{BLS}(y)=E[x|y]$

  - requires posterior $p(x|y)$, which needs $p(x)$ and $p(y|x)$
  - calculating posterior is complicated
  - estimator is **nonlinear**

- Definition of LLS

  - $\hat {\mathbf{x}}_{LLS}(y) = \arg \min\limits_{f(\cdot) \in \mathcal{B}} E\left[||\mathsf{x-f(y)}||^2\right] \\ \mathcal{B}=\{f(\cdot):f(y)=Ay+d\}$
  - 注意 $\hat {\mathbf{x}}(\mathsf{y})$ 是一个随机变量，是关于 $\mathsf{y}$ 的一个函数
  - LLS 与 BLS 都是假设 x 为一个随机变量，有先验分布，不同之处在于 LLS 要求估计函数为关于观测值 y 的线性函数，因此 LLS 只需要知道二阶矩，而 BLS 需要知道后验均值

- Property

  - Orthogonality
    $$
    \hat {\mathbf{x}}(\cdot)\ is\ LLS \iff E[\hat {\mathbf{x}}(\mathsf{y})-\mathsf{x}]=0\ \ and\ \ E[(\hat {\mathbf{x}}(\mathsf{y})-\mathsf{x})\mathsf{y}^T]=0
    $$

  - 推论：由正交性可得到

    - $\hat x_{LLS}(y)=\mu_X+\Lambda_{xy}\Lambda_y^{-1}(y-\mu_y)$
    - $\Lambda_{\mathrm{LLS}} \triangleq \mathbb{E}\left[\left(\mathbf{x}-\hat{\mathbf{x}}_{\mathrm{LLS}}(\mathbf{y})\right)\left(\mathbf{x}-\hat{\mathbf{x}}_{\mathrm{LLS}}(\mathbf{y})\right)^{\mathrm{T}}\right]=\Lambda_{\mathrm{x}}-\Lambda_{\mathrm{xy}} \Lambda_{\mathrm{y}}^{-1} \Lambda_{\mathrm{xy}}^{\mathrm{T}}$
  
  > **Proof**: x 可以是向量
  >
  > $\Longrightarrow$：反证法
  >
  > 1. suppose $E[\hat x_{LLS}(y)-x]=\mathbb{b} \ne 0$，take $\hat x'=\hat x_{LLS} - b$
  >    then $E\left[||\hat x' - x||^2\right]=E\left[||\hat x - x||^2\right]-b^2 < E\left[||\hat x - x||^2\right]$
  >    与 LLS 的定义矛盾；
  >
  > 2. $e=\hat x(y)-x$
  >    Take $\hat x' = \hat x_{LLS} - \Lambda_{ey}\Lambda_y^{-1}(y-\mu_y)$
  >    $$
  >    \begin{align}
  >    M &= E\left[(\hat x' -x)(\hat x' -x)^T \right] \\
  >    &= E\left[(\hat x-x)(\hat x-x)^T\right]-\Lambda_{ey}\Lambda_y^{-1}\Lambda_{ey}^T
  >    \end{align}
  >    $$
  >    由于 $E\left[||\mathsf{x-f(y)}||^2\right] = tr\{M\}$，LLS 的 MSE 应当最小
  >    由于 $\Lambda_y$ 正定，因此应有 $\Lambda_{ey}\Lambda_y^{-1}\Lambda_{ey}^T=0$
  >    故 $E\left[(\hat x-\mu_x)(y-\mu_y)^T \right]=0 \Longrightarrow E[(\hat {\mathbf{x}}(\mathsf{y})-\mathsf{x})\mathsf{y}^T]=0$
  >
  > $\Longleftarrow$：suppose another linear estimator $\hat x'$
  > $$
  > \begin{align}
  > E\left[(\hat x'-x)(\hat x'-x)^T\right] &= E[(\hat x'-\hat x+\hat x-x)(\hat x'-\hat x+\hat x-x)^T] \\
  > &= E[(\hat x'-\hat x)(\hat x'-\hat x)^T] + E[(\hat x-x)(\hat x-x)^T] \\&\ \ \ \ \  - 2E[(\hat x-x)(\hat x'-\hat x)^T] \\
  > &= E[(\hat x'-\hat x)(\hat x'-\hat x)^T] + E[(\hat x-x)(\hat x-x)^T]
  > \end{align}
  > $$
  > 第三个等号是由于 $\hat x'-\hat x = A'y+d'$
  >
  > 同样的根据上面 $MSE=tr\{M\}$ 可得到 $\hat x$ 有最小的 MSE

- 联合高斯分布的情况

  - 定理：如果 x 和 y 是联合高斯分布的，那么 
    $$
    \hat x_{BLS}(y) = \hat x_{LLS}(y)
    $$

  > 证明：$e_{LLS}=\hat x_{LLS}-x$ 也是高斯分布
  >
  > 由于 $E[e_{LLS}\ y^T]=0$，故 $e_{LLS}$ 与 y 相互独立
  >
  > $E[e_{LLS}|y]=E[e_{LLS}]=0 \to E[\hat x_{LLS}|y]=\hat x_{LLS} = E[x|y]$

  - 通常如果只有联合二阶矩信息，那么 LLS 是 minmax

## 3. Non-Bayesian formulation

- Formulation
  - observation: distribution of y **parameterized** by x,   $p_\mathsf{y}(\mathbf{y;x})$
    not **conditioned** on x,   $p_\mathsf{y|x}(\mathbf{y|x})$
    此时 x 不再是一个随机变量，而是未知的一个参数
  - bias: $b(x)=E[\hat x(y)-\mathbf{x}]$
  - 误差协方差矩阵 $\Lambda_{\mathrm{e}}(\mathrm{x})=\mathbb{E}\left[(\mathrm{e}(\mathrm{x}, \mathrm{y})-\mathrm{b}(\mathrm{x}))(\mathrm{e}(\mathrm{x}, \mathrm{y})-\mathrm{b}(\mathrm{x}))^{\mathrm{T}}\right]$
- **有效(valid)**估计器不应当显式地依赖于 x
  
- **MVU**: Minimum-variance unbiased estimator

  - 在 MMSE 条件下最优估计就是 MVU 估计
    $$
    \begin{align}
    MSE &= E[e^2]=E[(\hat x-x)^2]=E[(\hat x-\mu_{\hat x}+\mu_{\hat x }-x)^2] \\
    & =E[(\hat x-\mu_{\hat x})^2]+b^2= \Lambda_{\hat x}(x) + b^2\\
    \end{align}
    $$
  
- MVU 可能不存在
  
  - 可能不存在无偏估计，即 $\mathcal{A}=\varnothing$
  - 存在无偏估计 $\mathcal{A} \ne \varnothing$，但是不存在某个估计量在所有情况（任意 x）下都是最小方差

## 4. CRB

**定理**：满足正规条件时
$$
\mathbb{E}\left[\frac{\partial}{\partial x} \ln p_{y}(\mathbf{y} ; x) \right] = 0 \ \ \ \ for \ all \ \ x
$$
有
$$
\lambda_{\hat x}(X) \ge \frac{1}{J_y(x)}
$$
其中 Fisher 信息为
$$
J_{y}(x)=\mathbb{E}\left[\left(\frac{\partial}{\partial x} \ln p_{y}(\mathbf{y} ; x)\right)^{2}\right]=-\mathbb{E}\left[\frac{\partial^{2}}{\partial x^{2}} \ln p_{y}(\mathbf{y} ; x)\right]
$$
**证明**：取 $f(y)=\frac{\partial}{\partial x} \ln p_{y}(\mathbf{y} ; x)$，有 $E[f(y)]=0$

$$
cov(e(y),f(y))=\int (\hat x(y)-x)\frac{\partial}{\partial x} p_{y}(\mathbf{y} ; x)dy=1
$$

$$
1=cov(e,f)\le Var(e)Var(f)
$$

**备注**

- 正规条件不满足时，CRB 不存在
- Fisher 信息可以看作 $p_{y}(\mathbf{y} ; x)$ 的曲率

## 4. 有效估计量

- 定义：可以达到 CRB 的无偏估计量

- 有效估计量一定是 MVU 估计量

- MVU 估计量不一定是有效估计量，也即 CRB 不一定是紧致（tight）的，有时没有估计量可以对所有的 x 达到 CRB

- 性质：（唯一的、无偏的，可以达到 CRB）
  $$
  \hat x \ \ is \ \ efficient \iff \hat x(y)=x+\frac{1}{J_y(x)}\frac{\partial}{\partial x} \ln p_{y}(\mathbf{y} ; x)
  $$

> **证明**：有效估计量 $\iff$ 可以达到 CRB $\iff$ 取等号 $Var(e)Var(f)=1$ $\iff$ 取等号 $e(y)=k(x)f(y)$ $\iff$ $e(y)=x+k(X)f(y)$
> $$
> \frac{1}{J_y(x)}=E[e^2(y)]=k(x)E[e(y)f(y)]=k(x)
> $$

## 5. ML estimation

- Definition
  $$
  \hat x_{ML}(\cdot)=\arg\max_{a} p(y|a)
  $$

> **Proposition**: if efficient estimator exists, it's ML estimator
> $$
> \hat x_{eff}(\cdot)=\hat x_{ML}(\cdot)
> $$
> **Proof**:
> $$
> \hat x_{eff}(y)=x+\frac{1}{J_y(x)}\frac{\partial}{\partial x}\ln p(y;x)
> $$
> 由于有效(valid)估计器不应当依赖于 x，因此上式中 x 取任意一个值都应当是相等的，可取 $\hat x_{ML}(y)$
> $$
> \hat x_{eff}(y)=\hat x_{ML}(y) + \frac{1}{J_y(x)}\frac{\partial \ln p(y;x)}{\partial x}\Big|_{x=\hat x_{ML}}=\hat x_{ML}(y)
> $$
> **备注**：反之不一定成立，即 ML 估计器不一定是有效的，比如有时候全局的有效估计器(efficient estimator)不存在，也即此时按公式计算得到的 $\hat x_{eff}(y)$ 实际上是依赖于 x 的，那么此时就不存在一个全局最优的估计器，此时的 ML 估计器也没有任何好的特性。