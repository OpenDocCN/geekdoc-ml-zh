# 揭开 sklearn 困惑的神秘面纱 _ 矩阵和分类 _ 报告

> 原文：<https://medium.com/mlearning-ai/taking-the-mystery-out-of-sklearns-confusion-matrix-and-classification-report-2cc73dfebaa6?source=collection_archive---------3----------------------->

![](img/dc018a480f504cef0fb11c443cff181a.png)

在对数据进行预测时，重要的是评估预测中涉及的指标，以此作为努力纠正尽可能多的错误的出发点。一个非常直接的评估标准是 sklearn 的混淆矩阵。下图显示了二进制混淆矩阵的工作原理。理想情况下，所有的积极因素都是真的，而且…