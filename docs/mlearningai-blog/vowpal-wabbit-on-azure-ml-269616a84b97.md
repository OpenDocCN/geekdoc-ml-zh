# Azure ML 上的 Vowpal Wabbit

> 原文：<https://medium.com/mlearning-ai/vowpal-wabbit-on-azure-ml-269616a84b97?source=collection_archive---------2----------------------->

![](img/b7f96941c18a8f69cbbabd44024cac0c.png)

# 在 Azure ML 上运行 Vowpal Wabbit 的示例

# 先决条件

*   Azure 帐户
*   Azure 机器学习服务

# 步伐

*   用 python 3.8 创建一个以 Azure ML 为内核的笔记本
*   安装 vowpal wabbit

```
pip install vowpalwabbit
```

*   加载必要的库

```
import pandas as pd
import sklearn
import sklearn.model_selection
import sklearn.datasets
import vowpalwabbit
```

*   现在将虹膜数据集加载到数据帧中

```
iris_dataset = sklearn.datasets.load_iris()
iris_dataframe = pd.DataFrame(
    data=iris_dataset.data, columns=iris_dataset.feature_names
)
# vw expects labels starting from 1
iris_dataframe["y"] = iris_dataset.target + 1
training_data, testing_data = sklearn.model_selection.train_test_split(
    iris_dataframe, test_size=0.2
)
```

*   格式特征函数

```
def to_vw_format(row):
    res = f"{int(row.y)} |"
    for idx, value in row.drop(["y"]).iteritems():
        feature_name = idx.replace(" ", "_").replace("(", "").replace(")", "")
        res += f" {feature_name}:{value}"
    return res
```

*   显示几行

```
for ex in training_data.head(10).apply(to_vw_format, axis=1):
    print(ex)
```

*   创建足球工作区
*   使用多个样本进行训练

```
vw = vowpalwabbit.Workspace("--oaa 3 --quiet")# learn from training set with multiple passes
for example in training_data.apply(to_vw_format, axis=1):
    vw.learn(example)# predict from the testing set
predictions = []
for example in testing_data.apply(to_vw_format, axis=1):
    predicted_class = vw.predict(example)
    predictions.append(predicted_class)
```

*   现在预测输出

```
accuracy = len(testing_data[testing_data.y == predictions]) / len(testing_data)print(f"Model accuracy {accuracy}")
```

*   输出应该是

```
Model accuracy 0.7333333333333333
```

*最初发表于*[*【https://github.com】*](https://github.com/balakreshnan/Samples2022/blob/main/AzureML/Vowpal1.md)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)