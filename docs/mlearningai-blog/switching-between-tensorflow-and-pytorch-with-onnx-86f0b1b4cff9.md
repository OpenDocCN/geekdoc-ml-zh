# 使用 ONNX 在 TensorFlow 和 PyTorch 之间切换

> 原文：<https://medium.com/mlearning-ai/switching-between-tensorflow-and-pytorch-with-onnx-86f0b1b4cff9?source=collection_archive---------1----------------------->

## 无限制地使用你喜欢的人工智能框架

![](img/89c0ef62b82c2e4cb1479e7cc3da730a.png)

Photo by [Clarisse Croset](https://unsplash.com/@herfrenchness?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

随着机器学习模型变得越来越大，越来越复杂，建立模型的时间和材料成本往往要求你的项目在一个框架下保持一致。 [Tensorflow](https://www.tensorflow.org/) 和 [PyTorch](https://pytorch.org/) 是人工智能和机器学习领域最流行的两个框架，尤其是对于深度学习社区。

Tensorflow 和 PyTorch 之间的选择通常可以归结为您对每个框架或公司和行业标准中的开发和生产流程的熟悉程度。然而，从易用性和部署基础设施到可用的生态系统支持，每个框架都有一定的优势。

Tensorflow 较老，对其他语言(Tensorflow.js、Swift)有更多支持，内置 API 和生产工具，如 TFServing、TFX 和 TFLite，可简化跨本地、云、物联网和移动平台的模型部署。Tensorflow 还拥有更广泛的生态系统，其中包含特定于用例的深度学习应用程序(文本、图像、音频和视频)，可以跨平台集成。

PyTorch 显然更容易学习和使用，至少对于 Python 程序员来说是这样。它有一个更快的模型开发过程，其 CUDA 后端和有效的内存使用。这使得它成为大多数研究人员或 OpenAI 等开源组织构建模型的首选。这反过来意味着更多预先训练的最先进的模型是 PyTorch 独有的。

普遍的看法是，如果你在研究领域构建模型，PyTorch 是适合你的框架，而如果你在行业内工作或想以任何方式部署你的模型，Tensorflow 是理想的选择。但是，可以使用 PyTorch 进行开发，使用 [ONNX](https://onnx.ai/) 使用 Tensorflow 进行部署。

本文展示了使用 ONNX 在框架之间转换模型的能力。

# ONNX 简介

开放神经网络交换(ONNX)是一个开源生态系统，它使 AI 开发人员能够为他们的项目使用最有效的工具，而不用担心生产兼容性和下游部署。它为您的人工智能模型提供了一种通用格式，无论是深度学习还是其他方式，实现了不同框架和硬件优化之间的互操作性。

ONNX 抽象了框架之间的相似性，创建了一个带有内置操作符和数据类型的标准模型定义。该平台目前支持几个人工智能框架来构建、部署、优化和可视化你的模型。

## 使用 ONNX

作为一名从事计算机视觉项目的研究人员，该项目旨在对处于不同健康阶段的植物进行分类。您可能希望利用 PyTorch 的易用性和速度来迭代深度学习模型的开发过程。但是，您当前的后端模型部署流程仅限于利用 Tensorflow 服务的广泛性的 Tensorflow 特定格式，

您可以使用 PyTorch 训练您的模型，并使用“torch.save”功能保存模型

```
torch.save(your_model.state_dict(), ‘your_model.pth’)
```

然后利用 PyTorch 的内置 [ONNX exporter](https://pytorch.org/docs/stable/onnx.html) 以包含模型架构和参数的 ONNX 格式表示您的模型。

```
trained_model = model()
trained_model.load_state_dict(torch.load(your_model.pth’))dummy_input = Variable(torch.randn(1,3,128,128))torch.onnx.export(trained_model, dummy_input, “your_model.onnx”)
```

**注意** : ONNX 不仅仅是一种方便的模型格式，你可以通过 [ONNXRuntime](https://onnxruntime.ai/) 来利用这种格式的模型进行更快的重新训练和推理过程。

下一步是将模型从 ONNX 格式转换为 Tensorflow 模型格式。您可以使用 [ONNX Tensorflow 后端](https://github.com/onnx/onnx-tensorflow)包，该包支持 ONXX 到 Tensorflow 的兼容性。

```
import onnx
from onnx_tf.backend import prepareonnx_model = onnx.load('your_model.onnx')
tf_rep = prepare(onnx_model)
```

这将输出一个张量流模型表示，然后可用于推理或部署。

**注意**:这里你已经看到了从 PyTorch 到 ONNX 到 Tensorflow 的转换，使用 [Tensorflow 到 ONNX](https://github.com/onnx/tensorflow-onnx/) 和 [ONNX 到 PyTorch](https://github.com/ToriML/onnx2pytorch) 工具也可以完成相反的转换。

# 结论

本文简要介绍了 ONNX 及其实现 AI 框架和工具之间互操作性的方法。在本文中，我展示了在 PyTorch 和 ONNX 之间交换 AI 模型的简单过程，使用了不到十行代码。

你可以在这个 [GitHub 资源库](https://github.com/Soot3/Farmer_TK_Notebooks/blob/main/notebooks/potato-model.ipynb)中找到从 PyTorch 训练到 Tensorflow 推理的完整代码样本。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)