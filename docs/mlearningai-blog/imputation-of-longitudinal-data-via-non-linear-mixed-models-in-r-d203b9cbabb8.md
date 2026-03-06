# 纵向数据的插补

> 原文：<https://medium.com/mlearning-ai/imputation-of-longitudinal-data-via-non-linear-mixed-models-in-r-d203b9cbabb8?source=collection_archive---------1----------------------->

## 通过 R 中的非线性混合模型

一个好的开始，但不是最终的解决方案

在这篇文章中，我将分享我使用非线性混合模型对纵向数据进行插补的经验。这意味着我将展示使用生物学公式输入缺失数据的可行性。在这种情况下，奶牛提供的牛奶产量(MY)。奶牛产下幼崽并提供牛奶的时期称为产次。确切的时间通常被称为“产奶天数”(DMI)，并且已知奶牛有多胎。这些胎次是一个众所周知的预测我的常见指标，特别是因为奶牛的产奶曲线往往在不同胎次之间有所不同。

手头的数据集包含近 10 年的数据，累计超过 40 万次观察。数据是每日的。为了确定我的预测，我们必须首先处理肯定存在的缺失数据。尽管存在用于输入缺失数据的复杂方法，但我们现在正在处理一个已经开发出生物非线性曲线的过程，例如[伍德的模型](https://www.sciencedirect.com/science/article/pii/S002203029776185X)。

![](img/266eb2270bdd3d2d26846f1fa0ed1aa5.png)

因此，我们可以尝试使用众所周知的生物学公式来估算缺失值，而不是使用随机森林之类的机器学习算法或构建完整的特定条件插补模型。

让我们看看这是如何为我们工作的。

Lets import the data

![](img/dd25ec8b7733c7b867d278c66617d9fc.png)

And then look at the data. We have quite some data, of which some are missing at a very high rate. The most important are Dry Matter Intake (DMI)and Milk Yield (MY). Those are abundantly present.

T 他要做的第一件事就是在缺失的数据中进行更深入的研究。缺少多少个数据点？有模式吗？我们能在失踪人口中找到能帮助我们估算他们的关系吗？在前面的图表中，您已经看到了缺失百分比的高级概述，但现在我想更深入一些。

Some code on missing data exploration. It is all straightforward and what I am looking for are associations and things that stick out.

![](img/778c84a96a5b40d05804d71cc640e598.png)

Missing pattern analysis. This shows that missing observations for body weight (BW_NA) often go hand in hand with missing for Body Condition Score and BW in the morning or BW in the evening. Which is the case for 143928 observations. Then there are several more combinations which all have to do with these five variables.

![](img/d986b78d6ff2b755a4489f54e21a0cf0.png)![](img/5b0560ed0b7ebd8f506743793b254ddd.png)![](img/7693daa0b7e229d809d645b49d7bd12b.png)

Missing data overal

![](img/f5de5ddd3ed4390671d7b26d6acd88bb.png)![](img/537254fbfec8f05747d78386af110e3c.png)

Missing data per factor

![](img/919b90c644fb2596c18ca0c7e352b96e.png)![](img/6d50010514eb782df89fa5a9c157be97.png)![](img/0eb0d41e5d5fae7d89ae415b02ac890b.png)![](img/7d1461cd07d2c5656b0edd5342399237.png)![](img/0135446c882e3f5e8ed60f6a938908fa.png)![](img/873f67230cc8d0aae238f3fcca486a27.png)![](img/ccb749cdbfa5adcf37ea177f3cc86c7b.png)![](img/d6304f07e1a244b596c75ead369d70c7.png)

Missing data per factor extended.

总的来说，上述图表清楚地表明，缺失的数据仅限于几个变量，这些变量在我们当前的分析中几乎不起作用。当然，产奶量除外。所以，让我们深入挖掘原始数据。就我个人而言，我发现 R 在绘制这种数量级的数字时有些吃力。

Some code to plot the data

![](img/7ce5372c5cd5cd0b699add6dfa2cbf70.png)

All 400k data points.

![](img/615b3e59e37da6ce37a2541d2ff886b7.png)

Relationship between Milk Yield and Days in Milk as a function of parity. We have a total of 12 parities included, of which only 11 produce data. The higher the parity number, the less data we have. Also, there is a lot of variability within parity.

![](img/8d3d8062fa80117a6566df7a235f0fc6.png)

The relationship between Milk Yield, Days in Milk, month and year. We have almost 10 years worth of data.

![](img/02ef802728ed28ae933db46444d97b1e.png)![](img/941e83266b5e03e322c9f6747f72fab7.png)![](img/e0c5a1b6920d426080e0bd9daadbe337.png)

Relationship between Milk Yield and Days in Milk — by parity — for weeks, years, and number of calves. The plots do not reveal any interesting relationship to be honest, but then again, at these numbers, scatter plots can easily reach their visual limits.

![](img/e8a92f3c72aae9f66f3ce82860d1370b.png)![](img/595a66d02d863b330542dde3d4458caf.png)

Left: Relationship between Milk Yield, Days in Milk, and Parity which the Cow ID determining the color. Right: relationships plotted for several cows. As you can see, the curves have missing data, and can become quite unwieldy.

现在是时候进行一些非线性建模了。正如我之前所说，产奶量和产奶天数之间的关系可以用特定的公式来建模，Wood 的公式就是其中之一。下面你会看到我在观察数据上应用那个公式，并评估模型拟合。

![](img/5ccc27dc8e8fd8a4fadc0cf271d71469.png)![](img/c3564a5996a8ddff4a00c7e354d5c8e2.png)

Left is the original curve by observed data, right is Wood’s formula with arbitrary values. The fit is not great.

低于公式的系数。我想看到的是标准误差的大小低于估计值。这里的情况就是如此，这意味着数据可以为模型提供良好的基础。

![](img/1f64ab73577c849822959e6edfeeb3ed.png)![](img/2d5ab2d7c500c92a63a1761612bbbe07.png)

Results including a correlation matrix. That correlation matrix is very important since extremely high correlation means that the formula can be simplified. A and C seem to have high correlation, but we have not included random effects yet. Since the formula has also been tried and tested, I will leave it for now as it is.

![](img/c6aa8677f31af63fe6139ebef96ae619.png)

Confidence intervals looking quite good — not too broad.

![](img/32423fa31f7b35489e3dfcc8db2fb315.png)

Observed data with overlaid lines — first guess in red and fitted line by non-linear model in green. The non-linear model fits the data really good, even though it is not perfect. Remember that the NLS formula fits a marginal model — for each cow and each parity the same parameters apply. As such, its quite amazing to pick a good fit in this example since it could have easily been way off.

![](img/ec93cc7bb1e8a3a24cf770b66381ee1c.png)![](img/3b80f741f4f318d91497e8a0ad485eae.png)![](img/4b19010bfe3691107269147022159d60.png)![](img/a4ad9aeef4ccb43b1124cba77f84d559.png)

Plots showing how a, b, c, and d are estimated. From these plots you also get a sense of the size of the confidence limits.

我们知道产次对产奶量曲线有巨大的影响，所以我想看看能不能通过产次来调整固定效应——*a*、 *b* 、 *c* 、*d*——的影响。为了简化建模，我将把自己限制在前五个部分。

Model to adjust fixed effects by parity. Rest of the code is to check the model.

![](img/37d515d2100994580f45dc37e359feaa.png)

All parameters are significant. Standard errors are mostly of a magnitude smaller than the estimate.

![](img/75ca92b282b4ccf6aa8f0788b5939269.png)![](img/e3794ab79ed18ee4d5aa42561b701c7b.png)

Residuals are normal but not homogenous.

A bit of a repeat from above, but with a toned down dataset and codes included to plot the fit of the model on the data.

![](img/332c3a01f0ecf1f46894e7663d9c0cbc.png)

By modelling by parity, I can also fit different curves by parity — as you would expect. However, it is not enough and the fit is quit off at times. Also, for some, the curves are extremely unwieldy, which makes me doubt if daily values are sufficient for such a model. Nevertheless, the dataset on daily values is the most informative, so lets stick with it and build more advanced models.

Code to create calibration plots.

![](img/63789ff1bac5dcd32d42a53c035cacbc.png)

And a confirmation of what we already saw earlier — the models predicts well globally but is complete off when you zoom in towards a more granular level.

在构建模型时，我希望 id 之间有尽可能多的差异，因为我可以将这些差异建模，并将其包含在内。包含方差通常有助于预测，但对解释没什么帮助。尤其是在处理纵向数据时。我们已经看到，我们可以通过将奇偶校验作为四个固定因素中每一个的虚拟变量来提高预测准确性。现在，我们将添加随机截距和斜率，允许参数在奶牛水平上移动。因此，我们将把我们的非线性模型转换成非线性混合模型(NLMM)。关于 [NLMM](/@marc.jacobs012/non-linear-mixed-model-in-r-a6fc054c3f82) 和[随机效果](/@marc.jacobs012/randomness-in-mixed-models-8b7affd8a7c4)的话题已经在我之前的帖子中讨论过了。

首先，我希望并需要创建一个分层数据集，这可以通过使用 [**nlme 包**](https://cran.r-project.org/web/packages/nlme/nlme.pdf) 的 *groupedData* 函数轻松实现。在这里，您可以看到我指定了一个数据集，在这个数据集里，我将奶牛产奶的天数嵌套在胎次内。因此，我们有一个三级数据集— DIM(0)，Cow(1)，Parity(2)。最后，我获得了一个数据集，nlme 包会自动将它识别为具有我希望它识别的特定属性的分层/嵌套数据集。

![](img/5bb8d4595890000c1d047ef262bd1d5e.png)

Summary of the data

![](img/8618fb66e3169d7d952dbb0b362a81b7.png)

Structure of the data

![](img/b1cb1d2d810acebf1ea56ffd463b0146.png)

Nested structure of the data.

Lets plot the dataset.

![](img/f04481f80acefef8edb7d452dcee1e3f.png)

Some Cows have no reason to be included in this dataset

所以，现在是时候通过包含随机项来扩展非线性模型了。赋予混合模型[的所有假设也适用于非线性混合模型。](/@marc.jacobs012/introduction-to-mixed-models-in-r-9c017fd83a63)[这意味着随机项，如误差项，需要表现出同质性和正态性](/@marc.jacobs012/randomness-in-mixed-models-8b7affd8a7c4)。下面，我指定一个模型，其中包括边际固定效应(不是通过奇偶估计)，以及对 *a* 和 *b* 的条件随机效应。数据集有足够的观察值和方差来进行分析。起始值来自旧的非线性公式，有助于混合模型更快地找到最佳值。

Code to create and asses a Non-Linear Mixed Model.

![](img/4008c57eea6baae03c49b466e128aeec.png)

We have four fixed effects and two random effects across 350k observations coming from five parities and 1460 cows. The fixed effects mimic the effects obtained by the non-linear formula. The random effects are for sure viable, and show a certain amount of correlation (-0.51).

![](img/3fbdaf7be9f6da731cbe3125b8cd458d.png)![](img/9ea6b29a918fd40d0d33abe2fffd755a.png)

Residuals look OK overal, but are messed up when you look by parity which is where it really counts.

![](img/6749df78e472dfb73ed6a2208ca3d774.png)![](img/75788e95588ad1f476670b189a00b872.png)

Random effects estimates by Cow ID and Parity. This is automatically generated because the model we specified looks across parity. You can clearly see that the random values shift across cows and parity, although only parity one and two really stick out. Afterwards, they start mimicking each other.

![](img/a1d9f974ad920358cf6092fb2dbd3a31.png)

The intervals function provides approximate 95% confidence intervals for the fixed and random effects, including the variance and covariance components. This is important information to have as it provides a good glimpse of the underlying variability of the data.

很好，现在我有了我的基准模型，可以和我所有的新模型进行比较。因此，我会慢慢地开始建立更多的模型，在我进行的过程中加入更多固定的和随机的效果。下面，一个新的模型，其中我添加了一个额外的随机组件— *c* 。

![](img/61ebfe487eb268d9ae1db444fb5a5d6b.png)

The random components are for sure correlated, meaning that there is quite a heavy Cow-specific effect going on.

![](img/c7856e40c8252794a0a05ac4e521a69d.png)

Before diving deeper into any of the visual and statistical model diagnostics, I want to see if my newer model is a better, more parsimonious model. For this, I chose the ANOVA function. The results are crystal clear — **including a random effect for c** creates a better model!

现在我知道我的新型号是更好的型号，我可以进行评估了。

![](img/b58fdf00e44a6b86c502f44e2ecd6b38.png)![](img/d3b99d642c4acbab7a22da2a0e05335f.png)

Residuals make more sense, but are still not great at the parity level.

![](img/32bd03cd2ae865982d34597a90de640f.png)![](img/4d9272b996de22d9dd0f7a6cc92c870f.png)

Random effects for cow and parity. For sure, the random effect for c is warranted. The first parity stands out amongst the rest.

![](img/4559e89ce8eac9bb59fe2aed87977ac4.png)![](img/d9ad654b70bafa65bb5e2944a9c038eb.png)![](img/b088cd14898a6434c2523ba65d0194b8.png)

Plots for autocorrelation and looking at residuals by parity and cow. There is for sure a presence of autocorrelation, but the residuals seem to be equal across groups. To some extent.

![](img/9fef816211699a95f698fce903c2f4b9.png)

Heatmap of the fixed effects variance-covariance matrix.

让我们仔细看看这个模型是如何预测的，将预测叠加在观察结果之上。因为我们有缺失的数据，并且模型忽略了这些数据，所以我们也需要进行一些数据争论。

![](img/0f19b94b980acf2209b99418b85759bc.png)

For sure different predictions based on what level I predict — marginal (fixed), parity, or cow. The cow-level is the most specific model, looking for cow-specific effects.

![](img/218f21a67b003b57304c665e24eb3d28.png)

And the plot showing the predicted and the observed, overlaid. Although the model surely follows the pattern of the data, it cannot — by nature — deal with any of the perturbations.

![](img/0c742e0eab585d19e0e7d1bcb1263444.png)![](img/39f17b9bdfb9d9c93fff24bba793ec37.png)

Calibration plots. Although the overal plot left seems to show some nice trends, those calibration plots become sobering when shown per cow.

Code for plotting the residuals

![](img/d7710044bba33c5f369469e03c1e7dc7.png)![](img/071dc0474ac135ec3aa7d1877d45c0c3.png)

Residuals are best for the Cow model. However, the long tails should not be underestimated.

我将开始包括一些宇称特定的固定效应，因为我们知道这些是有影响的。

![](img/7340dc229cce51a94e0a1d2f5ca4f845.png)

Output starts to get bigger, fast.

![](img/ba97bc5dca1d51b8d00cf21616872e1f.png)

New model is significantly different, but not that much.

![](img/141ad15116be5dfc6b1b83b0b394d4b7.png)![](img/f1955987da60d4b562c3bf93a35ead2f.png)

Residuals have not improved much.

![](img/d77816ecf4ac34bbd708fcef8fd9cda7.png)![](img/4b33630536ec1560827c138bfdbb4e11.png)

Also here, pretty much the same.

![](img/bbf5de947cdbdaafc9f4bb2db9397a40.png)![](img/b2ad1d5c5cb8d8d9658a025f1ceb0cc0.png)

Still autocorrelation present.

![](img/b0bc41b22bba90730ecaecfabf2720ed.png)

The random effects for the a, b, and c. Quite some spread showing there that there are quite some cow-specific effects going on. Exactly what you want to see.

![](img/d12fa8f51d34be80069df5e50fd8307d.png)![](img/2639e04739b2996522742da9cc3c5174.png)

The last model, **fit3**, is better than the rest.

The most advanced model so far. We can expect a large summary output from this model.

![](img/6f80631f3352060e44fc3c36f35bf049.png)

And we got it — a very large summary output.

![](img/3cc7f55ecd8e04e53bae4f2faff5337b.png)

And this model is also better, a lot better!

![](img/217cdb286d276feb9451efc01bc3db3d.png)![](img/9427ec0fcc74364c554d1ca56077917b.png)

Residuals start to look better and better, for are still much broader then what you would like to see.

![](img/6bbee24596ae111a6ff3700a1f7366cb.png)![](img/fa3562607268479f4db592f0d62f7a68.png)

For sure difference in effects at parity and cow level.

![](img/3902a3b4ee55dbe4226ed8579a0fd9d2.png)![](img/7e00ce026673a0a05bb3e0d798410034.png)![](img/55153a2cc1f5dc2dc8334288221ef918.png)

We still have auto-correlation, but the residuals look quite good at the parity and cow level. With some very big outliers, even when applying a model of this size.

![](img/26c607239ecc7cf6d11248698e2000f0.png)

A heatmap showing the relationship between the fixed effects.

![](img/944e7872d04847ac6e96f1d09c33cbd0.png)![](img/7ca569f9ad1d617c456c0fd473af14e4.png)

Heatmaps showing the relationship between the random effects per level of the random effect. Left: shows the estimates of the random effects for each of the levels of Parity. Right: shows random effects for each of the levels of Cow.

下面的预测看起来不一定比之前的模型更好。还要注意，固定效应不再适用，它与奇偶模型完全相同。这是因为数据集的结构—它与 Cow 一起嵌套在奇偶校验中。通过询问一整套特定于奇偶校验的固定效果，嵌套成为模型的一部分。

![](img/1eec7958bc11f13ddb62d12c1073feef.png)![](img/14f01325edeb2674fd2b10223742d8b7.png)![](img/304d7126dccbd8c15c7f6beeece57970.png)

Calibration plot below left shows how messed up predictions are, still, at the cow level.

这项工作的真正考验是将模型应用于原始数据，并观察它的表现。

![](img/7b6eaa0ff69811e8505865068586c290.png)

A random cow, not in the model yet. You can see, only fixed / parity model is built but not a cow model, since this cow is not in the original dataset.

![](img/27fae714b162a30cdc731312e707ba50.png)

The Wood model as imputation model. Although the model imputes the data in a biological logical line, it cannot impute the perturbations taking place.

为此，我们需要使用完全不同的模型。或许和这个模型一起。但这是另一个时间和另一个职位。

希望你喜欢它！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)