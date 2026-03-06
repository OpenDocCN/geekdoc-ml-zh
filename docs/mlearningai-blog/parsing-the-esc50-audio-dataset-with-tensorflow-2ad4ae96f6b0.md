# 用 TensorFlow 解析 ESC50 音频数据集

> 原文：<https://medium.com/mlearning-ai/parsing-the-esc50-audio-dataset-with-tensorflow-2ad4ae96f6b0?source=collection_archive---------1----------------------->

## 从原始音频到 TFRecord 文件，再到随时可用的数据集

TensorFlow 的 TFRecord 格式得心应手。但是，刚开始几次，不方便处理。虽然许多数据集很容易来自 TensorFlow 数据集，但有些数据集必须手动解析。环境声音分类的简称 ESC-50 数据集就是其中之一。

![](img/e5d1882c9411e10d9b719a2b3e3761e5.png)

Photo by [Dan-Cristian Pădureț](https://unsplash.com/@dancristianp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)