# 构建多阶段推荐系统(第 2.1 部分)

> 原文：<https://medium.com/mlearning-ai/building-a-multi-stage-recommendation-system-part-2-1-b9cbece3f91b?source=collection_archive---------0----------------------->

重排序模型策略和设计—多门专家混合

在本系列的第一篇文章中，我们了解了采用多阶段推荐策略的重要性，尤其是对于拥有大量商品目录的公司。我们了解了什么是候选代，实现了它的一个著名代表:[双塔模型](/mlearning-ai/building-a-multi-stage-recommendation-system-part-1-2-ce006f0825d1)。在这篇文章中，我们将进入下一步，也就是更重要的一步。它的工作是，给定生成器发出的几百个候选项，对它们进行适当的排序(排序),以便…