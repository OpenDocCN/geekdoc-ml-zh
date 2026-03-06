# 里纳尔格-克莱姆法则

> 原文：<https://medium.com/mlearning-ai/linalg-cramers-rule-637fb4d1dc77?source=collection_archive---------5----------------------->

克莱姆法则，用几何学解释——3 蓝 1 棕

## 克莱姆法则是什么？

克莱姆法则是一种使用**行列式**到**求解方程组**的方法，这些方程组的变量与**的方程数量相同。**

假设我们有这样一个方程组:

![](img/047a18560b952d6194fba95e99b54af0.png)

我们可以用矩阵和向量来表示。

![](img/3700ed69cf5bd7c65d7b38333a2f9604.png)

我们可以从定义一个新的矩阵 Aᵢ(b 开始，这可以通过用向量 b 代替矩阵 a 的 iᵗʰ列来实现

![](img/41b76f8b135b26dcddb87e9169104791.png)

克莱姆法则:当矩阵 a 可逆时，解 x 可以表示为 xᵢ:

![](img/86bf075702c724270bc396ebb935d636.png)

举个例子，

![](img/bb117fab8b5d7b1cbf69fab6e17fba84.png)

所以使用我们的例子，我们首先需要检查行列式是否不为 0。

![](img/078fe9eb12ca3de75d0dae9a3f486df6.png)

calculating the determinant

现在我们知道我们的矩阵 A 是可逆矩阵。

新定义的矩阵 Aᵢ(b)会是这样的:

![](img/3b7ffba45b51050a0145b435796fbce0.png)

每个矩阵的行列式为:

![](img/f46f944c6e2298526e299bc02ee0fd30.png)

解决方案是:

![](img/0b3c232704b13fe28756150eefa2c0c5.png)

## 为什么这个规则是这样的？

利用行列式，我们可以更好地理解克莱姆法则。

假设我们有，

![](img/24ef0b9a5acb04e7b651e46bbe089891.png)

我们想找到匹配的向量 x。

转换(应用矩阵 A)前的初始状态如下所示:

![](img/a7b30cf90dda5e8d83a29f47449c91d3.png)

这里，我们要用一种特殊的方式来表示向量 x 的 x 和 y 坐标，用平行四边形。

<y-coordinate></y-coordinate>

![](img/b959907e303b658872e2f3866ea08bb8.png)

我们可以说向量 x 的 y 坐标正好是 y，但是平行四边形的这个面积也可以表示向量 x(变换前)的 y 坐标，因为它乘以了单位向量 i-hat(长度= 1)。

<x-coordinate></x-coordinate>

![](img/52a4671ebbfc0ad1f7d8a7791091c8d0.png)

我们可以说，向量 x 的 x 坐标正好是 x，但平行四边形的这个面积也可以表示向量 x(变换前)的 x 坐标，因为它乘以了单位向量 j-hat(长度= 1)。

**如果我们在这里应用线性变换会怎么样？**

![](img/3508174052b322262e0d5cbf2b7f154e.png)

<y-coordinate></y-coordinate>

![](img/7d46fd5d4b3d0bf90f21489214117653.png)

行列式告诉我们系统改变了多少。平行四边形的面积也会改变，改变的量是行列式的量。

所以新的平行四边形是:det(A) * y

所以 y 坐标可以写成:

![](img/9189a3963061a5b0add8e14bf1c6cdd9.png)

为了计算带符号的 Area₂，我们可以计算缩放的 i-hat 和缩放的向量 x 的 j-hat 着陆位置之间的行列式。所以我们要找的矢量 x 的最终 y 坐标应该是这样的:

![](img/8f69d36ae118ca5b67cb76be17e31682.png)

<x-coordinate></x-coordinate>

![](img/8662fd9cd7540bb2ffc3a9dee5792d9b.png)

行列式告诉我们系统改变了多少。平行四边形的面积也会改变，改变的量是行列式的量。

所以新的平行四边形是:det(A) * x

所以 y 坐标可以写成:

![](img/bc2eec7c5263c68e14e046c28fdd024d.png)

为了计算带符号的 Area₂，我们可以计算缩放向量的 i-hat 着陆位置和缩放的 j-hat 之间的行列式。所以我们要找的向量 x 的最终 x 坐标应该是这样的:

![](img/3f9a593ce2f2a1cc7b25bdfb7aed0169.png)

**结论**

![](img/0a439bfdf2c44f8a9f83ea8eb98f7768.png)

这与以下内容相同:

![](img/7d5c1f02dc7cefceb23a695d180a28df.png)

现在我们知道克莱姆法则在几何上是如何工作的了。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)