# 使用 MLFoundry 跟踪 ML 模型

> 原文：<https://medium.com/mlearning-ai/tracking-ml-models-with-mlfoundry-5de8520752e6?source=collection_archive---------2----------------------->

我正处于数据科学的早期，我遇到了数据科学家必须遵循的最佳实践。其中一个有影响的实践是实验跟踪。

什么是实验跟踪？

*   顾名思义，这是一个简单的实践，我们跟踪所有的实验和它们的度量。由于机器学习模型管道是一种迭代方法，所以最好存储、跟踪和检索项目中所做的实验。
*   分类模型的实验跟踪的一个简单例子是记录所有的度量，如召回率、精确度、准确度和 F1 分数。

跟踪不应该仅仅局限于度量和模型统计。由于我们在即兴发挥模型的准确性，我们可能会改变训练数据，最好也记录数据。

![](img/cc57d1eb72c5c6efb8c8367e77c6e022.png)

Photo by [Mika Baumeister](https://unsplash.com/@mbaumi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我已经使用 MLFoundry 作为我的实验跟踪工具一个月了，有一些很棒的特性我们应该知道。让我们开始学习如何使用 MLFoundry。

MLFoundry 既可以用作 Python 包，也可以用作 CLI。我们将使用 Python 包。从 pip 安装软件包

```
pip install mlfoundry
```

你可以在这里创建一个账户[，并在这里](https://app.truefoundry.com/signin)从[获得你的 API 密匙。一旦一切都设置好了，你就可以从你的 Python 脚本或者 Jupyter 笔记本连接到 MLFoundry](https://app.truefoundry.com/secrets)

```
import mlfoundry
mlfoundry.login(relogin=False)
client = mlfoundry.get_client()
```

系统将提示您粘贴 API 密钥，粘贴后，您的用户会话将被保存，并且不再需要重新验证。

考虑一个分类问题，我们训练一个 RandomForestClassifier，使用 MLFoundry，我们可以记录模型及其参数以及指标，还可以读取记录的指标和模型，使其成为机器学习操作的完美版本控制系统。

ML 指标可以通过几行 Python 代码来记录。

```
run.log_model(model=xgb, framework="xgboost")run.log_dataset(features=X_train, actuals=y_train, dataset_name="train")
run.log_dataset(features=X_test, actuals=y_test, dataset_name="test")run.log_metrics(metric_dict={"accuracy": round(metrics.accuracy_score(y_test, y_pred_test),2),
                            "precision": round(metrics.precision_score(y_test, y_pred_test),2),
                             "recall": round(metrics.recall_score(y_test, y_pred_test),2),
                            "f1-score": round(metrics.f1_score(y_test, y_pred_test),2)})
```

让我们看一下仪表板，上面有上传的指标和数据。

MLFoundry Run Dashboard

完整的笔记本请看我的 [Kaggle 笔记本](https://www.kaggle.com/code/vishank97/tracking-ml-models-with-mlfoundry)，我在里面记录了泰坦尼克号比赛的模型。

使用此工具的最佳方式是当您正在处理在数据和模型训练方面有所改进的项目时

我觉得这个工具的理想用法是当我们遵循数据科学生命周期的迭代方法时，我们不断地改变数据，即兴进行模型的训练，并计算什么模型与什么训练数据工作得好。

如果你学到了新的东西，一定要把这篇文章分享给可能需要它的人。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)