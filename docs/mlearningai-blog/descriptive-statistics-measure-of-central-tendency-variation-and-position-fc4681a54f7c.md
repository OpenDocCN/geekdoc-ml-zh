# 描述性统计:集中趋势、变化和位置的测量

> 原文：<https://medium.com/mlearning-ai/descriptive-statistics-measure-of-central-tendency-variation-and-position-fc4681a54f7c?source=collection_archive---------2----------------------->

## 描述性统计中的测量以及如何使用 Python 进行计算

![](img/77de70cbcfcaa9e9661cc9d4794a526e.png)

Photo by Nataliya Vaitkevich from [Pexels](https://www.pexels.com/photo/gray-laptop-beside-white-printer-paper-8062289/)

# 介绍

数据是统计学的核心。我们收集的观察数据在使用前需要分析。当我们的数据非常大的时候，对数据进行汇总是很重要的。总结数据确实有助于我们从数据中分析和提取见解。

描述性统计非常关键，我们可以很容易地将数据总结成数字和图形。更确切地说，描述性统计是一种收集数据、处理数据(总结和呈现)、描述和分析所有数据的方法。描述性统计最关键的是以信息的形式交流数据，并支持对数据的推理。

在描述性统计中有 3 个关键的衡量标准。它是集中趋势的度量、变化的度量和位置的度量。在本文中，我们将深入研究它，并使用 Python 进行计算。

> 你可以在这里访问我们使用[的完整代码](https://github.com/dedee95/descriptive_statistics/blob/main/desciptive_statistics.ipynb)

# 集中趋势的度量

集中趋势的度量是一种值度量，可用于表示数据集的中心值。在统计学中，有三种方法来衡量集中趋势:均值(平均值)、中位数(中间值)和众数(经常出现的值)。

![](img/c08ea84fc63e0f7ddc76dbbc326e5bf5.png)

Source: [https://www.quora.com/What-does-SKEWED-DISTRIBUTION-mean](https://www.quora.com/What-does-SKEWED-DISTRIBUTION-mean)

## 平均

平均值是数据集中值的总和除以数据集中值的数量。一般来说，当我们谈到平均数时，我们指的是算术平均数。总体和样本的平均值以同样的方式计算。

![](img/f62a517298e5fa4074a7d1903da0c7ba.png)

The formula for calculating the mean (Image by author)

![](img/03f2e25f797af18062230f0c825bb8e0.png)

Calculating arithmetic mean using Python (Image by author).

此外，加权平均值是算术平均值的子集。在加权平均值中，我们假设每个值都有一定的权重，因此要计算加权平均值，我们必须先将该值乘以其各自的权重。

![](img/014e4bc39d67de2ca7b5d96678de72ea.png)

The formula for calculating the weighted mean (Image by author)

![](img/c8b4b0dc24c8895fe7e885fcf78f9a60.png)

Calculating weighted mean using Python (Image by author).

然后，还有几何平均。几何平均值的计算方法是将数据集中的所有值相乘，然后用数据集中的和值的幂求根。

![](img/709aa0017b90e0d0c959e9dbba3a508f.png)

The formula for calculating the geometric mean (Image by author)

![](img/01554b5e1fe307d8accaf45d4e5478c0.png)

Calculating geometric mean using Python (Image by author).

此外，还有一个调和的意思。调和平均值的计算方法是将数据集中值的数量除以数据集中每个值的倒数。

![](img/2b2169416f73335a5415c19ef3b55384.png)

The formula for calculating the harmonic mean (Image by author)

![](img/c4bd440dddcc41606aac8887ba33b1c7.png)

Calculating harmonic mean using Python (Image by author)

## 中位数

中位数是数据集的中间值。要找到中值，我们必须首先对数据集中的所有值进行排序(从最小值到最大值)，然后寻找数据集的中点或中间值。如果数据集的数值是偶数，则取两个中间值的平均值。

![](img/59735b8db925deb18224429e61947856.png)

Middle value (Image by author)

![](img/04a6d59ece5500307bcd7adfd849aae0.png)

Calculating median using Python (Image by author)

## 方式

模式是数据集中经常出现的值。当一个值在数据集中出现的频率相同时，说明没有模式。同时，如果有两个值出现频率最高，则称为双峰。

![](img/6dd39c9983411aaed44f8a4722f24472.png)

The value that appears frequently (Image by author)

![](img/94e913ba2582fc3c59feec27cf2bed74.png)

Calculating mode using Python (Image by author)

# 变化的度量

变异或离差的度量是可用于表示数据的多样性或分布的值的度量。通过这个度量，我们可以确定数据如何从最小的数据扩散到最大的数据，或者数据如何远离整体数据分布的中心。当变化量为零时，则表明数据中的总体值是一致的。

## 范围

该范围是数据集中最大值和最小值之间的差值。但是，该范围有一个缺点，因为它在测量过程中只包括两个值。

![](img/e02efec92f44dbee5627d775ed14bed0.png)

The formula for calculating the range (Image by author)

![](img/5b886625e463a4b115f54bc1ef69df30.png)

Calculating range using Python (Image by author)

## 四分位数间距(IQR)

四分位数间距或四分位数之间的范围是通过减去第三个四分位数(Q3)和第一个四分位数(Q1)的值计算的。IQR 不会受到极端值(异常值)的影响。

![](img/ecefce5111a28d35a35d34a46dc764ab.png)

The formula for calculating interquartile range (Image by author)

![](img/88d7d35f513debd272f78d0eb31c06e6.png)

Calculating interquartile range using Python (Image by author)

## 差异

方差是衡量一组值围绕平均值分布的程度。方差的主要缺点是该值不再具有与数据集中的值相同的比例。但是，标准差可以解决这个缺点。

![](img/7d10de771af754dc0746f008ced69427.png)

The formula for calculating variance (Image by author)

![](img/b28e4426768975316ab2f0c6510d9b31.png)

Calculating variance using Python (Image by author)

## 标准偏差

标准偏差是用于确定数据分布并查看数据与平均值的接近程度的值。标准差的作用是看每个数字与平均值的差，求差的平方，然后看差的平方的平均值。最后，求平方根。

![](img/9a6787607907c66b1b421934d9fc07cc.png)

The formula for calculating the standard deviation (Image by author)

![](img/c9a9adc9c6299c67caf55f9a9a2fba11.png)

Calculating standard deviation using Python (Image by author)

当标准偏差较高时，数据更加偏离平均值。相比之下，如果标准偏差较小，数据分布将接近平均值。

![](img/e67ba199ea7f5a110f8176c07cab4598.png)

Standard deviation comparison in a normal distribution (Image by author)

此图说明了正态分布曲线的点如何随着标准偏差的增加而减小。相反，正态分布曲线的峰值点随着标准差的减小而增加。

# 位置的度量

位置度量是用于确定数据点相对于数据集的位置的度量。这种测量可以指示一个值是高、低还是平均。

## 四分位数

四分位数是将有序数据集分成四等份的值。有三个值被称为四分位数:Q1、Q1 和 Q3。第二个四分位数的值等于平均值。

![](img/6d91474410e06b1ba027d2cd3c1b57b6.png)

Quartile illustration (Image by author)

![](img/e2028c2d5f6ba4dbc380abe8b43ac36d.png)

Calculating quartiles using Python (Image by author).

## 十分位数

十分位数是将有序数据集分成 10 等份的值。这些值被命名为第一个十分位数(D1)、第二个十分位数(D2)，依此类推，直到第九个十分位数(D9)。

![](img/e31432920349d992e6d71abefee6af2f.png)

Decile illustration (Image by author).

![](img/9d6563b836796e79c6b6235f1805d994.png)

Calculating deciles using Python (Image by author).

## 百分位

百分点值是将有序数据集分成 100 个相等部分的值。有 99 个百分位值，从 P1、P2……和 P99 开始。百分点可用于检测异常值。当一个值小于第 5 百分位(P5)或大于第 95 百分位(P95)时，它可以被归类为异常值。

![](img/e1b452aed96ff3fc2a47671911ef9f21.png)

Calculating percentiles using Python (Image by author).

# 结论

描述性统计对汇总数据非常有用。当我们的数据非常大的时候，对其进行总结会让我们理解数据变得容易得多。中心趋势的计算对于确定数据集的中心或中点非常有用。然后，变化计算可用于确定数据集的分布和变化。而位置计算对于确定数据集的相对位置是有用的。

# 参考资料:

[1]t .尼尔德(2022 年)。*数据科学的基本数学:用基本的线性代数、概率和统计学控制你的数据*(第 1 版。).奥莱利媒体。

[2]统计—数理统计函数— Python 3.10.5 文档。Docs.python.org。(2022).检索于 2022 年 7 月 20 日，来自 https://docs.python.org/3/library/statistics.html.

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)