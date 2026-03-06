# 产前筛查试验的评价——R

> 原文：<https://medium.com/mlearning-ai/evaluation-of-a-prenatal-screening-test-simulation-in-r-239844058853?source=collection_archive---------3----------------------->

产前筛查的目的是在遗传疾病的任何迹象或症状出现之前识别该疾病。焦点是每个人的 23 对染色体，因为它们包含了你所有的 DNA。这些测试是专门设计来检测偏离常态的。

三种最常见的疾病是唐氏症、爱德华氏症和帕托氏症。分别在 21 号、18 号和 13 号染色体上发现了异常。

![](img/ad7840301c5a0935714a5c3c74559b4b.png)

评估筛选测试的可用性意味着你必须处理测试预测尚未发生的事情的能力。因此，每个测试都提供一个概率作为结果。因此，要评估一项测试，你需要设计和实验，并将测试结果与“黄金标准”结合起来。在这种情况下，金标准就是身体病史&检测染色体异常的 DNA 测试。

有了这样的数据集，我们可以创建一个 2*2 的表，告诉您测试预测阳性/阴性的频率以及黄金标准检测阳性/阴性的频率。

从 2*2 表格中，我们可以得出[四个最常见的指标，通过这些指标来评估测试的预测能力](https://online.stat.psu.edu/stat507/lesson/10/10.3):

1.  **敏感度**，这是一项测试将在患病者中显示“疾病”的概率。
2.  **特异性**，即未患病者中检测结果为阴性者的比例。
3.  **阳性预测值(PPV)** ，即如果检测呈阳性，某人患该病的概率。
4.  **阴性预测值(NPV)** ，即如果检测结果为阴性，某人没有患病的概率。

PPV 和 NPV 都取决于疾病在人群中的流行程度，因此遵循[贝叶斯定理](https://en.wikipedia.org/wiki/Bayes%27_theorem)，该定理基于可能与事件相关的条件的先验知识，规定了事件的概率。

下面您可以找到感兴趣的疾病的这些指标的值。

![](img/7b0e6058f9ecc88e8548835cd82b03a7.png)![](img/1d5cab4a3716bf4d7229d31af7d436f1.png)

The 2*2 matrix to evaluate a test by using a gold standard. Next to the table you see the calculations for estimating the four most common metrics of a test.

![](img/47928d78bfc5c82957aad4042bdb3290.png)![](img/4632af829547f08e299fdbd94f9aa199.png)![](img/98393b28db7c95c7f9f1b1bd895433dc.png)![](img/2e08a8decfb2c46b068cfa07f47a91d1.png)![](img/a6729b6dfb3cefed235a8fcc23ccb8c5.png)![](img/69d32bdbc30629d90d6dc687f983c451.png)![](img/380c7aed5507d44ea5ef3a19407ee1e8.png)![](img/d93f53e74d28fa50a051ff434ee60809.png)![](img/0b6b5ae8ffed45da823fcc6b0876edad.png)![](img/b36a31ace2a171f27f1eb2a87a16f72d.png)![](img/ac765752947b5cd9f077bdfe4307e317.png)

现在，让我们开始模拟产前检查的流行性、敏感性和特异性对 PPV 和 NPV 的影响。我将使用从文献中获得并在上面描述的值。

A function to determine the PPV can be found here.

![](img/3ea8f1805cd67eefcc7eb2eea9ca4627.png)

PPV is based on the prevalence, sensitivity, and specificity.

![](img/7ea0b72855df5fa0804bb777d56b42eb.png)![](img/bcebb9e91be650a8e70e6754c9caa7da.png)

PPV as a function of the prevalence

![](img/b043075913019bb18afebfc17241f9cd.png)![](img/df81f6d9b4572abce122588972731ae3.png)

Minor changes in the prevalence can have a big impact on the PPV given a certain sensitivity and specificity.

这里重要的是区分预测试和后测概率。预测试概率就是疾病在人群中的患病率。这是底线。后验概率是患病率(在贝叶斯定理中是先验的),而检验数据则是可能性。提供后验(检验后)概率。

![](img/e1dfa00613b7110117fbe2a4563d0a0b.png)

让我们看看 PPV 是如何随着患病率和特异性的变化而变化的。

![](img/14739cfdc2403a408494884a49d59bf7.png)![](img/3b7569b819de5ab638bb204afcb35aa2.png)

The relationship between prevalence, sensitivity, and specificity is clear.

现在是灵敏度的变化。

![](img/3d74d95b126f16126a87b9c47e456282.png)

Not so big as for the specificity, which has to do with how the metrics are build up.

让我们前进到似然比的领域。LR 代表一个因子，在给定阳性/阴性测试结果的情况下，通过该因子，您患/未患该疾病的概率增加/减少。这是测试的内在指标。

Code to measure the PLR and NLR.

```
[1] 0.0314379973 0.0001897793
```

![](img/84a11e18afbd028c4c38995d5ab9738e.png)

And the dataset with values based on the sensitivity and specificity.

![](img/d073a26188ea6388221d0bcf2d88a382.png)![](img/98a838564e2db5518686b0e0142cd889.png)

If a test is positive, your probability of having the disease is 18 times higher then your pre-test probability. *If a test is negative, your probability of having the disease is 10 times lower then your pre-test probability*

![](img/1df2990a9380937c2514720cd23c81e6.png)

Likelihood ratios for all two prenatal tests and three chromosomal deviations.

现在，如果你想估计后验概率，你需要有前验概率和似然比，计算如下:

*   预测优势=(预测概率/(1-预测概率)
*   测验后优势=测验前优势*可能性比率
*   事后检验概率=事后检验优势/(事后检验优势+ 1)

![](img/1df2990a9380937c2514720cd23c81e6.png)![](img/35a5346d327ac6b07474f366644cf356.png)![](img/3fbe1edc0cd469632707e79b8c508fad.png)

The baseline chance of a chromosomal abnormality is dependent on the age at pregnancy.

以及与更完美的测试(灵敏度和特异性均为 99%)相比，基于阴性和阳性测试来估计测试后概率的函数和代码。

![](img/ec4b27caf61eab6e7ee5a1afe1fed842.png)![](img/70a877fd4b6658a4220f18e43e553d30.png)

And the results, which show that the NIPT is a good as it gets

希望你喜欢！如果有什么不对劲，请告诉我！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)