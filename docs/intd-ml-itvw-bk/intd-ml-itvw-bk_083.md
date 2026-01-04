# 6.2 复杂度和数值分析

> 原文：[`huyenchip.com/ml-interviews-book/contents/6.2-complexity-and-numerical-analysis.html`](https://huyenchip.com/ml-interviews-book/contents/6.2-complexity-and-numerical-analysis.html)

*如果某些字符似乎缺失，那是因为 MathJax 没有正确加载。刷新页面应该可以解决这个问题。*

由于最近在机器学习中的大多数突破都来自于需要大量内存和计算能力的更大模型，因此，不仅要知道如何实现模型，还要知道如何扩展模型。为了扩展模型，我们需要能够估计内存需求和计算成本，以及在训练和部署机器学习模型时减轻数值不稳定性。以下是一些可以提出的问题，以评估你对数值稳定性和可扩展性的理解。

1.  矩阵乘法

    1.  [E] 你有三个矩阵：\(A\)、\(B\) 和 \(C\)，你需要计算 \(A \times B \times C\) 的乘积。你会按照什么顺序执行乘法，为什么？

    1.  [M] 现在你需要计算 \(A\) 矩阵的乘积。你将如何确定执行乘法的顺序？

1.  [E] 深度学习中数值不稳定的可能原因有哪些？

1.  [E] 在许多机器学习技术（例如批量归一化）中，我们经常看到在计算中添加了一个小的项。这个项的目的是什么？

1.  [E] 什么使得 GPU 在深度学习中变得流行？它们与 TPU 相比如何？

1.  [M] 当我们说一个问题是无解的时，这意味着什么？

1.  [H] 对循环神经网络进行反向传播的时间复杂度和空间复杂度是多少？

1.  [H] 仅了解模型架构及其超参数是否足以计算该模型的内存需求？

1.  [H] 你的模型在单个 GPU 上运行良好，但在 8 个 GPU 上训练时给出较差的结果。这可能是由于什么原因？你会如何解决这个问题？

1.  [H] 降低模型精度我们能获得哪些好处？我们可能会遇到什么问题？如何解决这些问题？

1.  [H] 如何以最小的精度损失计算 100 万个浮点数的平均值？

1.  [H] 如果一个批次分散在多个 GPU 上，我们应该如何实现批量归一化？

1.  [M] 给定以下代码片段。它可能存在什么问题？你会如何改进它？**提示**：这是一个在[StackOverflow](https://stackoverflow.com/questions/39667089/python-vectorizing-nested-for-loops/39667342)上实际提出的问题。

```py
import numpy as np

def within_radius(a, b, radius):
    if np.linalg.norm(a - b) < radius:
        return 1
    return 0

def make_mask(volume, roi, radius):
    mask = np.zeros(volume.shape)
    for x in range(volume.shape[0]):
        for y in range(volume.shape[1]):
            for z in range(volume.shape[2]):
                mask[x, y, z] = within_radius((x, y, z), roi, radius)
    return mask 
```
