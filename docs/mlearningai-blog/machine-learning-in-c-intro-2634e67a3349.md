# C++中的机器学习#简介

> 原文：<https://medium.com/mlearning-ai/machine-learning-in-c-intro-2634e67a3349?source=collection_archive---------10----------------------->

![](img/aea5cfecfbcde8ce458ae9417bafa301.png)

# 介绍

机器学习在 Python 中被广泛编码。这对于培训来说是可以的，但是我们必须让我们的产品更快，并集成部署的应用程序。为此，我决定提供这方面的教程。希望这些教程对工程师和研究人员有所帮助。所有代码都可以在 [Github repo](https://github.com/EmreOzkose/machine_learning_in_cpp) 获得。如果你正在开发 PyTorch，你也可以在 c++中查看 PyTorch 的[这个系列](/mlearning-ai/pytorch-c-intro-b50571762162)。

# 装置

我们将使用 c++中的机器学习库 [mlpack](https://github.com/mlpack/mlpack) 库。要安装 mlpack，您可以遵循以下步骤。

## 安装犰狳

```
sudo apt-get install liblapack-dev
sudo apt-get install libblas-dev
sudo apt-get install libboost-dev
sudo apt-get install libarmadillo-dev
```

## 安装 ensmallen

```
git clone [https://github.com/mlpack/ensmallen.git](https://github.com/mlpack/ensmallen.git)
cd ensmallen
mkdir build & cd build
cmake ..
sudo make install
```

## 安装谷物

```
sudo apt-get install libcereal-dev
```

## 安装 mlpack

```
git clone [https://github.com/mlpack/mlpack.git](https://github.com/mlpack/mlpack.git)
cd mlpack
mkdir build && cd build
cmake ..
sudo make install
```

在这些步骤之后，我们可以在 c++代码中包含`mlpack.hpp`。

# 总结

在这篇博客中，我简单介绍了 c++系列中的机器学习，并给出了安装 mlpack 的步骤。我们将在本系列的后续博客中学习如何用 c++实现/使用机器学习算法。所有代码都可以在 [Github repo](https://github.com/EmreOzkose/machine_learning_in_cpp) 中获得。

[](https://github.com/EmreOzkose/machine_learning_in_cpp) [## GitHub-EmreOzkose/machine _ learning _ in _ CPP:如何使用机器学习算法的教程…

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/EmreOzkose/machine_learning_in_cpp) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)