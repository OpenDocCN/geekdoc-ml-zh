# 在数组上使用 NumPy 函数的指南

> 原文：<https://medium.com/mlearning-ai/a-guide-to-using-numpy-functions-on-arrays-19f268ac42cf?source=collection_archive---------4----------------------->

## 切片和索引与 NumPy 数组，矩阵运算和更多..

![](img/fb5cd0d564ec3317474074c457514f57.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/matrix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当数组需要通过数学运算来操作时，您只需要对数组应用与数值常数(标量)或形状完全相同的数组相关的运算:

```
import numpy as np
a = np.arange(5).reshape(1,5)
a += 1…
```