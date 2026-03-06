# Python 中非正态数据的显著性检验和置信区间

> 原文：<https://medium.com/mlearning-ai/significance-testing-and-confidence-intervals-in-python-with-non-normal-data-e169d7302cc?source=collection_archive---------4----------------------->

![](img/ef6c984501e2750e75bb32b93a6cdc07.png)

Photo by [Tim Johnson](https://unsplash.com/@mangofantasy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为我职责的一部分，我必须定期对我们一直在进行的 A/B 测试的结果进行显著性测试。现在，这些结果不是正态分布的，如果有的话，它们是向轴倾斜的，有一个长尾巴。因此，使用 t 检验是不可能的。

**曼恩-惠特尼 U 检验** 在这种情况下，我求助于曼恩-惠特尼 U 检验
下面是来自[的一个优雅的定义 https://www . stats tutor . AC . uk/resources/uploaded/mannwhitney . pdf](https://www.statstutor.ac.uk/resources/uploaded/mannwhitney.pdf)

> Mann-Whitney U 检验是一种非参数检验，可用于替代不成对 t 检验。它用于检验两个样本来自同一总体(即具有相同的中位数)的零假设，或者检验一个样本中的观察值是否倾向于大于另一个样本中的观察值。

**我们如何在 Python 中使用它？下面是我用 Python 做显著性测试的脚本。我有一个示例数据集的链接，您可以从 github 访问它**

```
import pandas as pd
import numpy as np
import scipy.stats as statsdf=pd.read_csv(‘[https://raw.githubusercontent.com/bilalmussa/st_sig_testing/main/test_control_data.csv'](https://raw.githubusercontent.com/bilalmussa/st_sig_testing/main/test_control_data.csv'))group1 = df[df[‘AnalysisFlag’]==’Test’][‘DP’].to_list()
group2 = df[df[‘AnalysisFlag’]==’Control’][‘DP’].to_list()s, p = stats.mannwhitneyu(group1, group2, alternative=’two-sided’)print(“Statistc: “,s)
print(“P Value: “,p)
#Statistc: 6966579.5
#P Value: 0.6559138409860421
#In this case, there is no significance at any conventional level
```

就是这样，这将告诉我们，测试和控制之间是否有显著差异。

**置信区间** 下一步是使用自举创建置信区间。本质上，我们要从我们的数据集中采样 N 次。在此过程中，它将基于我们的重复次数(我们想要重新采样的次数)和 alpha 值(总体参数位于置信区间之外的可能性)来构建置信区间

```
def bootstrap_ci(df, variable, classes, repetitions = 1000, alpha = 0.05, random_state=None): 
 df = df[[variable, classes]]
 bootstrap_sample_size = len(df) 

 mean_diffs = []
 for i in range(repetitions):
 bootstrap_sample = df.sample(n = bootstrap_sample_size, replace = True, random_state = random_state)
 mean_diff = bootstrap_sample.groupby(classes).mean().iloc[1,0] — bootstrap_sample.groupby(classes).mean().iloc[0,0]
 mean_diffs.append(mean_diff)
 # confidence interval
 left = np.percentile(mean_diffs, alpha/2*100)
 right = np.percentile(mean_diffs, 100-alpha/2*100)
 # point estimate
 point_est = df.groupby(classes).mean().iloc[1,0] — df.groupby(classes).mean().iloc[0,0]
 print(‘Point estimate of difference between means:’, round(point_est,2))
 print((1-alpha)*100,’%’,’confidence interval for the difference between means:’, (round(left,2), round(right,2)),’ this is base on ‘,repetitions, ‘ repetitions’)

#run the function using the below 
bootstrap_ci(df,’DP’,’AnalysisFlag’,100,0.1)
#Point estimate of difference between means: 0.01
#90.0 % confidence interval for the difference between means: (-0.0, #0.03) this is base on 100 repetitions
```

仅此而已。请随意留下您的任何反馈，祝显著性测试愉快！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)