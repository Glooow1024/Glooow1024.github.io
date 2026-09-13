---
title: 高斯分布常用公式
date: 2023-12-18 21:46:07
tags:
  - 高斯分布
categories:
  - Probability Theory
mathjax: true
---

> 后面推导中反复用到Woodbury恒等式
> $$
> (A+UCV)^{-1} = A^{-1} - A^{-1}U(C^{-1}+VA^{-1}U)^{-1}VA^{-1}
> $$

### 1. 边缘分布

假设 $\boldsymbol{x}\sim {\mathcal N}(\boldsymbol{\mu}, \boldsymbol\Sigma_1), \boldsymbol{y}|\boldsymbol{x}\sim\mathcal{N}(\boldsymbol{A}\boldsymbol{x}, \boldsymbol\Sigma_2)$，那么 $\boldsymbol{y}$ 的边缘分布也是高斯分布
$$
\boldsymbol{y} \sim \mathcal{N}(\boldsymbol{\mu}_3, \boldsymbol\Sigma_3) \\
\boldsymbol\Sigma_3 = (\boldsymbol\Sigma_2^{-1} - \boldsymbol\Sigma_2^{-1}\boldsymbol{A} (\boldsymbol\Sigma_1^{-1}+\boldsymbol{A}^{\rm T} \boldsymbol\Sigma_2^{-1} \boldsymbol{A})^{-1} \boldsymbol{A}^{\rm T} \boldsymbol\Sigma_2^{-1})^{-1} = \boldsymbol\Sigma_2 + \boldsymbol{A}\boldsymbol\Sigma_1 \boldsymbol{A}^{\rm T} \\
\boldsymbol{\mu}_3 = \boldsymbol\Sigma_3 \boldsymbol\Sigma_2^{-\rm T} \boldsymbol{A} (\boldsymbol\Sigma_1^{-1}+\boldsymbol{A}^{\rm T} \boldsymbol\Sigma_2^{-1} \boldsymbol{A})^{-1} \boldsymbol\Sigma_1^{-1} \boldsymbol{\mu} = \boldsymbol{A\mu}\\
$$

### 2. 后验分布

假设有先验 $\boldsymbol{x}\sim {\mathcal N}(\boldsymbol{\mu}, \boldsymbol\Sigma_1)$，似然 $\boldsymbol{y}|\boldsymbol{x}\sim\mathcal{N}(\boldsymbol{A}\boldsymbol{x}, \boldsymbol\Sigma_2)$，那么关于 $\boldsymbol{x}$ 的后验分布也是高斯分布
$$
p(\boldsymbol{x}| \hat{\boldsymbol{y}}) \propto p(\boldsymbol{x},\hat{\boldsymbol{y}}) = p(\boldsymbol{x}) p(\hat{\boldsymbol{y}} | \boldsymbol{x}) \sim {\mathcal N}(\boldsymbol{\mu}_4, \boldsymbol\Sigma_4) \\
\boldsymbol{\Sigma}_4 = (\boldsymbol\Sigma_1^{-1} + \boldsymbol{A}^{\rm T}\boldsymbol{\Sigma}_2^{-1}\boldsymbol{A})^{-1} \\
\boldsymbol{\mu}_4 = \boldsymbol{\Sigma}_4 (\boldsymbol{\Sigma}_1^{-1}\boldsymbol{\mu} + \boldsymbol{A}^{\rm T}\boldsymbol{\Sigma}_2^{-1}\hat{\boldsymbol{y}})
$$
特殊情况下，假如 $\boldsymbol{A}=\boldsymbol{I}, \boldsymbol{\Sigma}_2=\sigma^2\boldsymbol{I},\boldsymbol{\mu}=\boldsymbol{0}$，此时将有
$$
\boldsymbol{\Sigma}_4 = \sigma^2 \boldsymbol{\Sigma}_1 (\boldsymbol{\Sigma}_1 + \sigma^2\boldsymbol{I})^{-1} \\
\boldsymbol\mu_4 = \boldsymbol{\Sigma}_1(\boldsymbol{\Sigma}_1+\sigma^2\boldsymbol{I})^{-1}\hat{\boldsymbol{y}}
$$



### 3. 条件分布

假设 $\boldsymbol{x}_1,\boldsymbol{x}_2$ 服从联合高斯分布
$$
\begin{bmatrix} \boldsymbol{x}_1 \\ \boldsymbol{x}_2 \end{bmatrix} \sim {\mathcal N} \left( \begin{bmatrix} \boldsymbol{\mu}_1 \\ \boldsymbol{\mu}_2 \end{bmatrix},  \begin{bmatrix} \boldsymbol{\Sigma}_{11} & \boldsymbol{\Sigma}_{12} \\ \boldsymbol{\Sigma}_{21} & \boldsymbol{\Sigma}_{22} \end{bmatrix}  \right)
$$
那么 $\boldsymbol{x}_2 | \boldsymbol{x}_1$ 也服从高斯分布
$$
\boldsymbol{x}_2 | \boldsymbol{x}_1 \sim {\mathcal N}(\boldsymbol{\mu}_5, \boldsymbol{\Sigma}_5) \\
\boldsymbol{\Sigma}_5 = \boldsymbol{\Sigma}_{22} -  \boldsymbol{\Sigma}_{21} \boldsymbol{\Sigma}_{11}^{-1} \boldsymbol{\Sigma}_{12} \\
\boldsymbol{\mu}_5 = \boldsymbol{\mu}_2 + \boldsymbol{\Sigma}_{21} \boldsymbol{\Sigma}_{11}^{-1} (\hat{\boldsymbol{x}}_1 - \boldsymbol{\mu}_1)
$$
