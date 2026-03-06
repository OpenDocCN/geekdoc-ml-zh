# 漂浮在开源上

> 原文：<https://medium.com/mlearning-ai/floating-on-open-source-bd6ff3817afb?source=collection_archive---------6----------------------->

许多机器学习业务实际上只是一个漂浮在一堆开源项目上的技术团队，结合了一个庞大而昂贵的数据集。原始技术工作的数量很少(应该如此),主要涉及将几个不同的 API 组合在一起，并确保它们能够很好地运行。

*   模型服务—(开源，如 TorchServe、FastAPI、Bento 等。)
*   模型培训— (Pytorch-Lightning、TensorFlow、Composer 等。)
*   实验跟踪— (MLFlow、TensorBoard 等。)

> 开源项目来来去去，但是你必须乘风破浪。

这一现实应该被充分接受。您的核心责任是选择正确的工具，知道您可以负担得起在每个工具上投入多少，并知道何时继续前进。

![](img/3af2b3574f35427b860f5fdeb74f83a7.png)

Riding on Open-Source

这确实会带来一些技术上的后果。某些设计模式倾向于快速改变底层实现的能力。不过，这些技术的核心实际上只是为了避免编写太多特定于 API 选择的代码，这些代码可能不会持久。Laszlo Sragner 已经就这个话题写了很多文章。

[在一个数据科学项目(substack.com)中，你只需要 2 个设计模式来提高你的代码质量](https://laszlo.substack.com/p/you-only-need-2-design-patterns-to)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)