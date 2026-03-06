# PyTorch 基础知识:入门所需的唯一指南—第 1 部分

> 原文：<https://medium.com/mlearning-ai/pytorch-basics-the-only-guide-you-need-to-get-started-f8e67b3d056a?source=collection_archive---------5----------------------->

![](img/d131da4fa7446d89d82f8474bac19b77.png)

你已经决定学习深度学习，并且已经学了一些理论，现在想做一些动手实践，但是你陷入了从哪个框架开始(Tensorflow 还是 PyTorch)的两难境地？

别担心，我也经历过这种情况，这也是我开始写这一系列文章的原因，我会写一系列文章来帮助你开始使用 PyTorch！

**那么，我们开始吧！**

![](img/83068f3c0686bbe235c3f21d4f9a870e.png)

[Source](https://tenor.com/view/lets-get-started-rosanna-pansino-lets-started-lets-make-a-start-lets-begin-gif-19252897)

**文章的流程:**

*   PyTorch 简介
*   张量
*   初始化张量
*   张量的属性
*   张量上的运算
*   PyTorch-NumPy 互操作性
*   GPU/CPU 兼容性

# **py torch 是什么？**

PyTorch 是一个基于 Torch 库的开源机器学习框架，用于计算机视觉和自然语言处理等应用，主要由脸书(现为 Meta)人工智能研究实验室开发。它为深度学习的科学计算提供了最大的灵活性和速度。还有，**用 GPU 的力量替代 NumPy。**

# **为什么选择 PyTorch？**

根据我的研究，TensorFlow、Keras 和 PyTorch 是机器学习社区中提到最多的库。如果您想要开发用于生产的模型，开发需要在移动平台上部署的模型，以及需要使用大规模分布式模型训练，Tensorflow 非常适合。

而 PyTorch 很适合用于研究目的，如果你想拥有 Pythonic 式的一切(比如我😉)!

# 张量

张量是非常类似于数组和矩阵的特殊数据结构。在 PyTorch 中，我们使用张量来编码模型的输入和输出，以及模型的参数。**tensor 类似于** [**NumPy 的**](https://numpy.org/) **ndarrays，唯一的区别就是可以在 GPU 和其他硬件加速器上运行。**

比如 1d-张量是向量，2d-张量是矩阵，3d-张量是立方体，4d-张量是立方体的向量。

如果你想直观地理解它，这里有一个惊人的 Youtube 视频，维滕贝格大学物理系教授丹尼尔·弗莱舍在视频中对此进行了精彩的解释。

## 初始化张量:

让我们看看如何在 PyTorch 中创建张量。张量可以用各种方法初始化。

让我们导入必要的库:

```
import torch
import numpy as np
```

1.  **直接来自数据:**

```
data = [[1, 2],[3, 4]]
**tensor_data = torch.tensor(data)****# Example:** print(tensor_data)**# Output:**
tensor([[1, 2],
        [3, 4]])
```

**2。用随机值创建张量:**

```
**random_tensor = torch.Tensor(n,m)**   # where n is the number of rows                 
and m is the number of columns (n x m)**# Example:** random_tensor = torch.Tensor(2,3)
print(random_tensor)**# Output:**
tensor([[6.8664e-44, 7.7071e-44, 1.1771e-43],
        [6.7262e-44, 7.9874e-44, 8.1275e-44]])
```

**3。用 a 和 b 之间的随机值创建一个张量:**

```
**uniform_tensor = torch.Tensor(n, m).uniform_(a, b)**   # where a and b are any natural numbers**# Example:** uniform_tensor = torch.Tensor(3,3).uniform_(1,5)
print(uniform_tensor)**# Output:** tensor([[2.2433, 3.1906, 4.2577],
        [3.6101, 1.9020, 2.6306],
        [1.2986, 3.6643, 1.4555]])
```

**4。从区间[0，1]上的均匀分布创建一个填充了随机数的张量:**

```
**rand_tensor = torch.rand(n, m)****# Example:**
rand_tensor = torch.rand(3, 3)
print(rand_tensor)**# Output:**
tensor([[0.6445, 0.5295, 0.8926],
        [0.6845, 0.2812, 0.7323],
        [0.9651, 0.4299, 0.8283]])
```

**5。创建一个大小为 *n* x *m* :** 的零张量

```
**zero_tensor = torch.zeros(n, m)****# Example:**
zero_tensor = torch.zeros(3, 3)
print(zero_tensor)**# Output:**
tensor([[0., 0., 0.],
        [0., 0., 0.],
        [0., 0., 0.]])
```

**6。创建大小为 *n* x *m* :** 的一个张量

```
**ones_tensor = torch.ones(n, m)** **# Example:**
ones_tensor = torch.ones(3, 3)
print(ones_tensor)**# Output:**
tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]])
```

## 张量的属性:

让我们看看如何获得张量的属性，如形状、数据类型以及它们存储在哪个设备上。

```
**tensor = torch.randn(n, m)****print("Shape of a tensor: {}".format(tensor.shape))
print("Data type of a tensor: {}".format(tensor.dtype))
print("Dimension of a tensor: {}".format(tensor.dim()))
print("Device tensor is stored on: {}".format(tensor.device))****# Example:**
tensor = torch.randn(3, 3)print("Shape of a tensor: {}".format(tensor.shape))
print("Data type of a tensor: {}".format(tensor.dtype))
print("Dimension of a tensor: {}".format(tensor.dim()))
print("Device tensor is stored on: {}".format(tensor.device))**# Output:**
Shape of a tensor: torch.Size([3, 3])
Dimension of a tensor: 2
Data type of a tensor: torch.float32
Device tensor is stored on: cpu
```

## 张量上的运算:

张量上有许多运算，我们将在这里讨论其中最常见和最重要的运算。如果你有兴趣了解所有的操作——在这里[讨论它们](https://pytorch.org/docs/stable/torch.html)。

1.  **分度操作:**

使用此操作，您可以访问或替换张量中的元素。

```
**tensor = torch.Tensor([[1, 2], [3, 4]])****# Replace an element at position 0,1**
tensor[0][1] = 7
print(tensor)**# Output:**
tensor([[1., 7.],
        [3., 4.]])**# Access an element at position 1,0** print(tensor[1][0])
print(tensor[1][0].item())    # using .item() you can access the scalar object**# Output:**
tensor(3.)
3.0
```

**2。切片操作:**

使用此操作，可以访问特定索引范围内的元素。

```
tensor = torch.Tensor([[1, 2], [3, 4], [5,6]])**# First element of every row** print(tensor[:,0])**# Output:**
tensor([1., 3., 5.])**# Last element from every row** print(tensor[:,-1])**# Output:**
tensor([2., 4., 6.])**# All elements from second row** print(tensor[1,:])**# Output:**
tensor([3., 4.])**# All elements from last two rows** print(tensor[1:,:])**# Output:**
tensor([[3., 4.],
        [5., 6.]])
```

**3。连接或连接张量:**

该操作用于沿着给定的维度连接一系列张量。

```
tensor_1 = torch.Tensor([[1, 2], [3, 4], [5,6]])
tensor_2 = torch.Tensor([[8,9],[10,11],[12,13]])new_tensor = torch.cat([tensor, ten2], dim=1)
print(new_tensor)**# Output:** tensor([[ 1.,  2.,  8.,  9.],
        [ 3.,  4., 10., 11.],
        [ 5.,  6., 12., 13.]])
```

还有一个类似于连接的操作，称为 [torch.stack](https://pytorch.org/docs/stable/generated/torch.stack.html) 。

**4。重塑张量:**

使用这个操作，你可以将你的张量整形为*n*×m 的大小。

```
tensor = torch.Tensor([[1, 2], [3, 4]])
reshaped_tensor_1 = reshape_tensor.view(1,4)
reshaped_tensor_2 = reshape_tensor.view(4,1)print("Reshaped tensor 1: ",reshaped_tensor_1)
print("\nReshaped tensor 2: ",reshaped_tensor_2)**# Output:** Reshaped tensor 1 tensor:
([[1., 2., 3., 4.]])

Reshaped tensor 2 tensor:
([[1.],
  [2.],
  [3.],
  [4.]])
```

**5。转置:**

您可以使用`.t()`或`.permute(-1, 0).`对张量执行转置操作

```
tensor = torch.Tensor([[1, 2], [3, 4], [5,6]])
print("Tensor: ",tensor)
print("\nTensor after transpose: ",tensor.t())**# Output:** Tensor:  tensor([[1., 2.],
        [3., 4.],
        [5., 6.]])

Tensor after transpose:  tensor([[1., 3., 5.],
        [2., 4., 6.]])
```

使用置换函数进行转置:

```
tensor = torch.Tensor([[1, 2], [3, 4], [5,6]])
print("Tensor: ",tensor)
print("\nTensor after transpose: ",tensor.permute(-1,0))**# Output:** Tensor:  
tensor([[1., 2.],
        [3., 4.],
        [5., 6.]])

Tensor after transpose:  
tensor([[1., 3., 5.],
        [2., 4., 6.]])
```

**6。叉积:**

你可以用两个中的任何一个得到两个张量的叉积:`tensor_1**.**cross(tensor_2)`或者`torch.cross(tensor_1, tensor_2)`

```
tensor_1 = torch.Tensor([[1, 2], [3, 4], [5,6]])
tensor_2 = torch.Tensor([[8, 9],[10, 11],[12, 13]])cross_prod = tensor_1.cross(tensor_2)
print(cross_prod)**# Output:** tensor([[-14., -14.],
        [ 28.,  28.],
        [-14., -14.]])
```

**7。矩阵产品:**

执行两个张量的矩阵乘法。如果 tensor_1 是一个( *n* × *m* )张量，tensor_2 是一个( *m* × *p* )张量，那么输出将是( *n* × *p* )张量。

```
tensor_1 = torch.Tensor([[1, 2, 3], [3, 5, 6], [7, 8, 9]])
tensor_2 = torch.Tensor([[10, 11, 12],[13, 14, 15],[16, 17, 18]])**# Using tensor_1.mm(tensor_2)** matrix_prod = tensor_1.mm(tensor_2)
print(matrix_prod)**# Output:** tensor([[ 84.,  90.,  96.],
        [191., 205., 219.],
        [318., 342., 366.]]) **# Using torch.mm(tensor_1, tensor_2)** maxtrix_prod = torch.mm(tensor_1,tensor_2)
print(matrix_prod)**# Output:** tensor([[ 84.,  90.,  96.],
        [191., 205., 219.],
        [318., 342., 366.]])
```

**8。逐元素乘法:**

该操作执行两个张量的元素乘积。

```
tensor_1 = torch.Tensor([[1, 2, 3], [3, 5, 6], [7, 8, 9]])
tensor_2 = torch.Tensor([[10, 11, 12],[13, 14, 15],[16, 17, 18]])product = tensor_1.mul(tensor_2)print(product)**# Output:** tensor([[ 10.,  22.,  36.],
        [ 39.,  70.,  90.],
        [112., 136., 162.]])
```

**9。张量之和:**

该操作返回张量所有值的和。

```
tensor_1 = torch.Tensor([[10, 11, 12],[13, 14, 15],[16, 17, 18]])agg = tensor_1.sum()
agg_item = agg.item()
print(agg_item)**# Output:**
126.0
```

**10。就地添加操作:**

这个操作将一个标量值加到张量的每个元素上。

```
tensor_1 = torch.Tensor([[1, 2, 3], [3, 5, 6], [7, 8, 9]])print(tensor_1)
tensor_1.add_(5)
print("\n",tensor_1)**# Output:** tensor([[1., 2., 3.],
        [3., 5., 6.],
        [7., 8., 9.]])

 tensor([[4.,  5.,  6.],
        [ 6.,  8.,  9.],
        [10., 11., 12.]])
```

## PyTorch-NumPy 互操作性:

您可以轻松地将 NumPy 数组转换为 PyTorch 张量，反之亦然。

1.  **PyTorch 张量到 NumPy 数组:**

```
t = torch.Tensor([[1, 2, 3], [3, 5, 6], [7, 8, 9]])
print("Pytorch tensor: ", t)
print("\n",type(t))n = t.numpy()
print("\nNumpy array: ", n)
print("\n",type(n))**# Output:** Pytorch tensor:
tensor([[1., 2., 3.],
        [3., 5., 6.],
        [7., 8., 9.]])

<class 'torch.Tensor'>

Numpy array:
[[1\. 2\. 3.]
 [3\. 5\. 6.]
 [7\. 8\. 9.]]<class 'numpy.ndarray'>
```

**2。NumPy 数组到 PyTorch 张量:**

```
numpy_array = np.ones(5)  # this method creates a numpy array with all elements as 1print(numpy_array)
print(type(numpy_array))pytorch_tensor = torch.from_numpy(n)
print("\n",pytorch_tensor)
print(type(pytorch_tensor))**# Output:** [1\. 1\. 1\. 1\. 1.]
<class 'numpy.ndarray'>

tensor([1., 1., 1., 1., 1.], dtype=torch.float64)
<class 'torch.Tensor'>
```

## GPU/CPU 兼容性:

如果你的计算机上有 GPU，并且还安装了用于深度学习的 CUDA 工具包，那么你可以释放 GPU 的力量，实现比 CPU 更快的计算。

```
tensor_1 = torch.Tensor([[1, 2, 3], [3, 5, 6], [7, 8, 9]])
tensor_2 = torch.Tensor([[10, 11, 12],[13, 14, 15],[16, 17, 18]])**if** torch.cuda.is_available():
    tensor_1 = tensor_1.cuda()
    tensor_2 = tensor_2.cuda()
```

今天就到这里吧！

我希望你喜欢学习 PyTorch 张量的基础知识。在下一篇教程中，我们将学习更多关于 PyTorch 中的数据集和数据加载器的知识，这样您就可以享受使用 PyTorch 处理数据的乐趣。

## 文章时间表

1.  [张量](https://pawarsaurav842.medium.com/pytorch-basics-the-only-guide-you-need-to-get-started-f8e67b3d056a)
2.  使用 PyTorch 进行数据处理
3.  转换
4.  使用 PyTorch 构建模型
5.  自动微分
6.  优化循环
7.  保存、加载和使用保存的模型

如果你喜欢这篇文章，不要忘记给一些掌声！😉

***拜拜！***

![](img/4cc410949985f2218a90501062adc368.png)[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)