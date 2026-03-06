# 变量消去法的复杂性和图形结构

> 原文：<https://medium.com/mlearning-ai/the-complexity-and-graph-structure-of-variable-elimination-618a79a2d005?source=collection_archive---------5----------------------->

在上一篇文章中，我们讨论了变量消去背后的算法。我们还看到了一个在实践中如何做到这一点的例子。在本文中，我们将尝试分析其复杂性。

## 图论分析

事实证明，变量消去算法的计算成本是由过程中产生的中间因子的大小决定的。该算法在因子中的变量数量呈指数增长，这意味着这些变量的大小…