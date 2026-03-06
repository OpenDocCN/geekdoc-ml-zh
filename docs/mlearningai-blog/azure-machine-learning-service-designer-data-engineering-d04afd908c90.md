# Azure 机器学习服务设计师—数据工程

> 原文：<https://medium.com/mlearning-ai/azure-machine-learning-service-designer-data-engineering-d04afd908c90?source=collection_archive---------9----------------------->

# 如何使用 designer 在 Azure 机器学习服务中进行数据工程

# 先决条件

*   Azure 帐户
*   Azure 存储
*   Azure 机器学习服务

# 介绍

*   本教程只是展示如何使用 designer 在 Azure 机器学习服务中进行数据工程。
*   使用的数据是泰坦尼克号数据集。这是机器学习中一个著名的数据集。
*   这里使用的是开源数据集。
*   每个任务或流项目都有参数和输出
*   运行后，每个任务的输出都可以可视化
*   输出将根据任务或流程项目而变化

# 总流量

![](img/dc27f415be8e7fb5b880bca5dc1f2ef9.png)

*   以上是整体实验
*   使用低代码环境构建
*   所有都是拖放

# 什么完成了

# 带上数据集

# 选择数据集中的列

![](img/d37b641cd053b6d6b35dda53025d0c6c.png)

# 执行 python 脚本-相关性图表

```
import seaborn as sn
    import matplotlib.pyplot as plt corrMatrix = dataframe1.corr()
    print (corrMatrix)
    sn.heatmap(corrMatrix, annot=True)
    plt.show()
    img_file = "corrchart1.png"
    plt.savefig(img_file) from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)
```

![](img/f2ef83245517bbd16db0324d23e36dc7.png)

*   输出

![](img/f8b979e2e586f2cc9c2e5ef89bda1fae.png)

# 执行 python 脚本-协方差图表

```
covMatrix = dataframe1.cov()
    print (covMatrix)
    sn.heatmap(covMatrix, annot=True)
    plt.show()
    img_file = "covchart1.png"
    plt.savefig(img_file) from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)
```

*   密码

![](img/67aec5c9d1f623931854069c94b52ed4.png)

*   输出

![](img/d87205b27b33153fafd3e3421f9b8465.png)

# 删除重复的行

![](img/5c744b1839a5735baf82a275d00e5999.png)

# 标准化数据

![](img/72ebe72408f34c98bb857fa1ccfb22b1.png)

# 对箱中的数据进行分组

![](img/717f4717c88547a54886a12239c7e49b.png)

# 编辑元数据以将字符串转换为分类列-名称

![](img/dc560c89c1995c5d673264ad43279654.png)

# 编辑元数据以将字符串转换为分类列 Cabin

![](img/9f379f8f1e843ef8a365d46cc1ca806c.png)

# 编辑元数据以将字符串转换为分类列-已装载

![](img/7689b11c67469cd9bda10c6fddc0290b.png)

# 裁剪值-避免过度拟合

![](img/f4c3b7aac6c47199d0f1c9347aca317c.png)

# 清除丢失的数据

![](img/ae5ceea22cc4cbe6c8932d56e87e8dea.png)

# 应用数学运算

![](img/13c22f0816055b940dfd364414769381.png)

# 将数据分为训练数据和测试数据

![](img/52fb47107c3ca026ae11bf64deaa36fa.png)

# 将模型带到培训中

![](img/74a94666f65eae5cd9977e47c41cfd91.png)

# 火车模型

![](img/8b35d1a4d62d1ce5c98011b4a7ed814c.png)

# 得分模型

![](img/23fd0ace076067cc5cc1957439d0c3a8.png)

*   输出

![](img/394347385a4cde1c5368367618734d09.png)

# 评估模型

*   输出

![](img/98e04a7f289d18d92fd95f2ca148e6a4.png)

*   受试者工作特征曲线

![](img/02800ea69bb1e46bf826657f635e469e.png)

*   混淆矩阵

![](img/67bdf3ad64de65ea38b539c39a019465.png)

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Samples2021/blob/main/AMLDesigner/designerdataengg.md)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)