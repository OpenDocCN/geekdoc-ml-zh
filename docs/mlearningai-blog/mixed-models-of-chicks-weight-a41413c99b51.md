# 雏鸡体重的混合模型

> 原文：<https://medium.com/mlearning-ai/mixed-models-of-chicks-weight-a41413c99b51?source=collection_archive---------5----------------------->

## 贝叶斯方法

那些读过我的帖子的人知道，或者很容易发现，我经常使用混合模型。事实上，考虑到混合模型的[正则化能力](https://pub.towardsai.net/blups-and-shrinkage-in-mixed-models-sas-3fbc6662fa6b)，我几乎看不出为什么还有人会决定使用正则线性回归。

在这篇文章中，我将对 ChickWeight 数据集使用贝叶斯分析，这是 r 中的一个标准数据集。这个数据集的好处是它是纵向数据，这使得它是嵌套的。因此，对于混合模型，处理这种数据集应该没有问题。

《贝叶斯方法》的副标题当然是用词不当。没有贝叶斯方法，只有数据、信息、知识和更新的循环，当涉及到整合新旧时，可以说没有比[贝叶斯定理](/mlearning-ai/why-science-is-beautifully-human-and-very-frail-4f6225d32bb0)更好的数学框架了。

因此，我在这里要做的，以及这个例子旁边的许多其他例子，是向你们展示我如何使用[贝叶斯定理](/mlearning-ai/pr-non-vax-icu-pr-icu-non-vax-f7af477896ad)分析数据集。与连接到 R 的所有东西一样，有许多可用的包让您连接到 STAN sampler，您将看到我使用不同的包来分析、检查和扩展我所做的分析。

这整个努力中最重要的一部分我将不得不忽略，这就是我如何选择我的前科。我拒绝使用弱的、无信息的、一致的先验，因为它们在数学上等同于知道绝对不可能的知识。

所以，我用了我不能真正备份的信息先验，因为确定你的信息先验需要一个完整的科学周期。这并不容易，但一旦你有了它，贝叶斯分析远远胜过任何一种频率，当代看数据。这就是为什么它只是贝叶斯定理的一部分。一个重要的部分，但仍然只是一部分！

让我们开始吧。

![](img/a394c039410e7d9d18b091f78562b089.png)

A quick look at the chickweight dataset shows you it is longitudinal data which is nested.

![](img/762d01ffe563bb4265c1bafa6f92c3a4.png)

Fancy output of simple linear regression models. Too fancy, and not informative at all.

![](img/4a1df164be0ce995470edd382e9d2271.png)

Boxplot of the data. We have four treatments, 12 measurement points and an increasing variance.

![](img/6db4bea2b1e15a4160f0df39fb88c26a.png)

The smoothed Diet means. Their meaning fades as time goes on due to the increasing variance.

![](img/9fbc3067e1a4610561480ba2bb23ecfc.png)

Showing the raw data on top of the means.

![](img/b52ecb336d332e10ce79471295a0b466.png)![](img/d921d72c7d0d299d94f85d87325212b1.png)

A first regression model, from which I plotted the grand mean (intercept only) and its diagnostics.

![](img/1f5956ba96843f435918feddd27809cc.png)![](img/aff8e19809a138f26c5e8658b448e011.png)

A second model, which is a mixed model, from which I plotted the predicted values coming from a random intercept (not a random slope). The calibration plot shows the model is not that bad, but does deviate seriously as values increase. This is normal considering the increased variance.

![](img/b7c3e372b2a38232463abef5bce07097.png)

Trying to see if a linear regression (blue line) does better than a polynomial (red line).

![](img/b480bc67dc4ba6b34ea3c03c538e5716.png)

The quadratic model performs best compared to an intercept only or a linear regression (yes, the polynomial is also linear in its parameters but you probably know what I mean).

![](img/dd657b1c6aef85db7b147994318a2e69.png)

The linear random intercept random slopes model in a nice output.

![](img/de6104b3a86b9312c84dfb3ca786a364.png)![](img/c262a4c69b9990db65664ab1296a89d6.png)

The predictions and calibration coming from a linear random intercept random slopes model. The model is severely overfitted, which you can see in the left plot. Because the random components (intercept, slope) are extremely correlated, the model will determine predictions from start to end, and from end to start. Hence, why it has such a problem predicting from the start. Fixing the values at the start would be of help, but for now, we have done enough frequentist modelling.

![](img/6c2c5285a8fa0aef3c58df1d77edf626.png)![](img/61baa1db25e066deab0ab68f008e38ab.png)![](img/7117f849619cb3717ccdd7753e2ca03a.png)![](img/78949579fb17b700875bbc230ecce884.png)![](img/10d3b2e6bdb6dc2157d7ab6cfed6d9e5.png)![](img/3a3c15509cc16cffb0f1b8a76692ea4c.png)![](img/2a5c7665c8b74320d71aefd3ddec5cb4.png)![](img/58b33dcc818ea0687ab37ebde11f64bb.png)![](img/5cbd05511dec722de406312bc5762e5b.png)![](img/bcd8a597f82a8500b2c2e7dd2962b04b.png)![](img/0c857325eb7c1889f6deee0b3fa70b50.png)![](img/268a1fd90a6ffd73cdc77060ef160dd1.png)![](img/b7264ff5fd88b14c554d31b8d81fc101.png)![](img/1219adcf2f5baeb1199538c04f257561.png)![](img/ceb7662d14f598b91f9e76c537b0a0ec.png)

A lot of plots coming from several packages by which you can assess a mixed model. It may seem like overkill, but a mixed model has several assumptions that must be adhere to, and the model itself can easily overfit and become unstable due to the correlation matrix of the random components. Another thing we did not put IN the models is the autocorrelation component of the residual term which is necessary to deal, in a robust manner, with the standard errors

现在，让我们移动到贝叶斯建模部分。我将首先展示 *lmer* 模型和 *stan_lmer* 模型，它们具有完全相同的语法，但肯定不会给出相同的结果。例如，贝叶斯模型根本不关心 [p 值](/mlearning-ai/inference-estimates-p-values-and-confidence-limits-a-frequentist-approach-acdd45d94bd5)，你也不应该关心。永远不会。

![](img/0bff4e9b930c17fdf46342fae00ec32e.png)

The **lmer model** results, showing a random intercept random slopes model. The correlation of -0.99 between the random components should have you worried. A text-book example of overfitting the model.

![](img/6d4b3188c8af993323c05edf51ff1e67.png)![](img/69227262320955adad7a51c57f1e3001.png)

And the results of the **Bayesian model**, which shows all the posterior estimates. Do note that NO prior distributions were specified, so the results should amount to almost exact replicates compared to the **lmer model**. We see it does not, but does show sufficient overlap. Nevertheless, a Bayesian model with no prior is a model bases solely on the likelihood, which is similar to the maximum or restricted maximum likelihood estimates we see in frequentist models.

![](img/73c9392cedd356d792b8ce7a565ef16e.png)![](img/a3f5fd9d4ded3654acd2d9bb4e920a79.png)

Just as we can assess the random components of the **lmer model**, we can also compare them of the **Bayesian model**. Looks good to me, with sufficient rank order to warrant the inclusion of both random components (intercept and slope).

![](img/5a21d9ef8f0e828616aa136d1ea27417.png)

The predictions coming from the **Bayesian model**. If you compare these posterior draws to the predictions from the **lmer model**, you can see it suffers the same problems. Way to high correlation of both random components leading to some bad predictions at the start. Correlation is not causation and leads to issues both ways.

![](img/c2daeecae1d862b22b2884ae775ddb3f.png)

There is another package, called **brms**, which can do the same as **stan_glmer** but is even more intuitive. You can see its results in the plot above.

![](img/02c51b56beff0ecef409240a73824ed2.png)

And the posterior distributions coming from the model, which had no specific priors included. Hence, nothing really different from a standard mixed model constructed in **lme4**.

![](img/60bd688c36724f94dbc179f5b63017b5.png)![](img/5fd770e7d150cefd35760cfe9c14c941.png)

Posterior draws from which to assess the chains. You want to see no patterns in the chains, it needs to be messy, noisy and overlapping.

![](img/d0ac106c8b7bbf6ba0e5bb924791d1df.png)![](img/5cc6d6544c2b92f19f03986c45093d53.png)

Differences between observed responses and posterior draws.

![](img/06f8f38597241b23e796d2488e068e38.png)

Assessment of the (mean, sd) of the observed response, and that of the sampling space. What you want to see is that the sampling space does not show any boundaries, not for a response on the normal distribution. However, what you DO NOT HAVE TO SEE, is that the observed value is within the sampling space of the posterior draws. The reason why the point of the observed (mean, sd) is almost in the middle of the sampling space is because we DID NOT indicate any priors. What you see is just sampling, nothing more, nothing less. Looks nice but hardly carries any worthwhile information.

![](img/443d4cd2fec36a4eb2b50bfc01a85a15.png)

Leave-One-Out resampling looking for Probability Integral Transformations. What you want to see is that the values lie on the diagonal line, indicating that the model by itself is able to pick the underlying distribution, which is normal. *In probability theory, the probability integral transform relates to the result that data values that are modeled as being random variables from any given continuous distribution can be converted to random variables having a standard uniform distribution. This holds exactly provided that the distribution being used is the true distribution of the random variables; if the distribution is one fitted to the data, the result will hold approximately in large samples*

![](img/67913e9512fc461f1c94398d2eff7ab9.png)

The observed median response, per diet, and the sampling space coming from the posterior draws.

现在，让我们进入有趣的部分。确定先验。老实说，没有信息先验，贝叶斯分析提供的消息很少，并且陈述没有信息先验的贝叶斯分析不是获取知识的方式。事实上，我会说，使用非信息性先验比任何信息性先验更难辩护。因为，如果你使用了一个非信息性的先验，你为什么要首先进行这项研究呢？填补空白，当然，但它不是凭空出现的。此外，你如何估计运行一个像样的[实验](/mlearning-ai/general-introduction-to-design-of-experiments-in-animal-science-using-sas-for-codes-fe1c5272b37a)所需的[样本量](https://towardsdatascience.com/how-many-samples-do-you-need-simulation-in-r-a60891a6e549)？

因此，让我们看看 **brms** 包在数据中找到了什么样的先验，我们将陈述哪些先验。请注意，我没有进行任何事先分析，也没有对什么是好的前科做任何研究，因为我没有设计这项研究。但我确实确保了事先提供信息，把它们保持得很紧，但不要太过分。

![](img/98d0ba0ae4832b0a3f1dd7c0e97f580b.png)![](img/d5510c1db7334ece173489eacdfbb518.png)

For any kind of variance estimate, **gamma distributions** work wonders, as they have a natural boundary of one which is also the natural boundary of a variance component.

![](img/71f1fa77ea9277ecb85d4d4b35a6b204.png)

The posterior distributions are now a true combination of the prior, likelihood and the entire dataset.

![](img/a57ef5c5623d23a7209037db4f9c7dc4.png)![](img/900aab3612c294014c012e811ec7e1c3.png)

Chains look good.

![](img/bb6c167f349a3904eb2d7681ac77fb53.png)![](img/27d16b582d9c8ce674c9f9225b14ffa0.png)

Posterior predicts look good. Still issues on the intercept though.

![](img/4fd2e5cc9a481280176b8580bf92a995.png)![](img/916fb0f77908847e01d1d02960591055.png)

Sampling went well.

![](img/87fa8ac134de58e70bf0972f1442ebc1.png)![](img/a29cb0c053c60bd1dd78681a59c45845.png)![](img/960267231cedf84e1e9d14724e272e90.png)

Predictions look good as well. They DO NOT have to be though. This should never be forgotten. All the previous plots are really to assess the sampling, not if you are able to fully connect to the latest dataset. That is not how science works.

![](img/7892dc361ba11889e1e6f6114fd7f30b.png)![](img/62311c79eef3496b9dcd71aa19a91748.png)![](img/227760161f6eaf964d2268faf7533cac.png)

And more fancy plots.

![](img/8fde439263f1e3826c534b2b2dfc35b7.png)

Even more fancy plots.

![](img/5d45810bf3bfb8a0322ceff10fefa61e.png)

Posterior distributions of the diets. As you can see, just looking at the diets without taking into account **time** or the **chicks** makes no sense at all.

现在，到了有趣的部分。比较饮食，边缘的，或有条件的。就像我之前说的，忘掉 p 值吧。它们完全没有意义。查看差异的分布并非没有意义，因为这些估计值有助于更深入的分析和在新模型中的进一步整合。

![](img/56de6b7d630cf30ccddd3bc00f662a0f.png)

Any differences that includes a zero in its distribution is not meaningless, but cannot rule out the possibility of no effect. The 5% threshold we often accept in frequentist analysis does not play a role here, although of course, it could. But I will not.

让我们运行一个最终模型，看看**时间**和**饮食**之间的相互作用。

![](img/92d974fc528ff1b5420cf158dcc1de00.png)![](img/5dc7a259af34a199e2ca2003240dc349.png)![](img/6254e48c575164c2e09e220d2421116f.png)![](img/3c29465d893ca11a4e22f1b71cc9d122.png)

Predictions seem to coincide with the data better in the middle of time, not the beginning or the end.

![](img/2f4f3a61eb330fa472c072ab750a444d.png)

Comparison between diets at time = 10.7 which is the mean time point. Of course, one could enter any time point they would like.

我希望这个小例子给出了一个关于如何使用混合模型对纵向数据建模的新观点。代码在下面，更多的帖子将跟随这个。

尽情享受吧！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)