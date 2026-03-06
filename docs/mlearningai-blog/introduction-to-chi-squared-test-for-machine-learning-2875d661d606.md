# 机器学习的卡方检验介绍

> 原文：<https://medium.com/mlearning-ai/introduction-to-chi-squared-test-for-machine-learning-2875d661d606?source=collection_archive---------0----------------------->

![](img/3a1dfa41be01001d33f03ee5abd23d4d.png)

Photo by [Clay Banks](https://unsplash.com/@claybanks?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

应用机器学习中的一个普遍问题是发现输入特征对于要预测的结果是否重要。
**这就是特征选择的问题。**

在输入变量也是分类变量的分类问题中，我们将使用统计检验来确定输出变量是依赖于输入变量还是独立于输入变量。如果是独立的，则输入变量是可能不适合问题并从数据集中移除的特征的候选。

皮尔逊的卡方统计假设是检验分类变量之间独立性的一个例子。

## 相依表

一个**列联表**，有时也称为双向频率表**是一个至少有两行和两列的表格机制，在**统计**中使用，以频率计数的形式呈现分类数据。**

例如，带有人为计数的 Sex=rows 和 Interest=columns 表可能如下所示:

```
 Science, Math, Art
Male     20,    30,  15
Female   20,    15,  30
```

卡方分布的自由度基于列联表的大小计算如下:

**自由度:**(行— 1) *(列— 1)

就 p 值和选定的显著性水平(α)而言，测试可解释如下:

If **p 值** **< =** alpha:显著结果，拒绝零假设(H0)，相依。

如果 **p 值****>**α:不显著结果，未能拒绝零假设(H0)，独立。

Pearson 的独立性卡方检验可以在 Python 中使用 chi2_contingency() SciPy 函数来计算。

该函数将一个数组作为输入，表示两个分类变量的列联表。它返回计算的统计数据和 **p 值**用于解释，以及计算的**自由度**和预期频率表。

```
stat, p, dof, expected = chi2_contingency(table)
```

我们可以通过从自由度的概率和数量的卡方分布中检索临界值来解释统计数据。

**例如，**
可以使用 95%的概率，表明假设测试的变量是独立的，那么测试的发现是很有可能的。如果统计量小于或等于临界值，我们可以不拒绝这个假设，否则，可以拒绝。

```
# interpret test-statistic
prob = 0.95
critical = chi2.ppf(prob, dof)
if abs(stat) >= critical:
print('Dependent (reject H0)')
else:
print('Independent (fail to reject H0)')
```

我们还可以通过将 p 值与选定的显著性水平进行比较来解释 p 值，显著性水平为 5%，通过反转临界值解释中使用的 95%概率来计算。

```
# interpret p-value
alpha = 1.0 - prob
if p <= alpha:
print('Dependent (reject H0)')
else:
print('Independent (fail to reject H0)')
```

我们可以将所有这些联系在一起，并使用一个设计好的列联表来演示卡方显著性检验。下面定义了一个列联表，它对每个群体(行)有不同数量的观察值，但对每个组(列)有相似的比例。给定相似的比例，我们期望测试发现组是相似的，并且变量是独立的(不能拒绝零假设或 H0)。

```
table = [ [10, 20, 30],
           [6, 9, 17]]
```

**下面列出了完整的示例。**

```
# chi-squared test with similar proportions
from scipy.stats import chi2_contingency
from scipy.stats import chi2
# contingency table
table = [ [10, 20, 30],
[6, 9, 17]]
print(table)
stat, p, dof, expected = chi2_contingency(table)
print('dof=%d' % dof)
print(expected)# interpret test-statistic
prob = 0.95
critical = chi2.ppf(prob, dof)
print('probability=%.3f, critical=%.3f, stat=%.3f' % (prob, critical, stat))
if abs(stat) >= critical:
print('Dependent (reject H0)')
else:
print('Independent (fail to reject H0)')# interpret p-value
alpha = 1.0 - prob
print('significance=%.3f, p=%.3f' % (alpha, p))
if p <= alpha:
print('Dependent (reject H0)')
else:
print('Independent (fail to reject H0)')
```

运行该示例首先打印列联表。计算测试，自由度(dof)报告为 2，
接下来，打印计算出的期望频率表，我们可以看到，通过肉眼检查数字，观察到的列联表确实匹配。
计算并解释临界值，发现变量确实是独立的(未能拒绝 H0)。对 p 值的解释得出了同样的结论。

```
[[10, 20, 30], [6, 9, 17]]
dof=2
[[10.43478261 18.91304348 30.65217391]
[ 5.56521739 10.08695652 16.34782609]]
probability=0.950, critical=5.991, stat=0.272
Independent (fail to reject H0)
significance=0.050, p=0.873
Independent (fail to reject H0)
```

感谢 Regex 软件提供冬季实习。

希望你喜欢这个帖子，一定要为这个帖子鼓掌。