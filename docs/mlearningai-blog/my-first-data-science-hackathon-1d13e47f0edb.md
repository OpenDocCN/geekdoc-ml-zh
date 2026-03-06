# 我的第一次数据科学黑客马拉松

> 原文：<https://medium.com/mlearning-ai/my-first-data-science-hackathon-1d13e47f0edb?source=collection_archive---------6----------------------->

![](img/cd2a158b91e74b0d54afd89858a93273.png)

[How to set up a successful Hackathon — A manager’s guide | The Innovation Mode](https://www.theinnovationmode.com/the-innovation-blog/how-to-run-a-successful-corporate-hackathon)

2022 年 6 月 14 日星期二，我参加了 EMEA 数据科学黑客马拉松。
目标:预测可寻址电视数据的广告类型类别
我们从美国多个城市/地区收集了 20 万行设备级别(移动/平板/桌面)的数据，用于构建涵盖 20 种类型的模型(训练数据集),其中 4 万行用于测试和提交我们的答案。我们被告知需要预测的 10 个类别，测试集中的每个设备都可以在训练数据集中找到。模型性能将基于一个微型 F1 分数。选择微观 F1 评分的原因是因为我们在这种情况下的多个类别流派。

训练和测试数据集具有以下列:
Row_number，UserID64_sha1，SiteDomain，Datetime，OperatingSystem，Browser，PublisherID，DeviceUniqueID_sha1，DeviceID，CarrierID，DealID，DeviceType，PostalCode，Application，LeafName

正如您所看到的，有大量有用的和可预测的信息，所以让我们从我如何解决这个问题开始吧。

1.  快速计数和数据类型检查

```
train.nunique()train.dtypes
```

2.意识到我最感兴趣的变量是 datetime，因此将 datetime 字段转换为 pandas datetime 类型，因为它当前是一个对象

```
train[‘c_datetime’]=pd.to_datetime(train[‘Datetime’])
```

3.创建一组从 datetime 派生的变量，如星期几、工作日/周末(二进制变量)、小时、日部分(小时的聚合)和月

```
train[‘c_dow’] = train[‘c_datetime’].dt.dayofweekdef weekday(row):
 if row[‘c_dow’] > 4:
 return 1
 else:
 return 0train[‘c_weekend’]= train.apply(weekday, axis=1)train[‘c_hour’] = train[‘c_datetime’].dt.hourdef daypart(row):
 if row[‘c_hour’] > 20:
 return ‘20’
 elif row[‘c_hour’] > 15 and row[‘c_hour’] <20 :
 return ‘16’
 elif row[‘c_hour’] > 11 and row[‘c_hour’] <16 :
 return ‘12’
 elif row[‘c_hour’] > 7 and row[‘c_hour’] <12 :
 return ‘8’
 elif row[‘c_hour’] <7 :
 return ‘0’
 else:
 return 0train[‘c_daypart’]= train.apply(daypart, axis=1)train[‘c_month’] = train[‘c_datetime’].dt.month
```

4.保留需要为测试集预测的内容类别(20 个中的 10 个)。

```
rows_to_keep = [‘GR10’,’GR20',’GR23',’GR25',’GR30'
 ,’GR31',’GR32',’GR34',’GR5',’GR7']train = train[train.LeafName.isin(rows_to_keep) == True].copy()
```

就是这样。我的数据准备工作完成了。我想保持简单，因为我将选择一种基于树的方法来预测内容类别(也称为 leafname ),所以如果我对热编码进行任何操作，我可能会过度拟合数据

我选择的预测内容类别的方法是[额外的树](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.ExtraTreesClassifier.html)，因为这是一个分类问题，基于树的方法比基于概率的方法更合适。

1.  导入必要的库以创建模型并执行交叉验证

```
from sklearn.ensemble import ExtraTreesClassifier
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import RepeatedStratifiedKFold
from numpy import mean
from numpy import std
```

2.基于只浮动的列设置 X，y 矩阵(我删除了用户和设备散列，因为它看起来不相关，可能只是在模型中添加了噪声)。y 变量是数据集中的内容类别 aka leafname。

```
X_train =train[[‘OperatingSystem’
 , ‘Browser’, ‘PublisherID’,
 ‘DeviceID’, ‘CarrierID’, ‘DealID’, ‘DeviceType’, ‘PostalCode’,
 ‘c_dow’, ‘c_weekend’, ‘c_hour’,
 ‘c_daypart’, ‘c_month’]]
y_train = train.LeafName
```

3.创建模型并评估 kfold 交叉验证。我更喜欢重复次数为 3 的 10 次拆分。然而，我的机器没有足够的内存，所以 8 分裂是我能做的最好的。我还将评分方法设置为 f1_micro，因为这是测试的评判标准。

```
# Fit/predict
etc = ExtraTreesClassifier()
_ = etc.fit(X_train, y_train)# evaluate the model
cv = RepeatedStratifiedKFold(n_splits=8, n_repeats=3, random_state=1)
n_scores = cross_val_score(etc, X_train, y_train, scoring=’f1_micro’, cv=cv, n_jobs=-1, error_score=’raise’)
# report performance
print(‘f1 micro: %.3f (%.3f)’ % (mean(n_scores), std(n_scores)))
```

当我回顾我的分数时，在 0.80 和 0.84 之间，平均 0.83，对于一个我很少与之相处的模特来说，这是相当令人印象深刻的。我还使用组合和额外的树做了一些[数据挖掘，看看哪些变量提高了微观 f1 的分数。这些变量是“操作系统”、“邮政编码”、“c_weekend”和“PublisherID”。然而，当我添加其余变量时，只有这 4 个变量的额外树模型的平均微观 f1 分数为 0.80 比 0.83。](https://bilalmussa.medium.com/using-combinations-to-model-all-possible-models-in-python-with-pandas-statsmodels-969aee159fd0)

4.使用我训练的模型预测测试数据集的内容类别

```
y_pred = etc.predict(X_test)
```

5.创建提交文件

```
submission=pd.concat({‘Row_number’: test.Row_number,
 ‘LeafName’: pd.Series(y_pred)},axis=1)
submission.to_csv(‘bm_submission_file.csv’, index=False)
```

搞定了。这是我完成的第一次数据科学黑客马拉松。我花了大约 4-5 个小时完成，因为我的机器有点慢，我在挖掘数据，看看我能找到什么信息来获得最高的微 f1 分数。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)