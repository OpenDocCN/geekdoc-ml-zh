# 投资回报池、投资回报对齐和投资回报扭曲的详细说明

> 原文：<https://medium.com/mlearning-ai/detailed-explanation-of-roi-pooling-vs-roi-align-vs-roi-warping-8defda37461a?source=collection_archive---------0----------------------->

## 这些特征提取方法可以帮助您保存图像中的信息！

在上一篇[文章](https://raychunyin00.medium.com/image-segmentation-extending-faster-r-cnn-into-mask-r-cnn-953fe4923d64)中，我们介绍了使用 Mask R-CNN 构建物体检测器的步骤，该检测器能够逐个像素地在检测到的物体上输出遮罩。掩模 R-CNN 中的一个步骤不同于更快的 R-CNN 中的一个步骤是在图像通过卷积网之后的特征提取方法。这一步在快速 R-CNN 中称为 RoI 合并，在掩模 R-CNN 中称为 RoI 对齐。