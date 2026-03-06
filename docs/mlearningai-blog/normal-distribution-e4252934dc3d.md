# 正态分布

> 原文：<https://medium.com/mlearning-ai/normal-distribution-e4252934dc3d?source=collection_archive---------5----------------------->

## 使用 R 和 Python

正态分布、高斯分布或钟形曲线是一种关于平均值对称的概率分布。它表明，与远离平均值的数据相比，接近平均值的数据更频繁。曲线下的面积是一。这在数据科学中非常重要，因为假设检验需要数据采用标准格式。线性和非线性回归都假设残差服从正态分布。在本文中，我们将学习以下主题

*   生成并可视化随机法向量
*   计算不同情况下的正态和标准正态曲线的 PDF 和 CDF。
*   计算 CDF 的逆的不同情况

![](img/6023b13ef9cce9045ce1c7415c96ccdd.png)

Photo by [Naser Tamimi](https://unsplash.com/@tamiminaser?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 创建一个随机数向量

*   r 使用`rnorm(n, mean = 0, sd = 1)`生成`n`个平均值`0`和标准差`1`的随机数
*   Python 使用`np.random.normal(loc = 0, scale = 1, size = n)`生成平均值`0`和标准差`1`的`n`个正常数的向量。

## 稀有

可视化以 R 为底的大小为`100000`点的向量

![](img/43606f994d5cd460ffbdaedbe744a3d0.png)

## 计算机编程语言

用 seaborn 可视化一个大小为`100000`点的向量

![](img/23eb15790cf14ae2fe2e6855a22fd31d.png)

# 正态分布函数

*   r 有`dnorm(x, mean = 40, sd = 5)`功能，计算`mean = 40`的正常曲线值和`x`点的立架偏差`sd = 5`
*   Python 有`norm.pdf(42, loc=40, scale = 5)`函数，计算`mean = 40`的法向曲线值和`x`点的立位偏差`sd = 5`
*   我们还将在 R 和 Python 编程语言中实现以下公式来解决问题

![](img/0b139071389c0e35922cb5be42a6d8b6.png)

## 问题

如果 X 是一个带有`μ = 40`和`σ = 5.`的正态随机变量，在`x = 42`处找到其正态曲线的纵坐标。

![](img/c38ff44887df51e717e1d1115d85e99c.png)

## 稀有

`x = 42, μ = 40, σ = 5`

**由用户定义的功能**

**通过基数-R 函数**

## 计算机编程语言

`x = 42, μ = 40, σ = 5`

**由用户定义的功能**

**由** `**scipy**` **功能**组成

# 标准正态分布

标准正态分布是正态分布函数的特例，其中`μ = 0`和`σ = 1`

![](img/a7e5e964fe0a287b4cc16245cf721f51.png)

## 问题

如果`z`是一个带有`μ = 0`和`σ = 1.`的标准正态随机变量，求其在`z = 0.4`处的正态曲线的纵坐标。

## 解决办法

![](img/437cea251315853d11bde2bc5d4152a8.png)

## 稀有

`z = 0.4, μ = 0, σ = 1`

## 计算机编程语言

`z = 0.4, μ = 0, σ = 1`

# 累积分布函数

## 案例 1

计算阴影曲线下的面积。

![](img/e7fab27606a68f9d84efc5c87945e947.png)![](img/a2684380223df2cfec9629960ad217fb.png)![](img/d9a168fe02f00eabd5c919a47d26a228.png)

## 稀有

## 计算机编程语言

## 案例 2

计算阴影曲线下的面积。

![](img/0cec0f828406ce50c863b19512c18e3d.png)![](img/927b6943172e9cbd775d65dc9ec8a141.png)![](img/983a5f94b3957e9c1e6bb88ab99eca30.png)

## 稀有

## 计算机编程语言

## 案例 3

计算阴影曲线下的面积。

![](img/abccc890a3463cfaf93e4ee91401ccd9.png)![](img/6f9847d5069259c58c061ffda667a5eb.png)![](img/de192d8f29b0c5f0de21c2e7f36819bc.png)

## 稀有

## 计算机编程语言

## 案例 4

计算阴影曲线下的面积。

![](img/9874b2befd5f7301ab795684d9878274.png)![](img/70c7c4a1e583982184d2452f884a9dfa.png)![](img/7178ad18e466963d62cb9765eeb43f2a.png)

## 稀有

## 计算机编程语言

# 计算要点

## 案例 1

给定概率`p`，我们需要计算点`x`

![](img/1fdd95cb97737dd3e41c99e2ecbcc055.png)![](img/2bf5b1aa31478616a3bdfb1cc0a3d7e6.png)

## 稀有

## 计算机编程语言

## 案例 2

给定概率`p`，我们需要计算点`x`

![](img/7ea00673b4b08d417b0975210c9e80f0.png)![](img/578fa3bfb5082d49f3cc54a41977aa72.png)

## 稀有

## 计算机编程语言

# 结论

在本文中，我们将学习如何生成一个随机标准数的向量，以及如何在 base-R 和 Python 的 seaborn 中可视化它们。我们还将学习如何计算正态曲线的 PDF 和 CDF，以及标准正态曲线和 CDF 的倒数。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)