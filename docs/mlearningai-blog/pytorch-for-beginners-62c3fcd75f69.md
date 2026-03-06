# Pytorch 适合初学者💫

> 原文：<https://medium.com/mlearning-ai/pytorch-for-beginners-62c3fcd75f69?source=collection_archive---------3----------------------->

## 第一部分:张量的基本运算

![](img/fc16b8232b6f558b37d89dcb1aff7263.png)

> 本文将向您介绍 Pytorch 的基本概念，并逐步指导您如何使用 Pytorch 在张量中实现一些基本操作(这是必须学习的)。

本文将涵盖以下主题:
1 .什么是张量
2。基本数组和张量的区别
3。如何创建张量
4？使用张量的基本数学运算

![](img/025c740e5a4f2d97b4e330ca3bd89ece.png)

# Pytorch 是什么？

PyTorch 是基于 Torch 库的开源机器学习库，用于计算机视觉和自然语言处理等应用，主要由脸书的人工智能研究实验室开发

# Numpy 和 Pytorch 有什么区别？

简单地说，Numpy 创建的数组将在您的 CPU 内存上工作。使用 Pytorch，您可以创建能够使用 GPU 进行快速处理的张量。

张量有一些额外的功能，如自动梯度，这将有助于以更简单的方式实现神经网络。

这两个框架最重要的区别是命名。Numpy 称之为张量(高维矩阵或向量)数组，而在 PyTorch 中只称之为张量。其他的都挺像的。

# 为什么是 PyTorch？

即使您已经知道 Numpy，仍然有几个理由转而使用 PyTorch 进行张量计算。主要原因是 GPU 加速。正如你将看到的，使用 PyTorch 的 GPU 是超级容易和超级快。如果你做大型计算，这是有益的，因为它加快了很多事情。

# 为什么是 Numpy？

Numpy 是线性代数最常用的计算框架。Numpy 的一个很好的用例是快速实验和小型项目，因为与 PyTorch 相比，Numpy 是一个轻量级框架。

![](img/b705c7690ecf240724cbcfa49afc16e4.png)

> 注意:如果您的机器上没有安装 PyTorch。尝试使用谷歌联合实验室或 Kaggle 笔记本。

 [## 在线运行数据科学和机器学习代码

### Kaggle 笔记本是一个计算环境，支持可重复的协作分析。

www.kaggle.com](https://www.kaggle.com/code) [](https://colab.research.google.com/?utm_source=scs-index) [## 谷歌联合实验室

### 编辑描述

colab.research.google.com](https://colab.research.google.com/?utm_source=scs-index) 

我们开始吧！

```
import torch 
import numpy as np
```

# 使用 Torch 创建张量

![](img/0dfa6dd56153c6e8c0401e8b086cf16f.png)

张量是一种多维数据结构。向量是一维数据结构，矩阵是二维数据结构。张量表面上与这些其他数据结构相似，但区别在于它们可以存在于从零到 n 的维度中

## 从简单列表和 Numpy 数组创建张量

```
data = [[1 , 2 ] , [3 , 4]]
np.array(data)
data = torch.tensor(data)
```

## 要检查数据类型，只需编写

```
data.dtype
```

## 创建一个大小为 3 行×4 列的空张量

```
a = torch.empty(size =(3,4))
a
```

![](img/95baba06e48e4b652bd1e59a75ab4f96.png)

> 注意:在你的张量中应该有一些随机值，但是不要担心空的张量会被创建，这样你就可以在其中存储一些新的张量值，这样你就可以覆盖这些值了

## 创建一个大小为(3 行×4 列)的空二维张量

```
b = torch.empty(size=(2 , 3 ,4))
b
```

![](img/df0d40aeceddc58c9849aafda2bf8b0c.png)

## 创建大小为(3 行 x 4 列)的张量，并将所有值初始化为 1

```
a =torch.ones(size = (3,4))
print(a)
print(a*3)
```

![](img/47a357de4fae69f3f47c04e2e438f4e3.png)

## 创建一个包含一系列数字的张量

```
print(torch.arange(10))
print(torch.arange(10).shape)
```

![](img/9f12c8431af4119090bde97991f7b91c.png)

```
torch.linspace(start = 1 , end = 10 , steps  = 20)
```

![](img/071ce47995ba5f1ad3796b722c2db2f3.png)

## 使用 Numpy 数组创建张量

```
arr = np.arange(10)
torch.tensor(arr)
```

![](img/67613634074c02a3ee4b979201f6fe86.png)

## 要改变张量的数据类型，只需写

```
torch.tensor(arr , dtype = torch.float32)
```

![](img/8528eae4d797564cd44557613bd46908.png)

## 将张量转换为 Numpy 数组写入

```
t.numpy()
```

![](img/60d0ddc6f1e4485a0331db3d4efd1536.png)

## 创建具有对角线值的张量

```
torch.eye(3)
```

![](img/61ede040952118ad05263ea08dc486a4.png)

```
t1 = torch.diag(torch.arange(5))
t1
```

![](img/6f92af93c2055daa5ce407516e100e91.png)

```
torch.diag(t1)
```

![](img/1336b906f73fffb995732a40bf59717d.png)

## 创建一个大小为 3 行 4 列的随机数数组

```
torch.rand(size = (3 ,4))
```

![](img/021d7d893ddc3d43ee97a7ab2001eb33.png)

```
x = torch.ones(size = (3 , 4))
y = torch.ones(size = (3 , 4))*4
print(x ,"\n\n", y)
```

![](img/83b2de153f57f4c143a00dee13ba861c.png)

## 添加两个张量

![](img/21dd3364579e61ac230bb55b6d4e79e1.png)

有不同的方法来添加张量

```
#1
z = torch.empty(size = (3,4))
torch.add(x,y ,out=z)
```

![](img/ec2c4072396f019fbaa8c94ed9a985de.png)

```
#2
z = torch.add(x ,y)
z
```

![](img/baadc1d9b5ea2e32dca8993a4a96a833.png)

```
#3
z = x+y
z
```

![](img/4c1be825977843f9f0085a5cc44f065e.png)

创建不同的变量来存储值会占用内存，为了避免这种情况，可以添加两个变量，并将总和存储在一个变量中。

```
#x+=y
x.add_(y) ## to save memory
```

![](img/06ddee13b63e0ce382c4fbe2ad48a837.png)

## 两个张量之间的乘法

![](img/b37f912ab8213e684a230f199f8ad70b.png)

两个张量相乘有不同的方法

```
#1
x = torch.ones(size = (3 , 4))
y = torch.ones(size = (4, 5))*3
torch.matmul(x,y)
```

![](img/97413a79473a7318b6ec08e755a8c546.png)

```
#2
torch.mm(x , y)
```

![](img/f278c404a5f833d3c6be71b71d849b37.png)

## 批量倍增

![](img/61ee082538e57a4152401393afaf778a.png)

```
batch = 10m = 30
n = 20
k = 10
x = torch.rand(size =(batch , m , n ))
y = torch.rand(size =(batch , n , k ))
torch.bmm(x,y).shape
```

![](img/9ae248fba5e58deffa0c5108255045e5.png)

## 改变张量的形状和大小

```
t = torch.arange(15)
t
```

![](img/45e2c90454e414423e075445c5f21629.png)

```
t.shape
```

![](img/babdeca533a37c5b59e730b36e8b841b.png)

```
t.unsqueeze(axis = 0 ).shape #row
```

![](img/9cda957ac65f79de796ce207d476c236.png)

```
t.unsqueeze(axis = 1)
```

![](img/405d2ec6ddd306621f5f4535376ceb59.png)

您还可以使用查看和整形方法更改特定行和列中的张量。

```
t.view(3,5)
```

![](img/4eebcdf7ed39d58fc47b389ec682aabc.png)

```
t1 = torch.linspace(0 , 15 , 24)
t1
```

![](img/275eac3400befce506badcdc9e683743.png)

```
t1.shape
```

![](img/30d053ba2f6acd47536272afc9efb30f.png)

```
t1.view(2 , 3 ,4 )
# 2 dimensions , 3 rows , 4 columns
```

![](img/8bf0ba93651b5c1ac95cdc8644435aa0.png)

```
t1.reshape(2 , 3 , 4)
# 2 dimensions , 3 rows and 4 columns
```

![](img/9072bfd873438b0d234811ac0041d957.png)

如果你想开始使用 Pytorch，以上是一些基本的操作。在本文的第二部分，我将向您展示如何使用这些基本操作来从头创建一个神经网络。如果你还有疑问，请随时联系我。

复制并编辑本 Kaggle 笔记本进行练习:

[](https://www.kaggle.com/aakashjoshi123/pytorch-for-beginners) [## Pytorch 初学者✨

### 使用 Kaggle 笔记本探索和运行机器学习代码|使用来自[私有数据源]的数据

www.kaggle.com](https://www.kaggle.com/aakashjoshi123/pytorch-for-beginners) 

在这里看看这个系列的第二部分！👇

[](/mlearning-ai/pytorch-for-beginners-d759cb85ff1a) [## Pytorch 适合初学者💫

### 第 2 部分:神经网络基础及其从头实现

medium.com](/mlearning-ai/pytorch-for-beginners-d759cb85ff1a) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

[成为 ML 写手](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)