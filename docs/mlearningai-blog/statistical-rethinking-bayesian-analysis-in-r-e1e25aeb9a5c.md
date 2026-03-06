# 统计反思——R 中的贝叶斯分析

> 原文：<https://medium.com/mlearning-ai/statistical-rethinking-bayesian-analysis-in-r-e1e25aeb9a5c?source=collection_archive---------0----------------------->

在之前的两篇文章中，我使用贝叶斯定理展示了[为什么科学是脆弱的](/@marc.jacobs012/why-science-is-beautifully-human-and-very-frail-4f6225d32bb0)以及如果你接受概率估计是有条件的会对[概率估计产生什么影响。](/@marc.jacobs012/pr-non-vax-icu-pr-icu-non-vax-f7af477896ad)

![](img/2e8143232821a1fd0cff22b9f4ade057.png)![](img/c8501c7ee6bd28eef5741235d45e54f4.png)

在这篇文章中，我将仔细阅读《T4 统计反思》一书提供的代码，并将它们应用于我自己的数据集。这篇文章的目的是向你展示如何用贝叶斯方法分析数据，并希望能把你从那些可怕的 p 值中吸引出来。世界不是二元的，它的变量也不是独立的。因此，我们应该珍惜这样一个事实，即研究问题没有确定的答案。

让我们开始吧。

首先，一个标准的例子展示了先验、似然和(未标准化的)后验是如何相互联系的。我只需要两个数据集——一个二项式分布先验和一个二项式分布似然。因为它们都是二项式的，先验是共轭的，这意味着它将是一个接近似然的快速竞赛。计算遵循贝叶斯定理。

![](img/0e10363281845eac08bf756d09195f7c.png)![](img/c0d23172867a551ebc82ec66c2c382be.png)

The posterior is literally a function of the old (prior) and new (likelihhod) data.

我在这里所做的是查看水的后验概率，提供我的先验和我观察到的。先验很简单——假设我相信只有 2/9 是水——也就是 22%。数据显示了一些其他的东西——它显示，平均而言，9 次中有 6 次是水。那么我的后验概率就是给定先验和数据找到水的概率。

我可以通过使用重新思考包来展示所有这些，并通过使用水和土地的二项式分布来分析找到水的概率。在分析之后，我可以提取先验样本、似然性样本和后验样本，看看一个样本如何影响另一个样本。

我还可以展示如果我改变先验和/或可能性会发生什么。学习进行中。

![](img/cd3b7a7bec496bbdf38ee83f8aa54a53.png)![](img/466cb9c112c30e5b9ff4e89fbbc3c688.png)![](img/ad662539c133496ed757f8e5b301e591.png)![](img/1c171d109b9df5207c596f7cebc4784c.png)![](img/b9b1ac314e791c875c68a5c6669bcae5.png)

Old versus New posterior based on New data that has the EXACT same distribution as the previous data. The more evidence you will find for a given distribution, the more the posterior will shift towards that distribution. Regardless of the prior.

好的，以上是一个典型的二项式分布的例子，它几乎总是被用来让人们开始。让我们加载一个数据集，其中有来自各种试验的数据。数据集可以做的比我在这里展示的多得多，我在这里展示的实际上是贝叶斯分析的入门知识。

有各种各样的包装器包可以让你更容易地通过贝叶斯方法进行分析，比如 R 中的 [BayesRules](https://www.bayesrulesbook.com/) ，但是重新思考包更基本。因此，我认为这是一个更好的开始。

![](img/3011d76c486b15bc32731928b369d0f3.png)

I will focus on ADG and ADF.

![](img/093fb8af65c1c5fda67d3c94d4aa9908.png)

The relationship between ADG and ADF, and their respective density plots of the mean.

![](img/72367da7d9a1e5f7b3c04107f7a664dd.png)

Relationship between the mean and the standard deviation of ADG.

Lets obtain the Percentile Intervals of the mean and sigma of ADG using the posterior distribution.

![](img/fbb1eb49497ef28cf471ccac3f786306.png)![](img/01e2e6f1517f3b258ef00eb291434401.png)

The posterior mean and standard deviation.

![](img/c9eeaaddcc7c35f39e23cb3b22cd8c7f.png)

And an overlaid plot showing the kernel posterior distribution and the normal distribution on top of that. The density curves have shifted because I used a different grid from which to obtain posterior estimates. The left pot shows the correlation between the mean and the standard deviation.

现在，到重新思考包开始分析数据。 *quap* 代表二次近似后验分布。因此，这不是马尔可夫链计算。那是以后的事。

![](img/bfd2125159e7b480abf84afc6e28d740.png)

From this plot, you can see the model with its prior information, the estimated mean and sd coming from the posterior, the variance-covariance matrix, and the posterior samples. You can also see the summary statistics of 1000 samples coming from the posterior.

![](img/65f7d46181c66199030af726ed8f316e.png)

A nice plot showing the distribution of both and their correlation.

Different prior likelihoods.

![](img/b3a9eb322e2bb3488a37077a15e4391b.png)

Different posteriors. Not so much though.

让我们看看 ADG 和民主同盟军之间的关系。

![](img/98fe2c59069bada6cc96875a18cde130.png)

Simple linear regression to start with.

![](img/ec06bba881dcf63b8adb0e293182bfe5.png)

Checking assumptions.

![](img/9df897da3e5e0efc66f7d5d1c2ce887e.png)

And, using plots to show how priors can impact posterior predictions. In the graph all the way to the right, the prior is non-informative, letting the data do the work. The predictions are not great though, using a different slope then expected.

L 让我们更进一步。直到现在，我们没有做任何真正的回归，贝叶斯风格。我们现在将改变这种状况，深入研究民主同盟军和 ADG 之间的关系。

![](img/802467be17130db8e42e3ff46e1fd955.png)

The prior density plots of the mean and SD of ADG (top), the likelihood density plot (below left) and the estimates of the variables inside the model. The prior and the likelihood will determine the coefficients which determine the posterior.

![](img/3cb38c42aaf6d5e742266ff71562642d.png)![](img/9a4c4ec4c0f3acda5d5c5b38e768ef2f.png)![](img/0d6117620a3093bbf18a8e8e97357b84.png)

The posterior density plots. These are density plots showing the distribution of the coefficients of the model as depicted in the graph above (below right). The spread of the intercept is noteworthy!

![](img/c8117bf3ac15f1a1048a592e79ea1d2c.png)

Five samples from the posterior.

![](img/01b26f8559fbaad2e6baa4c735b31adc.png)

Marginal regression line coming from the posterior distribution.

![](img/1f2abfb0a842cdc1ebb7e0349aefd1a3.png)

And 100 posterior regression lines. The slope seems to be off. To the right, the ADG when ADF equals 90.

![](img/d0066d055e66a8a6163b1fe908ab0837.png)

Code showing the posterior predictions from a dataset of 50 samples. You can see the density of mu, the 50 predicted lines, and the confidence and percentile intervals. I wanted to predict at ADF ranges from 90 to 115.

让我们继续前进，评估不同的模型。在这里，您将看到我使用二次模型，构建一个额外的变量，它是原始 ADF 值的多项式。我将尝试重新创建平均值和后验值的百分位数间隔。

然后，我将选择一个三次模型，加入另一个多项式 ADG。这就是事情变得棘手的地方，因为从原始数据中可以清楚地看到，ADG 和 ADF 之间的关系是相当线性的。如果你把模型推向一个不适合它们的方向，它们会表现得很奇怪。

![](img/8eb67e683de18c22f8e5adb98623ee85.png)

The percentile intervals of the mu and the posterior samples for the quadratic model (left) and the cubic model (middle). That is right, that model exploded in my face. To the right you see the estimates of the coefficients coming from the quadratic (m4.5) and cubic (m4.6) models. The intercept is asking for all the attention, and it seems that for the cubic model, the coefficients of ADF are almost zero. Which explains predicted values that fall outside the plotted area for that model.

现在，让我们比较所有三种模型——线性(m4.3)、二次(m4.5)和三次(m4.6)。

![](img/70c4881ac0536e6f2203b663757132b0.png)

If we compare the models we can see that the quadratic model performs best. However, if I plot the data, I do not believe it makes that much of a difference. The prior should safeguard against overfitting , but the way I modeled it here and the prior I chose is too much on the data already.

![](img/555106b5ce48798696914072bcdd7845.png)

In this plot, you can see the QIC of each model, with the best on top, and the deviation from it. The confidence intervals of those metrics overlap. As a result, I would personally go for the easiest to model solution, which is the linear model.

N ext up 是分类变量的包含。有许多方法可以在反思包中对分类变量建模。我将强调几个。

![](img/ce82a8f1ca0a619fe5db7e4a1a57785f.png)

Estimates

![](img/365f38ac643246051ae260a930e92236.png)

Prior and posterior histograms

![](img/09397c106262ae5f1c060f4975c80605.png)

Comparison of models, graphically.

![](img/56d23304f7c9b217e622660937d09b89.png)

Comparison of models. When looking at these results, I would personally go for the easiest to model data.

现在，一种不同的方式包括分类变量*国家*。

![](img/08a336966e417471414691ec27f292b9.png)![](img/10deb9be1f9650ef7e48c4d30330d30e.png)

These results actually allow the creation of new posterior values, taken as differences between countries. Do note that distributions of main effects are not the same as distributions coming from differences. You would not be the first to make that mistake. The model including both Country and Challenge is by far a better model than if you just include Country alone.

接下来，我们将使用多个预测因子，包括交互作用效应的模型。

![](img/a41d48e7c674dfeb04d46207795ce458.png)

And the posterior density estimates. The zero is included. Whenever a zero is included this means that the zero is a possibility and something you need to take into account. Unlike the frequentist method, this does not mean that there is NO effect, or NO coefficient, but rather that the distribution of the coefficient estimate includes a zero.

它来了，通过 *ulam* 代码的 MCMC 方法。

![](img/50d384af6af8a47dac8e41c805c5ee22.png)

You can follow chain progression via the R Viewer.

![](img/b127a581c4161103e32aba926e9a2ea8.png)

And the results coming from the Hamiltonian Monte Carlo approximation, using 2000 samples across 4 chains.

![](img/ec7374df4a44bdb66932c41b8fb23e67.png)

And the posterior estimates of the coefficients.

![](img/1123dff25842c7f469ed1e163e6d1bdb.png)

Posterior Density plots and their correlations.

![](img/ebf51684efd7fe677d5f59cb8ff0175a.png)

Chain progression — what you want to see is that the chains intermingle. Very densely.

![](img/4e56ff6f39f669962de514d98b66f193.png)

And the traceplot which is a simpler version of the chains plot. Here, you want to see an interwoven pattern. What also shows is the effective sample size. The more the better.

至为止，关注结果持续。现在，我们将构建一个离散的结果[0，1]，并观察模型如何运行。

Quite straightforward. The difference is the Binomial distribution used for the outcome — the rest of the coefficients have a prior coming from the Normal distribution.

![](img/4b3f19ccc223aa3930d7708ac83f10da.png)

Quite straightforward.

让我们更深入地研究一下，同时添加一个分类变量。

![](img/bdd96d52cad238726adfc542788fc0ae.png)

The fit from the GLM model.

![](img/3e8026040f64f545d4faf2adc04f3d80.png)

The fit from the Bayesian model. They are not comparable in output since the output of the GLM has an intercept which includes the dummy reference. In the Bayesian model this is not the case.

![](img/4dfd49e80143c334a172efb9a3e1f5ce.png)

The difference of the absolute probability difference between country 1 and country 2.

![](img/6df493a2787513b0e152ed307484fa74.png)

Quite some strange density plots which look more like mixtures then anything else. Perhaps I am missing explanatory variables.

![](img/dfade4c564b79a3db91eb7e721c5b6d9.png)

The odds ratio between country 1 and 2, and the density summaries for the inverse logit and the probability values when comparing two countries. You can see that the probability results differ from the above inverse logit difference. That is because here, we used 1000 samples from the posterior AND we used the absolute values.

The same as above, but this time using Hamiltonian MC.

![](img/33f24ad630c7a3a6dffbcf2f9212316f.png)

Similar results to quadrature.

![](img/9231021e16adaeb4ef3aebdf24c3ebbc.png)

Looking good

![](img/1475128d1c9cdf5e9b609a3f847bf3a0.png)

Looking good, but the effective sample size too a dive.

![](img/ae1ec617d1dce0f6603c910a4b516263.png)

Looking good.

O 呈泊松分布。泊松分布总是一个很好的使用，因为您永远不会遇到问题。

![](img/fb4ff347625bb4ea8a740b8f96c8f97d.png)![](img/5bd443855a60f0756f2985921de0a91d.png)![](img/2bf3f8f52efeb1decd42b44468ab115c.png)![](img/419402d1965d7154c3214c3fe13ba7a2.png)

You really really do NOT want to see this. The funny, but disturbing issue is that the results do not reveal this immediately, unless you start looking at the Rhat4 which shows the level of convergence of four chains. The higher the number, the worse it is.

正如我已经说过的，泊松分布还有很多地方需要改进。这就是为什么大多数建模者会冒险使用负二项式模型或伽玛-泊松模型。

![](img/7bd906633e9a57e10b1f54b363d0a1f9.png)

Looking much better.

![](img/e2e4337f9c0a63276e1a042402b529c4.png)![](img/f583cb857f166b03a2b2fc6c0b28151a.png)![](img/db0ba266e6d3e3850a0e04c86513b1d5.png)

Much better.

![](img/93319be8de0e3f37224f9704ceb94109.png)

Since they are both on the log scale, they ARE comparable. The Gamma-Poisson performs much better.

B 您将看到运行多元模型的代码，同时估计 ADG 和 ADF。依赖于协方差矩阵。

![](img/001d334d57b5b4de46cbf64549595e01.png)

Looking good.

![](img/1ed0859f86bd55056b55fcdfbbffe8a6.png)![](img/1e7766d92f8b8910a3b118bce2e09426.png)

Looking good!

L ast，但并非最不重要，混合模型。我经常发布关于混合模型的文章，我将在这里展示如何为它们建模。实际上非常简单。你只需要在它上面增加一层。

![](img/cc5d8548538a6211b73e51f90e84d4a6.png)

The fit from a mixed model showing the estimates of the two variance components (Country & Residual), and the estimate of the intercept.

![](img/5e17fef2750fad3bf1df4133d87d8240.png)

The estimates coming from the Mixed Model.

![](img/0685a2d284d4ce157e204276b10f59df.png)![](img/af2c2fe2034192dd2c1d674aa1281ca5.png)![](img/4db72b6bd31d3afc330f447e853fc2a7.png)

Density plots and trace plots looking good.

![](img/6a0253ca8a72c6d7819fe14d1d762fa3.png)![](img/d924858ff2150efdb472cca70ee3b8eb.png)![](img/0b6c78c9959a99122df8f637432dd372.png)

Posterior Validation checks also look good.

![](img/5b09a03da6300564ad1b0d7132e189dd.png)

No model really sticks out.

让我们随机添加多个成分— *国家*、*年度*、*治疗*。

![](img/ddf6d79d1d92ad1dac2d00e00ead9ab0.png)![](img/4b6bcfaf4abc13ebefec01e16850c077.png)

That’s a lot to process, but the chains converged. The effective sample size decreased heavily though.

![](img/e7b896bb2704c3d69887303a918e2938.png)

Looking good.

![](img/782c1426d70d19c32698ec2896252964.png)![](img/fd99cf4b04584ed59f0d4509e314a4f5.png)![](img/f3307918686a9eacfa8294c1743bb8da.png)![](img/360619c774e21e747ff97779aaa9f70d.png)![](img/5c5a0ba624cfd4051e5f40e234fd21e2.png)

Looking good.

![](img/f00f51a9bef5fb8aabda3b92fa88433b.png)

Last model is definitely the best.

![](img/5aad29bd07aa7dfa556f2016b0a442e3.png)![](img/e2362798f0efb14216f1a0c5004294ea.png)

Density plots from the last model.

好了，我们结束吧。我可以很容易地花更多的页面来展示例子，将来我也会，因为我展示的实际上是贝叶斯分析的介绍。我希望代码有所帮助，如果没有，请告诉我！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为移动人工智能的作者

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)