---
title: 舒尔补
date: 2021-05-28 08:23:04
tags:
  - 舒尔补
categories: 
  - Linear Algebra
mathjax: true
---

舒尔补其实就是将一个矩阵变成块对角化的矩阵，便于求逆。

## 1. 定义

n\*n 的矩阵可以写成分块矩阵形式
$$
M = \left[\begin{array}{cc}{A} & {B} \\ 
{C} & {D}\end{array}\right]_{n\times n}
$$

- 若 A 是非奇异的，则 A 在 M 中的舒尔补为：$D-CA^{-1}B$
- 若 D 是非奇异的，则 D 在 M 中的舒尔补为：$A-BD^{-1}C$
- 只要记住字母顺序 DCAB、ABDC 都是在 M 中**顺时针**排列的

## 2. 推导

若 A 非奇异，有
$$
\left[\begin{array}{cc}{I} & {0} \\ 
{-C A^{-1}} & {I}\end{array}\right]
\left[\begin{array}{cc}{A} & {B} \\ 
{C} & {D}\end{array}\right]
\left[\begin{array}{cc}{I} & {-A^{-1} B} \\ 
{0} & {I}\end{array}\right]=
\left[\begin{array}{cc}{A} & {0} \\ 
{0} & {D-C A^{-1} B}\end{array}\right]
$$
可得到
$$
\left|\begin{array}{cc}{A} & {B} \\ 
{C} & {D}\end{array}\right|=
\left|\begin{array}{cc}{A} & {0} \\ 
{0} & {D-C A^{-1} B}\end{array}\right|=
|A||D-CA^{-1}B|
$$
此时
$$
M 非奇异 \Leftrightarrow D-CA^{-1}B 非奇异
$$

## 3. 性质

记矩阵 $A<0$ 表示 A 负定，则对于矩阵 M 有以下性质
$$
M < 0 \\
\iff A<0, D-C A^{-1} B<0 \\
\iff D<0, A-BD^{-1}C<0
$$

逆矩阵
$$
\left[\begin{array}{cc}{A} & {B} \\ 
{C} & {D}\end{array}\right]^{-1} = 
\left[\begin{array}{cc}{(A-BD^{-1}C)^{-1}} & {X} \\ 
{Y} & {(D-CA^{-1}B)^{-1}}\end{array}\right]
$$