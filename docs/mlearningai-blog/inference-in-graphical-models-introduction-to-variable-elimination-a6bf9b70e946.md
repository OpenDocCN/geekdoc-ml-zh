# 图形模型中的推理——变量消去法简介

> 原文：<https://medium.com/mlearning-ai/inference-in-graphical-models-introduction-to-variable-elimination-a6bf9b70e946?source=collection_archive---------2----------------------->

在之前的[文章](/p/ae3f6bc1d7f6)中，我们谈到了图形模型上的推理。我们特别考虑了*精确*和*近似推理*的复杂性。事实证明，这两种情况都给我们带来了问题。因此，在本文中，我们将考虑*变量消除*的概念。我们将试图理解它背后的基本思想，然后看看它的算法。

## 了解变量消除