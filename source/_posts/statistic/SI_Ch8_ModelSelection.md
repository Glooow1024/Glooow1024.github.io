---
title: 统计推断(八) Model Selection
date: 2020-02-03 20:02:00
tags:
  - 模型选择
categories: 
  - 统计推断
mathjax: true
---

模型选择

<!--more-->

## 1.Bayesian Approach

- Consider a nested sequence of model classes 

  $$
  \mathcal{P}_{1} \subset \mathcal{P}_{2} \subset \mathcal{P}_{3} \subset \cdots
  $$

- ML decision rule: 
  $$
  \hat{m}=\arg \max _{m}\left\{\max _{p \in \mathcal{P}_{m}} p(\boldsymbol{y})\right\}=\arg \max _{m}\left\{\max _{a} p_{y | x, H}\left(\boldsymbol{y} | a, H_{m}\right)\right\}
  $$

## 2. Laplace’s Method

- 连续分布

  $$
  p_{\times}(x)=\frac{p_{0}(x)}{Z_{p}}
  $$

- 用 taylor 级数近似似然函数
  $$
  \ln p_{0}(x) \approx \ln p(\hat{x})+\left.(x-\hat{x}) \frac{\mathrm{d}}{\mathrm{d} x} \ln p_{0}(x)\right|_{x=\hat{x}}+\left.\frac{1}{2}(x-\hat{x})^{2} \frac{\mathrm{d}^{2}}{\mathrm{d} x^{2}} \ln p_{0}(x)\right|_{x=\hat{x}} \\
  p_{0}(x) \approx p_{0}(\hat{x}) \exp \left[-\frac{1}{2} J_{\mathbf{y}=\boldsymbol{y}}(\hat{x})(x-\hat{x})^{2}\right]
  $$

## 3. Bayes Information Criterion 

- MAP decision rule: 
  $$
  \hat{m}=\arg \max _{m} p_{\mathbf{y} | \mathbf{H}}\left(\boldsymbol{y} | H_{m}\right)
  $$
  其中
  $$
  p_{\mathbf{y} | \mathbf{H}}\left(\boldsymbol{y} | H_{m}\right)=\int p_{\mathbf{y} | \mathbf{x}, \mathbf{H}}\left(\boldsymbol{y} | x, H_{m}\right) p_{\mathbf{x} | \mathbf{H}}\left(x | H_{m}\right) \mathrm{d} x
  $$
  令
  $$
  q_{0}(x)=p_{\mathbf{y} | \mathbf{x}, \mathbf{H}}\left(\boldsymbol{y} | x, H_{m}\right) p_{\mathbf{x} | \mathbf{H}}\left(x | H_{m}\right) \propto p_{\mathbf{x} | \mathbf{y}, \mathbf{H}}\left(x | \boldsymbol{y}, H_{m}\right)
  $$
  可以有
  $$
  p_{\mathrm{y} | \mathrm{H}}(\boldsymbol{y} | H)=\int q_{0}(x) \mathrm{d} x \approx p_{\mathrm{y} | x, \mathrm{H}}(\boldsymbol{y} | \hat{x}, H) p_{\mathrm{x} | \mathrm{H}}(\hat{x} | H) \sqrt{2 \pi J_{\mathrm{y}}^{-1}(\hat{x})}
  $$
  其中最后一项为 **Occam’s razor factor**

