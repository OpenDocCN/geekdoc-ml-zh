# 使用用户界面的自动机器学习视觉

> 原文：<https://medium.com/mlearning-ai/automated-machine-learning-vision-using-ui-9da3db0dde8b?source=collection_archive---------4----------------------->

# 基于视觉的自动化机器学习的 Azure 机器学习 UI

# 先决条件

*   Azure 存储
*   Azure 机器学习工作区
*   Coco 数据集
*   带标签的数据标签项目

# 步伐

*   首先创建一个数据标签项目
*   然后为一些图像绘制边界框，并用标签标记它们

![](img/b79f3b8c5ac068996403d9738736bf00.png)

*   上面的项目是使用开源图像数据集创建的
*   一旦你有了标签，你就可以给图片加标签了
*   标记后，让自动标记运行
*   现在根据自动贴标机贴的标签，确认它们
*   现在用 Azure ML 数据集导出数据集

![](img/2cc42376c08c294f52cb4cd2e6dea2e0.png)

# 使用用户界面的自动机器学习视觉

*   现在转到左侧导航窗格中的自动化 ML
*   创建一个新的自动化 ML 项目
*   选择我们从数据标签项目中导出的 coco 数据集

![](img/8139778053241f80c8ed809ef2517529.png)

*   然后单击下一步
*   创建一个新的实验，如下图
*   选择目标标签的标签列表列
*   为计算选项选择计算群集
*   只有选择基于 GPU 的计算选项可用
*   在我的例子中，我使用了实例分段

![](img/d90701b0ff97dd77e23fceaf1ead22d5.png)

*   选择算法的下一个选项

![](img/e9544bf7c6bf3465895fd24969ed0882.png)

*   为算法提供超参数

![](img/911b69cd84e4f22e1e18339d0a917f15.png)

*   现在可以选择添加更多的算法

![](img/2b57417a9008fe8a56bb1d2498a2e18a.png)

*   提供采样、迭代、提前停止和并发迭代
*   并发迭代允许算法在多个计算节点上运行
*   现在点击下一步
*   如果有，请提供验证数据集
*   在我的示例中，我没有，因此我将跳过这一步，单击“Finish”

![](img/33556d9c4f6d64b77de8eb0adae69775.png)

*   现在点击完成
*   上述过程将创造一个新的工作/实验。
*   现在转到“Jobs ”,单击您刚刚创建的作业
*   您应该看到作业正在运行。

![](img/60503244b5eb4219a78cf1044ce69f0f.png)

*   现在等待 10 分钟，等待作业完成
*   一旦作业完成，检查输出

![](img/345325e083a6460187d278323de6880d.png)

*   工作产出

![](img/70cd8e077e2667605d6c4e4ac220d206.png)

*   以下是这项工作的衡量标准

![](img/a6a2b455d70a372a3a27ff8395a398e1.png)

*   模型输出

![](img/f11f07445b2331c22111ce43b481fd87.png)

*   模型输出

![](img/1588082d224ae681c62477ce6a4c763f.png)

*   比较不同算法的指标
*   选择要比较的作业运行

![](img/fdf57c9b124bc9e16e43ae8b02cdb2b2.png)

*   单击比较预览

![](img/8cc4bcad00e4e4a1aa8652d2eff715c3.png)

原始文章位于[samples 2022/automlvisionui . MD 位于主 balakreshnan/samples 2022(github.com)](https://github.com/balakreshnan/Samples2022/blob/main/AzureML/automlvisionui.md)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)