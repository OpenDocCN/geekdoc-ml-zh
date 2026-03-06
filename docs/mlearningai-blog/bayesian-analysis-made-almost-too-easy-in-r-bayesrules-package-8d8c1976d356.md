# BayesRules 包

> 原文：<https://medium.com/mlearning-ai/bayesian-analysis-made-almost-too-easy-in-r-bayesrules-package-8d8c1976d356?source=collection_archive---------1----------------------->

## 贝叶斯分析在 R 中变得几乎太容易了

在我之前的文章中，我强调了贝叶斯分析如何被用来表明科学的脆弱，贝叶斯定理如何被用于条件概率计算，以及你如何使用 R 中的[反思包来以贝叶斯方式估计参数。](/@marc.jacobs012/statistical-rethinking-bayesian-analysis-in-r-e1e25aeb9a5c)

今天，我将向大家展示如何使用 [BayesRules 包](https://www.bayesrulesbook.com/)来估算一款[混动车型](/@marc.jacobs012/introduction-to-mixed-models-in-r-9c017fd83a63)。这篇文章将会很短，因为一个简单的例子将会向你展示你需要进一步扩展的所有内容。BayesRules 包使得贝叶斯分析几乎太容易了，这也是危险所在，这篇文章不是关于贝叶斯的内部工作。它是关于使用你习惯的建模类型，将贝叶斯方法添加到你的 Frequentist 工具包中。

让我们开始吧。

![](img/6dc1a618fed1affd9965ed2ef5979ff8.png)

Loading all the libraries. Not all are necessary, but many come in handy.

![](img/840a1bf5d285509899419b9062ac77d5.png)

Relationship of interest — Average Daily Feed predicting Average Daily Gain across numerous studies. The relationship is not shocking and well known. The example is used to showcase the Bayesian analysis.

![](img/3152a629a753fdc103e4375f5a747818.png)

This is much more interesting especially since many densities do not have a single peak. They look like mixtures.

Lets run a mixed model using the lme4 package first.

![](img/a4e9fa8a6de8083d14f1f83e18fe91ac.png)![](img/c376923f9673fc6aa9173288fe2fd80e.png)

ADG as a function of ADF, whether the study was included in 2017 and the random effect of Studycode. Meaning that I expect intercepts to vary according to Studycode. The Pearson Residuals are looking good.

现在是时候介绍 BayesRules 包了。[编码非常简单](http://mc-stan.org/rstanarm/articles/priors.html)，借鉴了 glmer 和 lmer 编码方式，以及 [rstanarm 包](http://mc-stan.org/rstanarm/index.html)。

您可以指定:

1.  要包含的 ***公式*** 类似于 lmer 和 glmer，您可以指定要包含的变量、要提取的数据以及要使用的系列。
2.  **prior _ intercept***→*intercept prior 将其他变量居中后。自动缩放意味着用户指定的先前缩放可以基于预测器的缩放进行内部调整。
3.  **先验** →回归系数
4.  **prior _ aux***→prior 为误差项*
5.  *[**先验 _ 协方差**](http://mc-stan.org/rstanarm/articles/glmer.html) →多级项(即变化截距和斜率)的先验*
6.  ***链条** *→* 您想要运行多少个 MCMC 链条*

*![](img/9e95776322cb824d4cb08d50c36db14e.png)*

*The estimates from the model.*

*![](img/fbb682a1b099845514ad387fb75b037f.png)*

*MCMC diagnostics — looking good.*

*![](img/e37546a4ce7547645dc75df2e66edacc.png)*

*Fixed effects.*

*![](img/c4ba350869ee30e2f1ae21484ea4e04e.png)*

*Random effects.*

*![](img/af1366ecea2215ff1937d9185a1ef5da.png)*

*Plot of the fixed, random, and auxiliary effects.*

*![](img/73412d6e0dbb9251bcc417e11d3cdc46.png)*

*The distribution of the random effects. Almost all pas the zero line, and estimates are rather large. Nevertheless, some are on the left side of the zero line, others on the right, which builds my confidence that there is enough variation. The variance estimate of Studycode in the plot above this one also verifies that believe. These plots are very good to plot as they reveal the workings of the random effect, and highlight some form of clustering.*

*Code to do model assessment, looking at the convergence of the MCMC chains, the effective number of samples needed, the autocorrelation function, and the posterior probability checks.*

*![](img/ee113a2eb56f756828d3faefc9a73e22.png)**![](img/c8088c610577a048169826c05863d89f.png)**![](img/c12fdb6c750a06732c6e6e42ff410eae.png)*

*I cannot say I am too happy with this ploy, since my posterior probabilities do not cover the dual peak of the data. That is something to take into account anyhow.*

*I am using the following codes to take a deeper look at what the model is producing.*

*![](img/b6eb5c33328c4d4f0aa454ef9a468f92.png)**![](img/30d65d972518147d0aedf832918c3455.png)*

*Overal estimates and estimates coming from the posterior for two studycodes. The model for sure has the correct direction, and is able to take into account the differencing intercepts.*

*Asking for posterior prediction, a prediction summary, a cross-validated prediction summary, and a [leave-one-out estimator for the WAIC](http://mc-stan.org/loo/articles/loo2-example.html).*

*![](img/2924c41154935a1ab8dfd49427455015.png)*

*Looking good.*

*![](img/3fa1ce5215efa5932084b23a3c4a4136.png)*

*Looking good as well.*

*![](img/030541a474a00d5ca28220791c190eb3.png)*

*Cross-validation shows a sharp increase of about 400% indicating, to me, that my model is not where it should be.*

*![](img/9eb0730b3a4e761a7948d308991cb5e3.png)**![](img/2c9dbb6ce613eb5ff826fdb467ccafe7.png)**![](img/ce7a94d0497c5a05129ba55e04b53128.png)**![](img/6b75ab251480320f2915585a9fcf7614.png)*

*Clearly shows that the model is not what it should be, especially on the tails.*

*![](img/3d08fd8a0619277ca46b8ddc6d348b6b.png)**![](img/d5748bbf5518dbb4972a7556859f54b9.png)**![](img/7bb42a63b1299facebb8f9b36e1a5d22.png)**![](img/113b55ec0d19310235dfd4245676d388.png)*

*Plots showing the posterior distribution, posterior predictions per study group per sample, and the estimated posterior of a new dataset.*

*接下来是一个更完整的模型，包括许多额外的预测。我还会将这个新模型与以前的模型进行比较。*

*![](img/abccb9196baa6e2b3bc5d800829c3884.png)*

*Model 3 added a lot of predictors by no predictive power.*

*![](img/d3c0fc3ef7d20d123b1afd3342118d8c.png)**![](img/2462b6a4243035a8b31b2f01cf580263.png)*

*Posterior predictions per Studygroup. Looks good.*

*现在，一个包括随机截距和随机斜率的模型。*

*![](img/414700864bf67669a360269d76b73604.png)**![](img/5559967b3de2a08d6cb261b2f88ba60b.png)*

*The newest model is better, but not much better.*

*![](img/fa102b8349a946bf6f3631d62e5ef26d.png)**![](img/8ca20d30be17f983ebed1b438af57cc4.png)**![](img/d5496f13898a0ad3f05c3244e2e2891f.png)*

*Chains look good, posterior predictions for random effects as well, but their overlap for the random slope is so much that I do not see any additional benefit to including them.*

*![](img/bd1a369fe18fd5ba56e43e4bd1ff5175.png)**![](img/35ea4513258417f7ff0bb571e71f5c6d.png)*

*Random intercept is about zero and the posterior predicted distributions still do not match the dual peak of the observed distribution. Something that should be included in the model still is not included.*

*上述结果表明，要么我遗漏了一个感兴趣的变量，要么我应该使用一种混合物来模拟数据。这将是以后文章的主题。*

*目前，高斯贝叶斯建模的快速介绍使用了 BayesRules 包。*

*尽情享受吧！*

*[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)*