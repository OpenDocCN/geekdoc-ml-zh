# 基于深度学习模型中瓶颈的研究论文

> 原文：<https://medium.com/mlearning-ai/research-papers-based-on-bottlenecks-in-deep-learning-models-33b3b03172c?source=collection_archive---------7----------------------->

![](img/41a45ff2d2fb0dfaba9e24205b5d33dc.png)

Photo by [Kadarius Seegars](https://unsplash.com/@kseegars?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/bottle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

1.  **通过输入最重要的数据来钻信息瓶颈的软木塞(**[**arXiv**](https://arxiv.org/pdf/2105.07181)**)**

**作者:** [彭新宇](https://arxiv.org/search/?searchtype=author&query=Peng%2C+X)，[张嘉伟](https://arxiv.org/search/?searchtype=author&query=Zhang%2C+J)，[王飞跃](https://arxiv.org/search/?searchtype=author&query=Wang%2C+F)，[李丽](https://arxiv.org/search/?searchtype=author&query=Li%2C+L)

**摘要:**最近十年，深度学习已经成为最强大的机器学习工具。然而，如何有效地训练深度神经网络仍有待彻底解决。广泛使用的 minibatch 随机梯度下降(SGD)仍然需要加速。信息瓶颈理论认为优化过程由初始拟合阶段和后续压缩阶段组成，是更好地理解小批量 SGD 学习动态的一个有前途的工具。基于这一原理，我们进一步研究了典型抽样这一有效的数据选择方法，并对它如何帮助加速深度网络的训练过程提出了新的解释。我们表明，如果适当地采用典型抽样，IB 理论中描述的拟合相位将随着梯度近似的高信噪比而提高。此外，这一发现还意味着训练集的先验信息对优化过程至关重要，更好地利用最重要的数据可以帮助信息更快地通过瓶颈。理论分析和在合成和真实数据集上的实验结果都证明了我们的结论。

**2。信息瓶颈理论及其在深度学习中的应用综述(arXiv)**

**作者:** [穆罕默德·阿里·阿洛马尼](https://arxiv.org/search/?searchtype=author&query=Alomrani%2C+M+A)

**摘要:**在过去的十年里，深度神经网络取得了前所未有的进步，继续影响着当今社会的方方面面。随着高性能 GPU 的发展和大量数据的可用性，ML 系统的学习能力已经飙升，从对图片中的数字进行分类到以超人的表现在游戏中击败世界冠军。然而，即使 ML 模型继续取得新的进展，由于缺乏对其内部工作的深入理论理解，它们的实际成功仍然受到阻碍。幸运的是，一种称为信息瓶颈理论的已知信息论方法已经成为更好地理解神经网络学习动力学的一种有前途的方法。原则上，IB 理论将学习建模为数据压缩和信息保留之间的权衡。这项调查的目标是提供一个关于 IB 理论的全面回顾，包括它的信息理论基础和最近提出的理解深度学习模型的应用

**3。缓解边缘机器学习推理瓶颈:加速 Google Edge 模型的实证研究(**[**arXiv**](https://arxiv.org/pdf/2103.00768)**)**

**作者:**

**摘要:**随着对边缘计算需求的增长，许多现代消费设备现在都包含边缘机器学习(ML)加速器，这些加速器可以计算各种神经网络(NN)模型，同时仍然符合严格的资源限制。我们使用 24 个 Google edge NN 模型(包括 CNN、LSTMs、transducers 和 RCNNs)分析了一个商业 Edge TPU，并发现该加速器在计算吞吐量、能量效率和内存访问处理方面存在三个缺点。我们全面研究了所有 Google edge 模型中每个 NN 层的特征，并发现这些缺点源于加速器的一刀切方法，因为不同模型和同一模型中不同层的关键层特征存在大量异质性。我们提出了一个新的加速框架，叫做 Mensa。Mensa 整合了多个异构 ML 边缘加速器(包括片上和近数据加速器)，每个加速器都满足特定模型子集的特征。在运行时，Mensa 安排每一层在最合适的加速器上运行，同时考虑效率和层间依赖性。当我们分析 Google edge NN 模型时，我们发现所有的层都自然地分组为少量的簇，这允许我们只使用三个专门的加速器为这些模型设计 Mensa 的有效实现。在所有 24 个 Google edge 型号中取平均值，Mensa 的能效和吞吐量分别比 Edge TPU 提高了 3.0 倍和 3.1 倍，比最先进的加速器 Eyeriss v2 提高了 2.4 倍和 4.3 倍

**4。通过概念瓶颈模型(**[**【arXiv】**](https://arxiv.org/pdf/2101.01239)**)**可解释的基于神经网络的调制分类

**作者:** [劳伦·王晶](https://arxiv.org/search/?searchtype=author&query=Wong%2C+L+J)，[肖恩·麦克弗森](https://arxiv.org/search/?searchtype=author&query=McPherson%2C+S)

**摘要:**虽然 RFML 有望成为未来无线标准的关键推动者，但 RFML 技术的广泛采用面临的一个重大挑战是深度学习模型缺乏可解释性。这项工作调查了 CB 模型作为一种手段，在基于 DL 的 AMC 的背景下提供内在的决策解释的使用。结果表明，所提出的方法不仅满足单网络基于 DL 的 AMC 算法的性能，而且提供了期望的模型可解释性，并且显示了对训练期间未看到的调制方案进行分类的潜力(即，零触发学习)。

**5。发现并解释 DNNs (arXiv)的表示瓶颈**

**作者:** [邓慧琪](https://arxiv.org/search/?searchtype=author&query=Deng%2C+H)，[任](https://arxiv.org/search/?searchtype=author&query=Ren%2C+Q)，[，](https://arxiv.org/search/?searchtype=author&query=Zhang%2C+H)，[张全世](https://arxiv.org/search/?searchtype=author&query=Zhang%2C+Q)

**摘要:**从深度神经网络编码的输入变量之间交互的复杂性角度，探讨了深度神经网络特征表示的瓶颈。为此，我们关注输入变量之间的多阶交互作用，其中阶代表交互作用的复杂性。我们发现 DNN 更有可能编码太简单的交互和太复杂的交互，但是通常不能学习中等复杂的交互。这种现象被不同任务的不同 dnn 广泛共享。这种现象表明 DNNs 与人类之间存在认知鸿沟，我们称之为表征瓶颈。我们从理论上证明了表示瓶颈的根本原因。此外，我们提出了一个损失来鼓励/惩罚特定复杂性交互的学习，并分析了不同复杂性交互的表征能力