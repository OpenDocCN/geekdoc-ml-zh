# Pytorch & C++ #4:使用 Torchvision 模型

> 原文：<https://medium.com/mlearning-ai/pytorch-c-4-using-torchvision-models-f12bd25f4744?source=collection_archive---------2----------------------->

![](img/2df193ef8fcb85dc3308b62b246c4add.png)

# 介绍

在本系列中，我将尝试提供使用 Pytorch c++ API 的示例/实践/项目。如果你是这个系列的新手，你可以在潜水前查看[的第一篇博客](/@yozkose3/pytorch-c-intro-b50571762162)。在这个故事中，我们将用 Python 导出 Torchvision 模型，并在 c++代码中使用它们。模型在 Imagenet 上接受训练。我们还将使用 [OpenCV](https://github.com/opencv/opencv) 来读取图像。所有代码都可以在[这个 Github repo](https://github.com/EmreOzkose/pytorch_cpp) 中找到。

# 内容

1.  装置
2.  导出模型
3.  在 c++中使用导出的预训练模型

# 装置

我们必须安装 OpenCV 来读取图像。要安装它，请遵循以下说明

```
git clone [https://github.com/opencv/opencv.git](https://github.com/opencv/opencv.git)
cd opencv
mkdir build && cd build
cmake -D GLIBCXX_USE_CXX11_ABI=0 ..
make -j12
sudo make install
```

libtorch 和 opencv 之间可能存在不匹配问题。当你在没有`-D GLIBCXX_USE_CXX11_ABI=0`的情况下安装 openCV，你可能会面临以下问题。

```
undefined reference **to** `cv::imread(std::string **const**&, int)*'* collect2: **error**: ld returned 1 **exit** status
```

有两种方法可以解决这个问题:

1.  构建 openCV 时使用`-D GLIBCXX_USE_CXX11_ABI=0`
2.  使用 [cxx11-ABI 代替 cxx11 之前的 ABI](https://pytorch.org/get-started/locally/) 。

# 导出模型

如果你读了之前的博客，你可能对 jit 模型很熟悉。我们应该将 Pytorch 模型转换成 Python 中自身的 jit 版本。

现在，我们有一个跟踪的 jit 模型加载到 c++代码中。为了简单起见，我选择了 Alexnet，但还有许多模型，如 Resnet，VGG 等…

# 使用导出的预训练模型

在深入主要代码之前，我们应该添加下面几行代码来使用 openCV。

```
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(simple-tensor-operations "${OpenCV_LIBS}")
```

由于预训练模型是在 ImageNet 上训练的，所以我们需要知道索引对应的类名。

这样，我们就可以创建一个 map 对象，从获得的索引中获取相应的类名。现在，我们应该用 openCV 读取图像。

我们应该根据预训练模型的训练来归一化图像。用平均值[0.485，0.456，0.406]和标准差[0.229，0.224，0.225]对预训练的火炬视觉模型进行归一化。在规范化之后，我们应该加载我们的模型并向前运行。

现在我们可以构建和测试。

```
cmake -DCMAKE_PREFIX_PATH=/path/to/libtorch .. && cmake --build . --config Release && ./detection /path/to/bees/16838648_415acd9e3f.jpg
```

这应该给

```
Prediction: bee
```

# 结论

在这篇博客中，我们用 Python 导出 Alexnet，并用 c++来预测图像。我们使用 openCV 读取图像。所有代码都可以在[这个 Github Repo](https://github.com/EmreOzkose/pytorch_cpp) 中找到。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)