# 构建多阶段推荐系统(第 1.2 部分)

> 原文：<https://medium.com/mlearning-ai/building-a-multi-stage-recommendation-system-part-1-2-ce006f0825d1?source=collection_archive---------1----------------------->

## 双塔模型的实现及其对 H&M 数据的应用

这篇博文是对第 1.1 部分的跟进，在第 1.1 部分，我们解释了两阶段推荐流程，并特别强调了候选人生成步骤。如果你还没有读过这篇文章，我鼓励你先读一读。我们已经深入描述了双塔模型，现在我们将在 TensorFlow 2 中实现它，并将其应用于 Kaggle 数据集。

# H&M·卡格尔竞赛