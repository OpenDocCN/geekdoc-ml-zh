# 机器学习中的准确性悖论

> 原文：<https://medium.com/mlearning-ai/stop-using-accuracy-to-assess-your-ml-models-73d4fff55beb?source=collection_archive---------0----------------------->

## 以及为什么正确的 ML 模型评估的含义将重新定义我们对 ML 中“学习”的理解。

*这篇文章收集了最近在*[*neur IPS/WHMD*](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjQrauku5z1AhXKhP0HHY_9DtMQFnoECAcQAQ&url=https%3A%2F%2Farxiv.org%2Fabs%2F2112.06775&usg=AOvVaw2L6RU4g5Inzc_HSiSPyCg-)*和*[*AAAI hcomp*](https://www.humancomputation.com/assets/blue_sky/HCOMP_2021_paper_102.pdf)*和这组* [*研究*](https://arxiv.org/abs/2209.15157) *的论文中的概念。*

绝大多数 ML 研究和开发工作都集中在改进某种形式的“正确性”指标的算法上——从准确度、精确度和召回率到 F1、AUC、Bleu 分数等等。排行榜也是如此，经常被用来…