# 对艾姆斯衣阿华州住房数据的深入研究。第三部分，共四部分。

> 原文：<https://medium.com/mlearning-ai/a-thorough-dive-into-the-ames-iowa-housing-dataset-part-3-of-4-2c00e0a0eacf?source=collection_archive---------3----------------------->

在本系列的第[第二](https://jesservillines.medium.com/a-thorough-dive-into-the-ames-iowa-housing-dataset-part-2-of-5-3e24ea276e1c)部分结束时，我们将一个经过清理和特征工程的数据框的副本保存为. csv 文件。这是我们想要用来构建回归模型的文件。如果您派生并克隆这个[库](https://github.com/jesservillines/Housing-Prices)，那么模型文件可以从 Github 导入。

现在真正的乐趣开始了，我们开始建造模型！

我们将比较几种类型的线性回归，看看它们如何与数据交互。普通最小二乘法(OLS)，OLS 与标准化，OLS…