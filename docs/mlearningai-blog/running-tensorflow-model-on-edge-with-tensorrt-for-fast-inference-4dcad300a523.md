# 使用 TensorRT 在 Edge 上运行 Tensorflow 模型进行快速推理

> 原文：<https://medium.com/mlearning-ai/running-tensorflow-model-on-edge-with-tensorrt-for-fast-inference-4dcad300a523?source=collection_archive---------1----------------------->

## 关于 Linux 环境下 TensorRT 的设置、量化和运行推理

![](img/63aa6ef8fdd3884646302a633c8f56ae.png)

Image by [CHUTTERSNAP](https://unsplash.com/@chuttersnap) on [Unsplash](https://unsplash.com/)

在实时人工智能模型部署过程中，模型预测(或推理)的速度至关重要。为了确保无缝的用户体验，苹果 Siri 的语音引擎不能永远破译一个…