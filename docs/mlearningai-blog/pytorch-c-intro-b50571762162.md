# py torch & c++ #简介

> 原文：<https://medium.com/mlearning-ai/pytorch-c-intro-b50571762162?source=collection_archive---------4----------------------->

![](img/2df193ef8fcb85dc3308b62b246c4add.png)

# 介绍

近年来，研究人员和工程师在人工智能方面取得了巨大成功。虽然主要原因是不断增加的数据和计算能力，但还有另外两种力量有助于改进:开源编码和深度学习工具的兴起。这两个因素也很关键，因为发明/发现并不总是影响实际生活，发明也应该转移到现实生活中。此时，Pytorch c++ API 的重要性正在增加，因为我们通常要求实时系数小于 1，而 Python 可能不提供这一要求。

在本系列中，我将尝试提供 Pytorch c++ api 的示例/实践/项目。可能会有一些 onnx 模式而不是 Pytorch 本身，但我认为它可以被归类为 Pytorch 的扩展。

# 装置

```
pytorch = 1.12.1
libtorch = [https://download.pytorch.org/libtorch/cu113/libtorch-shared-with-deps-1.12.1%2Bcu113.zip](https://download.pytorch.org/libtorch/cu113/libtorch-shared-with-deps-1.12.1%2Bcu113.zip)
```

我遵循了 [Pytorch c++安装步骤](https://pytorch.org/cppdocs/installing.html)。下载了 Libtorch，编写了一个简单的 cpp 脚本，并用下面的命令构建它

```
mkdir build
cd build
cmake -DCMAKE_PREFIX_PATH**=**/absolute/path/to/libtorch ..
cmake --build . --config Release
```

# **内容**

CPP API 包含几个部分，但我通常会使用其中的两个部分:

**ATen:** 张量和数学运算库。
**torch script:**torch script JIT 编译器和解释器。

我将在下面的博客中更深入地提到这一点。

你可以用 ATen 做数学运算，用 torch::nn::Module 创建 AI 模型，做推理，甚至用 CPP 脚本训练它们。虽然主要用途可能是在 CPU 上，但是您也可以使用 GPU。其实我也不知道有没有效率，但是我会实际尝试一下:)。

# 结论

在这篇博客中，我介绍了这个系列。我计划每周发表一篇博客。我希望这个系列对在 edge 设备上创建 Pytorch 应用程序有用。继续关注:)。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)