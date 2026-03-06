# 确定租金

> 原文：<https://medium.com/mlearning-ai/determining-the-rent-d1431c90ca9f?source=collection_archive---------6----------------------->

## 使用样条的贝叶斯方法

我喜欢[样条](https://blog.devgenius.io/dose-response-response-surface-designs-and-splines-using-sas-42535db1302e)并且我真的喜欢[贝叶斯](/mlearning-ai/mixed-models-of-chicks-weight-a41413c99b51)分析，所以在几篇关于[样条](/mlearning-ai/analysis-of-bodyweight-in-grower-finisher-pigs-using-mixed-models-splines-and-bootstrapping-in-r-4a997a04f77d)和[贝叶斯](https://pub.towardsai.net/good-old-iris-93358b8ecdbd)分析的文章之后，是时候展示如何使用这两者了。我要做的是给你一个例子，使用 rent99 数据集，然后将所有代码放在最后。你不需要读这篇文章，如果你喜欢，你可以用代码浏览它。

首先，让我们来看看 [rent99](https://www.rdocumentation.org/packages/gamlss.data/versions/6.0-2/topics/rent99) 。这是一个数据集，显示你为一个地方支付的租金，其中包括几个可能的预测变量。数据集没什么特别的，但它很适合显示样条曲线，尤其是张量。

那么，让我们开始吧。

![](img/5ac296554634e6c6650af7d9449fccec.png)

First six rows of the dataset.

![](img/861358277653e5c099d1de6b7b4d35cb.png)

**Skimr** is a great package for doing an initial assessment of the dataset — how many rows, columns, data types and the data itself.

![](img/2d45a100dae3c5c67a837e58f45409d2.png)

The results coming from **skim** in **skimr**.

![](img/beb91d87b07acfa6318662373438d876.png)![](img/98117cf05f67f8f3641e6d7386dc0310.png)![](img/a0f3ce9ca815bea8eb107e9a3bc576fb.png)

Nothing beats plotting the data, looking at various combinations to see which factors could possible have some form of relationship with **rent**, the variable of interest.

![](img/62f070bc835562a9870b9471d5632047.png)![](img/eab8cc337d35d2c3c861228df370a560.png)

Surface plots to get an idea of the viability of splines / tensors.

现在来看第一个模型，它是一个包含张量积的[一般加法模型](/mlearning-ai/predicting-the-feed-consumption-of-a-swine-farm-81ce4f703f3a)。

![](img/4739bfc7770942684528ec6d2fd1289e.png)![](img/c39e2610608749468e0a8f2c6b2cea47.png)

Simple model in which I, via cross-validation, let the algorithm find the maximum likelihood connecting the **area** of the house and the **year** to the **rent per square meter**.

![](img/4363397aaab4a5b5a021980cdc5e08ca.png)

The tensor product visualized. It sue is worthwhile to include one.

![](img/880a2bf4ded2275d4a0144c104210cd5.png)![](img/501ea6dbecc248195bba911dce321beb.png)

And the predicted values, coming from the model. To the left, the relationship between area and year, and to the right the same but then you can see the lower and upper confidence limits. The same could already be seen on the plot before, but then overlaid (red, green, black lines).

任何可以使用标准最大似然法或频率主义建模法建模的事物，也可以使用贝叶斯定理建模。使用 **brms** 包，我可以获得包使用数据找到的先前值，但是我不想这样做。我想包括信息丰富的前科，所以我做了。在这里，一个信息丰富的先验是一个表明方向的先验，并且不害怕对它有一种相当的确信感。

与上面的通用附加模型相比，这是一个使用 [gamm4](https://cran.r-project.org/web/packages/gamm4/gamm4.pdf) 包的通用附加[混合模型](/mlearning-ai/introduction-to-mixed-models-in-r-9c017fd83a63)。这意味着我们在残差旁边增加了一个额外的随机分量。当然，我们也需要在此之前分配一个优先级。

![](img/08671c03dd9640f57e866d6ef8cf3315.png)

As you can see, I need to include several priors. I need to do that for the knots of the tensor product, the **intercept**, **district**, **sigma**, and the **sigma of the tensor product**. The more priors you include, the more Bayesian the analysis become and the more interesting the knowledge transfer.

![](img/1fcc965fe18146eda549b9ff3b45cc70.png)

The Bayesian model created.

![](img/4974f199ab33a00be0412bb034da7ab9.png)

The summary coming from the model. As you can see, you cannot use **te** in the Bayesian model, but just [**t2**.](https://rdrr.io/cran/mgcv/man/t2.html) The difference is not that big.

![](img/4de68794a9daf6949f7149e9aca9de99.png)![](img/9f2bc7cb32c0920aaa9745f84dd63bcf.png)

Chains converged.

![](img/2ea3a68aa8e019d0bb49dfebdc9d4783.png)

A nice plot showing the correlations between each of the random components in the model. The term random component is misleading. In mixed models we talk about fixed and random effects, but in Bayesian analysis everything is random.

![](img/c4205078d4e44f96380d42d98e0ff68d.png)

Samples coming from the posterior compare to samples coming from the data.

![](img/fa9b648a1b71f154f38554e4b6abbb9c.png)

Conditional results coming from the Bayesian model

我们可以扩展上面的模型，以包括嵌套的随机效果，可以编码为(1|X|Z)。我们还可以在模型的 sigma 部分添加张量积。这会使建模很快变得相当复杂。为了实际创建这样一个模型，您还需要包括一个自相关组件，以及随机组件之间的相关组件。看看你需要包括多少前科。制作这样一个模型贝叶斯是极其强大的，但也需要大量的研究。我的意思是在创建/使用 rent99 数据集之前进行研究。

![](img/f81235916a1229210e5ebc9d7bc836c2.png)

The code I used to model a general additive mixed model using nested random effects and tensor products. I tried to take a look via the **gamm4** package

![](img/7d5026f6aa44b3f232f7e17083609088.png)

More priors to include. As you can see, not using any priors will result in flat prior use which is not Bayesian analysis. It is Maximum Likelihood at best.

为了了解如何对这些数据建模，我通过应用[](https://cran.r-project.org/web/packages/gamm4/gamm4.pdf)****软件包，使用最大似然法运行了一个一般的加性混合模型。这个包结合了 [**gam**](https://cran.r-project.org/web/packages/gam/gam.pdf) 和 [**lme4**](https://cran.r-project.org/web/packages/lme4/index.html) 包。通过将该模型应用于数据，我可以得到一瞥，但我也可以通过仅应用[**brms**](https://cran.r-project.org/web/packages/brms/index.html)**而没有任何信息先验来得到。无论如何，贝叶斯统计中的这种建模还没有完成。事先需要给出一个，它需要提供信息。******

******![](img/4137b6788da5d61de37aba2d6f020f2e.png)******

******Gam results are simple, showing the **gamm4** package.******

******![](img/a1d9d97d5cdd6dfea3e0fe5c52becac7.png)******

******The **mer** part of the gam model shows the tensor products, the correlation matrix of the fixed and random effects and the make up of the model. Here, we did not apply the nested random effects, but just stuck with (1|District).******

******![](img/21ea208dcf726fa7b6f28c327827e8e9.png)******

******Priors coming from **brms** without using anything and informative priors. I included an autocorrelation in the model, and also defined the cholesky matrix. As you can see, I kept them quite simple and uninformative. Even a simple general additive model using tensor products can become quite impressive to model.******

******![](img/b8e0e0aa344a90c2a587af4c89de5ef1.png)******

******The summary results.******

******![](img/3773daa8781aee3acf1136bbaba78592.png)************![](img/b927465f9b1610ae655238fc7108fa46.png)************![](img/f39511f1f14fc3981836a5a3b86cfdc9.png)************![](img/cfccf48391cd44b8871f876a25149ae7.png)******

******Chain convergence looking good.******

******![](img/72aac21d7b6a0f08a7ec63abbab6ff83.png)******

******Posterior draws on the overal response distribution.******

******![](img/0c82d75a3fbc82db84d5400c11dab68a.png)************![](img/cecb6849ca7d728635d9e343d45564d0.png)************![](img/cd1deea41f4b7de5c71a46dd309995bb.png)******

******The conditional probabilities coming from the model for each of the variables and the tensor product.******

******![](img/9380ebfa3e3220f3032b8728f1b0801c.png)************![](img/a557b4980746fcf475acb0c18d7c0fb2.png)******

******And, the tensor products for the mean and the error.******

******![](img/95ba6a6f868d49c255a6af28f4bd0b24.png)******

******Looking at the information criteria of each of the models. The first model did better, which is no surprise if you look at the end results and the number of the parameters included.******

******![](img/1c27ef836b337bf4bbd8d2f403c2bb6c.png)******

******Lets stick with the simpler model.******

******所以，我希望这篇文章是对贝叶斯模型中张量积的一个很好的介绍。如果有任何不妥、错误或必要的地方，请告诉我！******

******[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)******