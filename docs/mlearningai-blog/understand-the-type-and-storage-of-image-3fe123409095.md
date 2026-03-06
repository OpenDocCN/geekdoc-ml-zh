# 理解数字图像的类型和 Python 中的代码

> 原文：<https://medium.com/mlearning-ai/understand-the-type-and-storage-of-image-3fe123409095?source=collection_archive---------9----------------------->

单通道、三通道、灰度图像等。

这个故事回到了数字图像的基础，以了解不同的图像，如单通道，三通道，灰色和 RGB，是如何存储的，以及如何正确读取和识别它们。

让我们开始吧。

1.  **单通道灰度图像**
    使用 below NumPy 生成 5x10 像素大小的单通道图像。

```
img1=np.random.randint(0,255,size=(5,10))
```