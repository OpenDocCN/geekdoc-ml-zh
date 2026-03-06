# 激活功能

> 原文：<https://medium.com/mlearning-ai/activation-functions-17f8e7d5fcf8?source=collection_archive---------7----------------------->

激活函数是附属于网络中每个神经元的数学函数，并确定它是否应该被激活。通常，在每一层中，神经元使用权重和偏差对输入执行线性变换:

![](img/ac12ae7a52eb9b3846bf0855c40b27ab.png)

然后，将激活函数应用于上述结果:

![](img/ce7955486865062cfa1c851d03fa2f32.png)

当前层的输出成为下一层的输入。这个过程与网络中的所有隐藏层一起重复。这种信息的前向运动称为 ***前向传播*** 。

激活函数有一些重要的意义:

*   它在神经网络中引入非线性变换，使网络具有学习和执行更复杂任务的能力。想象没有激活函数的神经网络，因此使用权重作为偏差对输入只有线性变换。在这种情况下，神经网络作为线性回归模型工作，那么它的功能将会减弱，并且不能执行复杂的任务。
*   此外，一些激活函数也有助于将每个神经元的输出标准化到一个新的范围，在 0 和 1 之间，或在-1 和 1 之间。

现在，我们将发现一些流行的激活功能:

# **1。乙状结肠功能(逻辑激活功能)**

该功能定义如下:

![](img/7ced7725fe2e01d7bd498e01746ac372.png)

```
import matplotlib.pyplot as plt
import numpy as npx = np.arange(-6,6, 0.01)def plot(func, ylim):
  plt.plot(x, func(x), c = 'r', lw = 3)
  plt.xticks(fontsize = 14)
  plt.yticks(fontsize = 14)
  plt.axhline(c = 'k', lw = 1)
  plt.axvline(c = 'k', lw = 1)
  plt.ylim(ylim)
  plt.box(on = None)
  plt.grid(alpha = 0.4, ls = '-.')
```

乙状结肠功能的可视化:

```
def sigmoid(x):
  return 1/ (1 + np.exp(-x))plot(sigmoid, ylim = (-0.25, 1.25))
```

![](img/aa1ef18ceebe8014228718c187eed84a.png)

*   由于输出值介于 0 和 1 之间，因此该函数尤其适用于需要预测输出概率的模型。
*   该函数可微分，其导数由下式给出:

![](img/3cf70b204795305f57aa8c2e7e2c30f4.png)

```
def derivative_sigmoid(x):
  return sigmoid(x)*(1-sigmoid(x))plot(derivative_sigmoid, ylim=(-0.02, 0.35))
```

![](img/f11129831fed8820a5b8c4296715593b.png)

**优点:**

*   输出在范围 0 和 1 内标准化
*   平滑渐变

**缺点:**

*   消失渐变:根据上图，当 x > 6 或 x < -6\. The vanishing gradient may stop the neural network from further training.
*   The outputs are not centered.
*   The computational complexity is large.

# 2\. Tanh function

Tanh function is defined as:

![](img/301dc9c404b3f3eada50e6fd74c77ed7.png)

This function is very similar to the sigmoid function. However, it is symmetric around the origin. The outputs of this function are centered and range between -1 and 1.

```
def tanh(x):
  return 2*sigmoid(2*x) - 1plot(tanh, ylim = (-1.4, 1.4))
```

![](img/055c1d70751fb1928d391ae0318f0aac.png)

tanh(x) is a monotone, continuous and differentiable function. Its derivation is determined by:

![](img/dc17534cfe1fbccced414d5a4f0b66b3.png)

```
def derivative_tanh(x):
  return 1 - (tanh(x))**2plot(derivative_tanh, ylim = (-0.2, 1.2))
```

![](img/62569a338d06a0e0954fab670c566e6e.png)

Similar to the sigmoid function, the derivation of this function is closed to zero when x > 3 或 x < -3\. Hence, that also leads to vanishing gradient phenomena during training neural networks.

# 3\. Rectified Linear Unit (ReLU) function

This function has been used widely in the deep learning domain. Its formula is defined by:

![](img/d886659529292eaa672f58e94fff43c8.png)

```
relu = np.vectorize(lambda x: x if x > 0 else 0, otypes=[np.float])plot(relu, ylim=(-0.3, 1.5))
```

![](img/751790b6917ca81ed600b56ef278ebfa.png)

This function is continuous, monotone in ℝ, and differentiable for all x ≠ 0.

The derivation of the ReLU function is determined by:

![](img/9732122087508753b6e85fa91cdfd0f6.png)

```
derivative_relu = np.vectorize(lambda x:1 if x > 0 else 0, otypes=[np.float])plot(derivative_relu, ylim = (-0.5, 1.5))
```

![](img/e6981c0960bfd6702fb2a9f63a5a3661.png)

**时，渐变值接近 0 优点:**

*   与 sigmoid 和 tanh 函数相比，此函数的计算效率更高。因此，使用该功能可以加快训练时间。

**缺点:**

*   输出既不是标准化的，也不是居中的。
*   这个函数在原点不可微
*   由于 ReLU 只激活正信号，而去激活所有负信号，因此它不能为负输入值提供一致的预测。
*   当输入接近零或负值时，导数值为零。因此，在反向传播期间，一些神经元的权重和偏差没有被更新。

# 4.泄漏 ReLU 函数

这个函数是 ReLU 函数的改进版本，克服了 ReLU 的一些缺点。

![](img/10ac69f632d8ad4f7f9abbc4c91c01b9.png)

```
a = 0.01
leaky_relu = np.vectorize(lambda x: x if x > 0 else a*x, otypes= [np.float])plot(leaky_relu, ylim = (-0.5, 1.5))
```

![](img/30dfd30b8dbf6faf202579c38f9b769c.png)

在这种情况下，所有负值 x 都被 ax 代替，其中 a ∈ (0，1)。这允许我们不断更新所有神经元的权重和偏差。

**优势:**

*   这个函数很简单，其计算复杂度仍然小于 sigmoid 和 tanh 函数。
*   不断更新所有神经元的权重和偏差，因此它确实支持反向传播。

**缺点:**虽然它保持了负向神经元的活跃，但是这些神经元被重新调节成了一个很小的值。换句话说，对应于这些负向神经元的信号显著减少。因此，漏 ReLU 不能为负输入值提供一致的预测。

# 5.Softmax 函数

softmax 函数由下式定义:

![](img/9ab8415ac1f4e99c5ca878b583fa13bc.png)

```
def softmax(x):
  z = np.exp(x)
  return z/z.sum()softmax([0.4, 2, 5])>>> array([0.00948431, 0.04697607, 0.94353962])
```

softmax 函数的导数:

![](img/769989a00fb3c6adda496f533e680c9c.png)

当 j ≠ i 时:

![](img/100dce3e586f9f919ee21a48316247e9.png)

让我们来表示:

![](img/38a03e3b61a7d5c6abc1c5555a341475.png)

然后，softmax 函数的导数可以重写为:

![](img/a67674a07c818d62e3b766b518e5e682.png)

softmax 函数返回介于 0 和 1 之间的输出值，这些输出的总和等于 1。因此，此函数对于多类分类非常有用，我们的目标是预测数据点属于特定类的概率。

**结论:**我们发现了一些在构建卷积神经网络时经常出现的激活函数。希望这篇帖子对你有帮助。

如果你有任何问题，请随意写在评论区。

谢谢大家！

***我的博客页面:***[https://lekhuyen.medium.com/](https://lekhuyen.medium.com/)