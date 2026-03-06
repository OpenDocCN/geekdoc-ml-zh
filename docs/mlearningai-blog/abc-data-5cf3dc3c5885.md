# ABC 数据

> 原文：<https://medium.com/mlearning-ai/abc-data-5cf3dc3c5885?source=collection_archive---------5----------------------->

## 贝叶斯分析

贝叶斯分析通常被称为“另一种方式”或“新的统计”，但这当然是无稽之谈。[贝叶斯定理](/mlearning-ai/pr-non-vax-icu-pr-icu-non-vax-f7af477896ad)比费希尔的著作更古老，比最大似然法更符合[科学](/mlearning-ai/why-science-is-beautifully-human-and-very-frail-4f6225d32bb0)和知识的获取。

为了表明贝叶斯分析真的没有那么难，或者与我们人类处理(新)信息的方式相差甚远，我已经发布了几个贝叶斯分析。在已知的数据集上，比如 [Iris](https://pub.towardsai.net/good-old-iris-93358b8ecdbd) ， [Chickweight](/mlearning-ai/mixed-models-of-chicks-weight-a41413c99b51) ， [MTcars](/mlearning-ai/mtcars-f68f02c29a2) 或者 [Rent99](/mlearning-ai/determining-the-rent-d1431c90ca9f) 。今天，我将展示我如何分析一个名为 ABC 的模拟数据集。这没什么稀奇的，但也不尽然。贝叶斯定理创造了奇迹，尤其是在小样本中，它的力量来自于当前数据集之外。这就是为什么使用贝叶斯分析总会比任何一种最大似然法告诉你更多。

当谈到贝叶斯和贝叶斯分析时，它似乎经常归结为知道三个词:先验，似然，后验。同意，这些是需要知道的重要词汇，但它们不是贝叶斯分析的主要驱动力。那就是你，模型师。那个研究员。一个是在创建新的数据集(可能性)之前尽最大努力获取尽可能多的可靠信息，以将过去的知识扩展到新的当前知识(后验)。建模是人类与生俱来的，Bayes 很好地展示了这种有条件的工作方式。

让我们从 ABC 数据集开始，一步步学习。我把代码放在后面，我希望图表是帖子的主要驱动力。这是因为怎么做，你以后就能搞清楚了。但首先重要的是，你要熟悉 p 值[的缺失，并拥抱不确定性。](/mlearning-ai/inference-estimates-p-values-and-confidence-limits-a-frequentist-approach-acdd45d94bd5)

在生活中，这往往是我们所拥有的一切。

![](img/60b906bc7ec0d1a82929b30a593142bc.png)

The creation of data. Image by author.

![](img/dd56c16ca8ffbd36d1bf5acc4fb89fe6.png)

A plot of the simulated dataset. Image by author.

![](img/51f486bb86756180e7000bf79017ed08.png)

And a way of showing how each mean of each category differs from the grand mean. These plots are extremely easy to make in R, but add very little except more numbers, statistics etc. Image by author.

![](img/de030e44f28ab6de55f946477531c786.png)

Another plot showing how you can create p-values for each possible combination of comparison across each of the five levels. The overkill is clear to see, but make no mistake: the p-value is still a dominant driver in the sciences. Image by author.

好了，让我们快速转向贝叶斯分析。我将使用 [**brms**](https://cran.r-project.org/web/packages/brms/index.html) 包，尽管存在许多其他包，如 [**rstanarm**](https://cran.r-project.org/web/packages/rstanarm/index.html) 或 [**MCMCglmm**](https://cran.r-project.org/web/packages/MCMCglmm/index.html) 。像 R 中的大多数东西一样，你可以找到十种不同的方法来做同样的事情。

没有预先确定先验的贝叶斯模型只是一个使用最大似然的频繁模型。结果应该完全相同。如果我们不指定先验信息，那么 **brms** 包会为我们指定先验信息，因为采样引擎需要先验信息。您可以在下面看到，根据将要分析的数据集，确定了五个先验。这不是贝叶斯分析。

![](img/2e673eeb066a2bdffd00eea72a50241b.png)

Priors determined by the **brms** package by looking at the data. Image by author.

![](img/4695fea10a2b67d1b6a6f87a5e70580a.png)![](img/3c48e0286f063c66d1ed1bab16f50196.png)

The summary results coming from the **brms** package and the convergence of each of the three parameters of interest. Although you saw five priors in the graph above, the model actually only contains three paramaters of interest. The difference between **source default** or **source (vectorized)** is that they are named differently, but in fact the three priors for class **sd** call for the same thing. Hence, **brms** will only use one and that is why you only see three distributions with accompanying chains. Image by author.

![](img/a9c916f021bd801aa5148ffc386a4238.png)![](img/ce12b84df3ae2a80cb1fa7da93c9e620.png)![](img/c4c0171e89204ce8229b71d7736dbad2.png)

Below you see three different graphs showing the exact same thing, but slightly different: the posterior distribution of each of the five categories. Image by author.

现在，下图是最有趣的。我们已经在上面展示了 p 值，但是没有贝叶斯可以不关心 p 值，你也不应该。重要的是显示任何组合组之间差异的后验分布的估计值、效应大小和密度。因此，如果我们看到类别 A & C 之间的差异不包含零，这意味着差异不为零。在这里，它徘徊在 1.6 左右，这个数字有相当大的不确定性。这意味着，尽管 A & C 之间存在差异(考虑到该数据集和我们的非信息性先验)，但这种差异有一个需要包括在内的范围。这并不意味着差异将总是在 1.6 左右。

![](img/a9457ec13a32543bf51a2c74cece49f6.png)

Image by author.

如果你想要一个更好的，但不完全相同的，频率主义者和贝叶斯分析方法的比较，那么看看下面。最大的区别，但现在不是很重要，是线性回归 **(lm)** 模型将条件视为固定影响，尽管贝叶斯模型将其视为随机影响。顺便说一句，用贝叶斯理论讨论固定和随机效应时要小心——根据贝叶斯理论，所有参数都是固有随机的。

![](img/fc6955e7d3b836fb1ebc23a8d74a8983.png)![](img/4852b6ca5d63c51a9a21c50e7a4d9986.png)

Linear regression (lm) model showing the summary and assumption plots. Image by author.

![](img/1c136ba72cc29aa3b17dc65b3f8ecab7.png)![](img/44da6add8f00b6b360eb1a10d82720b3.png)

The Bayesian model. The estimates are not directly the same because the **lm** model specified fixed effects whereas the **Bayesian** model specified random effects. Image by author.

当在 R 中创建贝叶斯模型时， **brms** 包需要与 [STAN](https://cran.r-project.org/web/packages/rstan/vignettes/rstan.html) 采样器对话，它通过在将数据发送给 STAN 之前将数据转换成 STAN 代码来完成。在下面，你可以看到代码。如果您愿意，也可以通过 STAN 代码直接对数据建模。下面，你从头开始看斯坦代码。乏味吗？

![](img/8863e650aaa1f9298e773891a3266ed0.png)

Model in STAN language. It will often use half-cauchy distributions for modelling variance components. Image by author.

![](img/3d017c8d1c13e8043803059d1f1458b4.png)![](img/85b7b5505b99c16e18efc15e092e33f0.png)

Here, you can see the Cauchy distribution used, and the Normal distribution used. The Cauchy is not my favorite for modelling variance — I prefer gamma or beta. Image by author.

![](img/4966827ff5327bd2bcb095377da13632.png)

You can enter the STAN code directly and get Bayesian results. Image by author.

![](img/d73d602485298e1ea316ae2ebd4fe966.png)

And the Bayesian results. You obtain, for each parameter sampled, the **average**, **standard error** and **various percentages of the distribution**. You also see metrics, like **n_eff** and **Rhat**, related to standard Bayesian evaluation but they offer little to me. I prefer plotting the posterior distributions and see if the chains made the appropriate noise-like behavior. Image by author.

现在，让我们将来自线性模型( *linear_results* )的结果与包括固定条件( *bayes_results2* )或随机条件( *bayes_results* )的 bayes 模型的结果进行比较。

![](img/4bc655d2e5a9de636716375b333463a2.png)

The “posterior” estimates coming from a linear, and two Bayesian model. Of course, the linear model cannot provide posterior estimates. They are likelihood estimates. Image by author.

![](img/eb0d09a58b333a259028534896e455bf.png)![](img/9a92b294e52acba2dfd24e2f7f682763.png)

And the results, which deviate somewhat for all three, with the biggest difference in Bayes Random. That make quite some sense, considering how fixed and random components work. If you want to read more about this, and why [**Mixed Models**](https://towardsdatascience.com/introduction-to-meta-analysis-in-r-468e9b33925c)(containing fixed and random components) are the closest thing to Bayesian modeling (especially if you know that Mixed Models are models used for [**Meta-analysis**](https://towardsdatascience.com/introduction-to-meta-analysis-in-r-468e9b33925c)). Image by author.

我最不喜欢 R 的一点是，你可以通过许多不同的方式进行相同的分析，但是周围的特性往往会有所不同。例如，*采样*过程来自 **rstan** ，它允许我的 stan 代码连接到数据集并检索 R 格式的数据。从该格式中，我可以获得后验图，并绘制它们，但如果我将 **brms** 包与 [**bayesplot**](http://mc-stan.org/bayesplot/) 结合使用，看起来会有些不同。因此，许多许多的组合是可能的，但是你总是需要确定你知道你正在使用什么包。

![](img/8863e650aaa1f9298e773891a3266ed0.png)

Once again, the STAN code. Built from scratch. Image by author.

![](img/33fa8c7f22e6f8cc907b3019e658a5f4.png)

The **sampling** procedure. Image by author.

![](img/33031f37fda139f37f0dd18409fbe3a2.png)

The magic of **tidyverse** and [**tidybayes**](http://mjskay.github.io/tidybayes/) to get everything you want from your posterior draws. Image by author.

![](img/b9fd46fdd5cc620fd34d562d427e2207.png)

Posterior distribution of the difference between each of the categories. No p-values. You have to love this! Image by author.

我之前已经说过，您也可以使用 **rstanarm** 从数据中取样。

![](img/5682fc0d660c203d92515ee9c0811810.png)

The syntax for Bayesian modeling could not be more simple. However, no informative priors were provided so we can hardly call this model Bayesian. Image by author.

![](img/d073ba1123213b8aaa1ab53175b547ca.png)

The results from **rstanarm**. Image by author.

![](img/9adfc2773908dd7ebf7aa07220109a81.png)

Default posterior distribution plot coming from **rstanarm**. Image by author.

![](img/b3067f2fcc09650e518e27239fd6764f.png)![](img/d3601c5c0a1a81f58edfe9df9c03ec20.png)

The plots when using **bayesplot**. This is all you need to get an idea about the sampling efficiency and accuracy of your model. The chains need to have some form of convergence mixed with acceptable noise. No drift, no patterns. That is what you want to see. Image by author.

![](img/9634491e185c10954891a54909d9fa68.png)![](img/d30c32540906ef45e7fa6393709813f1.png)![](img/4cfce3ace1b1ceb616770a384cf05df9.png)![](img/b79390d0acc3f51b48cb530c6dfbf616.png)

And the posterior draws and plots coming from the model and its data. Image by author.

![](img/7d535a72486c321c06e7bdd301c11a5a.png)![](img/256e5ef08b3b3bdde231a4aaf5cc02a7.png)

And a very quick way to achieve what we have seen above in a simple syntax of code, using **MCMCglmm**, **emmeans** and **tidybayes**. So many flavors, so many options to chose from. Image by author.

到目前为止，我使用的是非信息先验，这使得上述任何一个都很难贝叶斯。从来没有理由不使用信息丰富的前科。如果你仍然决定去寻找无信息的先验，你是在告诉你自己和你的听众，你不知道你在做什么。你是说你不愿意使用过去的知识，让一切都依赖于你拥有的那一份数据？

这不可能。因此，我建立了一个模型，其中我使用了一个正态分布和两个伽玛分布分别用于截距和方差。让它们变得有用的不是它们的平均值，而是它们的方差。我把它做得足够小，让它们产生影响，虽然 ABC 数据库是一个模拟的数据库，但我现在确实有影响它的先验知识。这就是贝叶斯分析应该如何进行。

![](img/ccb76e374dfb391476f64ab9ed5d2269.png)

The model used in **brms**. Image by author.

![](img/606dc03f221ee7d8153b205ed143dff3.png)![](img/847bc543a92aaa645cf3ea2027c49ccf.png)

The results. You can cearly see how the posterior draws deviate from the priors, which is not a problem at all. As long as the chains converge and they do. Image by author.

![](img/538751edfbc5659074d009afeb78e496.png)

The distribution of the observed values and the posterior draws (**Yrep**). The solar flares extending the responses coming from the dataset is where the posterior distributions are showing the discrepancy between the prior and the likelihood. Image by author.

![](img/26eb25b50047c2e9b8c97955c32d449c.png)![](img/030342d08fa26e6d4ede1e980b532e3b.png)![](img/542bf2c3cb3c504f9d03c4eca83da59f.png)

Looking good! Image by author.

![](img/e9fb3707c05e5c16431f4c4a2ba5fd07.png)![](img/27c03d520890fdf8fb6131145392a18a.png)

Looking good! Image by author.

![](img/ee53bb3ba1d343c2694c2089ffae401e.png)![](img/dc3f28550654354df4cbcd5846bbb19b.png)

Looking good! Image by author.

![](img/aedfc7408ae74d4688bdc7604ab7c5cd.png)![](img/0586c906ab0fb6476c2b7c8152364b23.png)

Looking good! Image by author.

以上所有的图应该有一些抽样变异，但不是极端的。这是我唯一感兴趣的。[先验和数据很可能会出现偏差](https://blog.devgenius.io/analyzing-height-weight-5ba7449d6431)。没有伤害。这就是科学的运作方式。

![](img/a51527bc90f87059a6d3b28906ae6268.png)

You can see that the median response deviates from the posterior draws, BUT that the draws themselves are sampled beautifuly. Image by author.

![](img/1b8db2bf41d36b7b03acf1e9681f63ba.png)![](img/6dd4833854e236aa4285026b8e634b04.png)![](img/a966c10e8b48c9f4b074ed948c1f861f.png)

Prediction plots on the observation level. Image by author.

![](img/33da906573ba1f4246c1c571eb3b5f51.png)

Error plots. Image by author.

![](img/80feb06e37b7a1d26bc080f8bc95d08b.png)![](img/2135ac8bd77c3e71297f249895f993f5.png)![](img/2135ac8bd77c3e71297f249895f993f5.png)

More diagnostic plots looking if the distributional family of the response (here Gaussian) was chosen wisely. It seems it was, but that should hardly come as a surprise. Image by author.

所以，我希望这篇文章是有信息的，因为它表明贝叶斯分析并不那么难运行。真正的困难远远超出了本文的范围，那就是在你进行实验之前，确保你已经获得了所有有价值的知识，知道如何填补这个空白。因为这些知识不仅决定了先验知识，也决定了你获取新数据的方式。

获取知识需要专注，但最重要的是更多的知识。

如果有不正确或错误的地方，请告诉我！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)