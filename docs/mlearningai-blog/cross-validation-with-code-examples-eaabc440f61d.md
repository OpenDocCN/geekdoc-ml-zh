# 使用代码示例进行交叉验证

> 原文：<https://medium.com/mlearning-ai/cross-validation-with-code-examples-eaabc440f61d?source=collection_archive---------3----------------------->

![](img/4d3b3bbbaec0b062022a32deb92d854b.png)

Photo by [Tai Bui](https://unsplash.com/es/@agforl24?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 总结:

1.  什么是交叉验证？
2.  为什么我们要使用交叉验证？
3.  交叉验证的常见类型有哪些？
4.  如何应用交叉验证(带代码)？
5.  交叉验证中的过度拟合和欠拟合
6.  我们应该注意什么？

## 什么是交叉验证？

交叉验证是一种用于评估机器学习模型性能的评估技术。它使用多个训练测试分割来评估单个模型，并返回多个准确度分数。该过程类似于重采样过程，使用原始数据集的不同子集来训练和评估同一模型，以便我们可以从一系列得分中获得一个平均准确度得分，以确定我们训练的模型是否是一个好的预测器。

## 为什么我们要使用交叉验证？

首先，交叉验证给了我们一个**更稳定可靠的模型估计**。估计的准确度分数将根据在训练和测试集中结束的数据样本而变化。因此，通过交叉验证，我们可以从多个训练测试拆分返回的一系列分数中获得模型的平均准确度分数，而不是依赖于单个特定的训练集来获得最终的准确度分数。

第二，交叉验证可以**显示模型敏感度**。有了交叉验证返回的一系列准确度分数，我们可以对模型性能做最坏情况或最好情况的设想。具体来说，我们可以绘制准确度分数的分布图，以查看我们的模型在新数据集上表现不佳或非常好的可能性。

## 交叉验证的常见类型有哪些？

**K 重交叉验证。**

以 K = 5 为例。将原始数据集随机分成大小相等的 5 份，并重复该过程 5 次。对于每一次，一个折叠作为测试集，另外四个折叠作为训练集训练模型得到相应的准确率得分。有了所有这些分数，我们可以获得一个平均交叉验证准确度分数。

![](img/a3158be00b0a118e5024cca2619f4a3b.png)

Image Source: SIADS542 by Kevyn Collins-Thompson

由于在模型训练和测试期间使用了所有数据样本，K-fold 交叉验证返回的模型偏差较小。

然而，如果原始数据样本是按照某种顺序或根据类别标签排序的，那么我们可能会以不平衡的训练子集和无代表性的测试集来结束对模型的训练。因此，得到的准确度分数将是不准确的。

**代码示例**

```
# import libs
from sklearn.datasets import load_breast_cancer
from sklearn.preprocessing import MinMaxScaler
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score# get cancer data
cancer = load_breast_cancer()
X_cancer = cancer['data']
y_cancer = cancer['target']# normalized the data
scaler = MinMaxScaler()
X_cancer_scaled = scaler.fit_transform(X_cancer)# apply classifier
clf = LogisticRegression()# get cv scores
cv_scores = cross_val_score(clf, X_cancer_scaled, y_cancer, cv = 5)print('Cross validation scores (5 folds): {}'.format(cv_scores))
print('The average cross validation score (5 folds): {}'.format(np.mean(cv_scores)))## final result ##
## Cross validation scores (5 folds): [0.95614035 0.96491228 0.97368421 0.95614035 0.96460177]
## The average cross validation score (5 folds): 0.9630957925787922
```

**分层 K 折交叉验证。**

为了解决潜在的不平衡问题，我们可以使用分层交叉验证。基本上，数据样本被重新排列，以确保每个子集中类的比例尽可能接近整个数据集中类的实际比例。

![](img/c0cd08d39e50d1bd191689902760d365.png)

Image Source: SIADS542 by Kevyn Collins-Thompson

这样，每个子集都很好地代表了整个数据集。

**代码示例**

```
# import libs
from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import StratifiedKFold# get iris data
iris = load_iris()
X_iris = iris['data']
y_iris = iris['target']# stratified 3-fold splits
skf = StratifiedKFold(n_splits=3)
skf.get_n_splits(X_iris, y_iris)   # 3 iterations# apply classifier
clf = LogisticRegression()# get stratified cv scores
cv_skf_scores = cross_val_score(clf, X_iris, y_iris, cv = skf)
print('Cross validation scores (3 folds): {}'.format(cv_skf_scores))
print('The average cross validation score (3 folds): {}'.format(np.mean(cv_skf_scores)))## final result ##
## Cross validation scores (3 folds): [0.98 0.96 0.98]
## The average cross validation score (3 folds): 0.9733333333333333
```

需要注意的是，这里我们只是为了演示。事实上，我们自己并不需要进行分层拆分。`cross_val_score`中的`cv`参数将自行识别输入估算器。如果`y`是二进制或多类，则自动使用`StratifiedKFold`。也就是说，我们可以直接用`cv = 3` 代替`cv = skf` 得到同样的结果。

**留一交叉验证。**

当 K 等于数据集中数据样本的总数(n)时，留一交叉验证是 K 重交叉验证。顾名思义，只留下一个数据样本作为测试集，其余所有数据样本作为训练集。迭代 K = n 次后，可以得到平均交叉验证准确率得分。

![](img/d99a1d14b69a80647962987497b69bcf.png)

Image Source: SIADS542 by Kevyn Collins-Thompson

如果我们使用自定义的 P 个样本而不是一个样本作为测试集，那么这种交叉验证称为**留 P-out 交叉验证**。

**代码示例**

```
# import libs
from sklearn.datasets import load_iris
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import LeaveOneOut
from sklearn.model_selection import LeavePOut# get iris data
iris = load_iris()
X_iris = iris['data']
y_iris = iris['target']# leave one out splits
loo = LeaveOneOut()
loo.get_n_splits(X_iris)   # 150 iterations# leave P out splits
lpo = LeavePOut(2)
lpo.get_n_splits(X_iris)   # 11175 iterations# apply classifier
clf = LogisticRegression()# get leave-one-out cv scores
cv_loo_scores = cross_val_score(clf, X_iris, y_iris, cv = loo)
print('Cross validation scores: {}'.format(cv_loo_scores))
print('The average cross validation score: {}'.format(np.mean(cv_loo_scores)))## leave-one-out result ##
Cross validation scores: [1\. ... 1\. 1.]
The average cross validation score : 0.9666666666666667# get leave-p-out cv scores
long time to run!
cv_lpo_scores = cross_val_score(clf, X_iris, y_iris, cv = lpo)
print('Cross validation scores: {}'.format(cv_lpo_scores))
print('The average cross validation score: {}'.format(np.mean(cv_lpo_scores)))## leave-p-out result ##
## Cross validation scores : [1\. 1\. 1\. ... 1\. 1\. 1.]
## The average cross validation score: 0.9652796420581655
```

注意，留一法和留 p 法都是详尽的交叉验证技术。当我们有一个小的数据集时，最好使用它们，否则，运行起来会非常昂贵。

## 绘制验证曲线，查看过度拟合和欠拟合

下面，我们使用`validation_curve()`获得 SVM 模型在*乳腺癌*数据集上的训练和交叉验证分数，我们之前使用该数据集来查看 SVM 模型欠拟合或过拟合的相应伽马值。

![](img/90c3db587688ecf68036c25fffbd56de.png)

Image by the author

我们可以看到，当伽玛值低于 10 的负 7 次方时，模型欠拟合，当伽玛值高于 10 的负 4 次方时，模型过拟合。一个好的 gamma 值应该介于两者之间，因为训练和交叉验证分数都很高(≥0.9)。

**代码示例**

```
*Reference:* [*Scikit-learn.org*](https://scikit-learn.org/stable/auto_examples/model_selection/plot_validation_curve.html#sphx-glr-auto-examples-model-selection-plot-validation-curve-py)from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import validation_curve
from sklearn.svm import SVC
import matplotlib.pyplot as plt# get cancer data
cancer = load_breast_cancer()
X_cancer = cancer['data']
y_cancer = cancer['target']# set gamma parameter values
param_range = np.logspace(-10, -2, 6)# get training and test scores
train_score, test_score = validation_curve(SVC(random_state=0), X_cancer, y_cancer,param_name = 'gamma',param_range = param_range, cv = 5)# get means and stds of training and test scores 
train_score_mean = np.mean(train_score, axis = 1)
test_score_mean = np.mean(test_score, axis = 1)
train_score_std = np.std(train_score, axis = 1)
test_score_std = np.std(test_score, axis = 1)# make validation curve plot
plt.figure(figsize = (8,6))plt.title("Validation Curve with SVM on Breast Cancer Dataset")
plt.xlabel(r"gamma $\gamma$")
plt.ylabel("Accuracy Score")
plt.ylim(0.0, 1.1)plt.semilogx(
    param_range, train_score_mean, label="Training score", color="blue", lw=2
)
plt.fill_between(
    param_range,
    train_score_mean - train_score_std,
    train_score_mean + train_score_std,
    alpha=0.2,
    color="blue",
    lw=2,
)
plt.semilogx(
    param_range, test_score_mean, label="Cross-validation score", color="green", lw=2
)plt.fill_between(
    param_range,
    test_score_mean - test_score_std,
    test_score_mean + test_score_std,
    alpha=0.2,
    color="green",
    lw=2,
)
plt.legend(loc="best")
plt.show()
```

## 我们应该注意什么？

交叉验证可能**计算量很大**，尤其是当我们有一个大数据集并设置了一个大折叠值时。这是因为算法不能并行计算折叠结果，所以需要 K 倍的时间来获得所有分数。

此外，交叉验证**用于模型评估，而非模型调整**。我们应该使用网格搜索，而不是使用交叉验证来调整模型参数。

*感谢阅读！如果你喜欢这篇文章，请为我鼓掌*👏更多内容请关注我！ ☀️🌸😺

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)