# 模拟生长肥育猪的体重

> 原文：<https://medium.com/mlearning-ai/analysis-of-bodyweight-in-grower-finisher-pigs-using-mixed-models-splines-and-bootstrapping-in-r-4a997a04f77d?source=collection_archive---------2----------------------->

## 在 R 中使用混合模型、样条和引导

这是我在 2017 年做的一个分析。尽管现在是 2021 年，我不得不重新评估所有代码，看看它们是否仍然有效，但考虑到数据的性质及其潜在的生物学，我相信我很难比当时做得更好。

该数据集包含对 300 多头生长肥育猪的观察，分为两个处理，为期 168 天。在产仔、哺育和生长完成阶段总共进行了 11 次测量。实验设计不得不重新洗牌，导致两个随机完全区组设计。

这个数据集的难点在于，猪有很强的能力来弥补过去的生长中断。此外，随着时间的推移和体重的增加，差异也会增加。严重地。此外，围栏内和围栏间发生的事情变得越来越重要，往往取代了治疗的影响。

从统计学的角度来看，这是一个很棒的数据集，其中再次强调了使用[](/@marc.jacobs012/introduction-to-mixed-models-in-r-9c017fd83a63)**[**样条**](/@marc.jacobs012/analysis-of-bw-in-piglets-using-mixed-models-splines-in-r-a7c0952b15fc) 和 [**自举**](https://www.statmethods.net/advstats/bootstrapping.html) 。一如既往，我相信我在这里展示的东西可以大大改进。**

**尽情享受吧！**

**As always with R, the more functionality you want to use, the bigger the list of libraries becomes.**

**Import of growth dataset and transformation in long-format to allow longitudinal analysis.**

**![](img/dfc276e3ae1b4da51944e3d58905d787.png)**

**How the data looks now — one row per observations.**

**Centering of time-varying coefficients to enable better convergence. Does not change model diagnostics, but can hinder interpretation, since everything is now subtracted from the grand mean. Do note that I am not scaling here, just centering.**

**![](img/0070943b76306febcfa62a969e274e30.png)**

**The dataset after centering.**

**Creation of time-varying variables, and assessment of deleting some perhaps strange observations.**

**![](img/02fe762d51726bba8f2515d116bda208.png)**

**Adding time-varying variables.**

**MELTING data to allow drawing of certain plots.**

**![](img/eeef7dec81ffa91e371b637f0b1bdf20.png)**

**TREATmelt dataset, which shows per treatment and time some key metrics. Note the difference between means for the treatments over time.**

**![](img/f7d1e4543bd857fc4cd561a05061aa05.png)**

**Deleting some animals will lead to differences in estimates. If there is no real biological reason to delete observations or complete subjects, I would always advise against it.**

**The code to draw plots that follow after extensive data wrangling.**

**![](img/762168d6d63f5013054859f20d1c0a6a.png)****![](img/5f3041cba128e188023ca26145a8e431.png)****![](img/b18183eade67808eefae943fa8c04669.png)**

**Plots showing the variation within and between treatments. Image by author.**

**![](img/84c6d8625889ffc150fe9447488654a8.png)****![](img/7e973ab15d9e21cf5129e4c33adb49c6.png)****![](img/f59444fbfe7a2ce238f84920fb144547.png)**

**Plots showing variation between sows, blocks, and across phases. Image by author.**

**![](img/72100a279c724e3baad31f3f5754ae41.png)****![](img/830cd2b6977fa849b0527738513182e9.png)****![](img/e0835d32fc860a84e8b34f980a4aea67.png)**

**Plots showing variation dependent on potential important covariates. Image by author.**

**Code to assess if a linear or polynomial / spline model is warranted. The previous plots already gave us a pretty good idea about what to do. Image by author.**

**![](img/8f472cf0303e46b4754b885a304333f8.png)**

**Plots of polynomial degrees shows that a quadratic regression should suffice. Image by author.**

**到目前为止，代码和结果应该非常简单。我们加载、争论和绘制数据，以了解我们正在处理的是什么。我们确实有足够的观测数据，但是方差越来越大，使得数据建模变得不那么容易。让我们深入探讨一下。**

**Code to look at variation across time in three ways.**

**您可以在下面看到，所有时间点的数据分布似乎都是非常高斯的，但自由标度可能很棘手，因为接下来的两个图显示的是方差几乎呈指数增长。这将使数据建模变得非常困难，尤其是当我们必须通过许多观测数据来构建一个可靠的协方差矩阵来对误差部分进行建模时。因此，为了创建某种程度上稳定的模型，可能要使用 [**Box-Cox 变换**](https://en.wikipedia.org/wiki/Power_transform) (BC)并稳定随时间变化的方差。**

**![](img/43dfeddf5cb3ac02df5c1af14a53e4d9.png)****![](img/5ec9aa8836ab6d8978614b66e79c9d03.png)****![](img/30c21a8435464d804fa6560b9ed88a85.png)**

**Image by author.**

**Box-Cox，或**幂变换**，是一种简单而直接的减少变量内方差的方法。通过利用“幂树”,自动化程序将运行几个变换，并评估哪些变换获得最佳对数似然性。常见的 Box-Cox 变换遵循连续标度的**λ**标准变换。与键转换相对应的 lambda 的值得注意的示例有:**

**2→1/Y2
-1→1/Y1
-0.5→1/(Sqrt(Y))
0→log(Y)
0.5→Sqrt(Y)
1→Y
2→Y**

**我用线性回归来做评估。Once 还可以选择更深入—通过治疗运行它—或者更深入地查看数据集的嵌套级别以寻找合适的转换。为了简化已经变得相当费力的分析，我选择只使用固定效应模型。**

**![](img/7e1a25bcbfa9dcf0a7d5f6bbbf1a5fd3.png)**

**The optimal lambda value was 0.18\. Image by author.**

**![](img/8a383dcfebb7b937ebc2af8d390846d8.png)****![](img/25ac40e44ac653c091eb4335fbb69315.png)****![](img/f901a83ba6fd8bbebfb04e82ac2fc8fb.png)**

**Applying the Box-Cox transformation to the outcome did indeed stabilize variances. Image by author.**

**现在是开始构建模型的时候了，你在下面看到的是过多的模型。我不会深入探究我为什么做这些，因为建模是由科学、统计和简单的试错法驱动的。最后一条评论不是最性感的，但却是事实。很多时候，我甚至不知道我到底在找什么，直到我看到它，通过引导许多模型并观察它们变化的行为，我希望看到哪些部分会影响，哪些不会。由于参数的数量很少，我们可以这样处理它。**

**事实上，你在这里看到的只是一个更大的模型列表中的一小部分。需要注意的关键是，我没有对这些模型应用 BC 转换，因为我想看看它们的表现如何。**

**![](img/62f11594366b95fc30cddeef5c86825b.png)**

**It seems some models for sure perform better than others. The last one is incomparable to the others, since it is on a different scale.**

**![](img/60a3f2d172552276af7dd25cfea6c861.png)**

**Fit 10 cannot be used in an Anova since it is on a different scale then the rest. Actually, by including it here, I am messing up the sequence of the Anova. But I do not like these models anyhow, so I just let it be as for now.**

**好，让我们建立一个新的模型，仍然使用原始的 BW 比例。我在*时间*变量上应用样条，与*处理*交互，添加额外的预测值，请求嵌套在阶段*(1 |阶段/块)*的块上的随机截距，以及仅用于每只动物生长的随机斜率，嵌套在母猪 *(0+ns(Time_center，3)/sownr/pig_id)* 。这是一个值得估计的模型。**

**![](img/1776840413da2e54bebd4903c1cf4903.png)****![](img/c68d68044dc1314583654e0717bb89a7.png)****![](img/630c42db2aa596bbfbb85b13fef87aa0.png)****![](img/a3c97cb34dc182136452714a8a2438fc.png)****![](img/fee99d369b58b9f87c67a8894b6a5bda.png)****![](img/09c233189d00ca5ea1e693df1b03b098.png)**

**Can’t say I am to pleased with the QQ plot nor the residuals estimates for block, pen, and time. The residuals for phase and sex seem to be doing better, although residual increase follows time, as expected. All of this is because we did not apply the variance stabilizing factor as provided to use via Box-Cox. Image by author.**

**到目前为止的结果证明了通过 Box-Cox 提供的稳定因子——λ值的使用是正确的。这是我们在下一个模型中要做的。**

**These models are substantially different then the previous once. We cannot compare them using the standard metrics, because any transformation on the raw data — and not inside the model — will provide us with different metric values. Image by author.**

**基于对一组数据(未在此显示)的额外分析，我们拟合了最终模型。由于在数据上增加了三个节点，模型的复杂性大大增加了。现在来看这个模型，我会说它严重过度拟合，但当时的意图是确保模型尽可能地拟合数据，以便在最终寻找治疗均值和变异系数的差异，确切地说，是在方差最高的时候。**

**![](img/e7d5fefb570d67833514c90b0544358f.png)**

**The output of the models shows how complex it is with many variances being calculated. The random-effects components have standard errors exceeding the point estimates in size. That is not a good sign. The correlation matrix seems to be good. Image by author.**

**![](img/2365fb45e4ebe6453dfb3a500dca1e74.png)****![](img/755d05e7c2c5843098ead93b9c1597f9.png)****![](img/92beb142509faba885a32a8919670e2e.png)****![](img/a7a7c8d1377769b6ad9ae13bd6ed3c63.png)****![](img/cc53d368ae89608c211700fcb906cac2.png)****![](img/5675fe12cfcc78c9d6d67b76a6d71cd2.png)**

**The QQ-plot does not convince me, it shows heavy tails which means I am most likely not including all predictors that I need to in order to explain variance. However, considering the heavy model included, one might wonder how much is left. Random effects seem to be okay, except phase, which can hardly be named a random effect with only three levels. Image by author.**

**通常，在分析重复观测值时，模型确实需要考虑自然发生的自相关——这意味着可以通过过去的观测值预测某个时间的观测值。这会对您的估计值产生影响，因为数据不再是唯一的，所以标准误差会自动增加。不考虑互相关意味着你通过夸大你所拥有的独特观察的数量(在重复观察实验中很少等于收集的观察的数量)而低估了标准误差。**

**[lme4](https://cran.r-project.org/web/packages/lme4/lme4.pdf) 包不能对模型的错误部分建模，但是 [nlme](https://cran.r-project.org/web/packages/nlme/nlme.pdf) 包中的 gls 函数可以。我所做的是让算法在同一个模型上拟合误差部分的所有潜在协变量结构，并给我带回最佳拟合模型。**

**请注意，我在这里没有使用 BC 转换。请记住，它们是为了在无法对误差部分建模的情况下稳定方差。**

**![](img/bd8c55505c389a36c2993d1ea64d296f.png)**

**Model 7 has the lowest AIC, which corresponds to a spherical spatial correlation structure.**

**下面你可以看到这个模型的结果。方差没有得到控制，甚至没有这个模型，这使我相信，除非我严重过度拟合模型，否则我最终将很难处理方差。这正是我感兴趣的地方。**

**![](img/4989851964a76412ad852d31f25d5e17.png)****![](img/f7f3a53d1890224700038fa20e23488d.png)****![](img/089d3ca14a44f64aaf98b4d2bac3f819.png)**

**Image by author.**

**![](img/31122003423aac28fad86f633f73a08c.png)****![](img/bf638eeb1c1e61ff9bbb1a5651093c6f.png)**

**The last plot shows the insane importance of Time according to the Anova function. Time is by itself a horrible variable to include since it is a proxy for change, without knowing what is causing it. Time — by itself — cannot cause anything. Image by author.**

**接下来，我将着眼于潜在的影响对象。也许在这里，我可以找到一些缓解，但对于这样一个复杂的模型，这项工作很容易需要几个小时。除了识别影响的来源，我还发现迭代删除观察值是查看模型稳定性的好方法。如果你的控制台在这个过程中一直显示奇点或无法收敛，你可以放心，你在过度拟合方面做得很好。**

**I identified some animals that might not help me in my endeavor. Lets see what happens if I delete them. Image by author.**

**![](img/a30a6d855a8244b3bb73376bbc7b8ae5.png)****![](img/ccadbb40ac596af3aa8c85cc847feea7.png)**

**Nothing happens, except loosing observational data, so I keep them in. Image by author.**

**Code to plot the predicted values from the final model — back-transformed of course — on the observed data points. Image by author.**

**![](img/cb6e881d11ea02fb17ff1eea949582bb.png)**

**Quite a good fit. An overfitted fit I might add. Image by author.**

**现在，因为我相信我不能做得更好，让我们做一个完整的模型评估。**

**![](img/7b663043f86d0b3839fea567470426d1.png)**

**Residuals look good, except the QQ-plot. The calibration plot is a bit to perfect to be honest. Image by author.**

**最后，但同样重要的是，我们需要开始做出更可靠的预测。所以我创建了一个新的数据库，模仿过去的结构。这也是自举发挥作用的地方。我所做的基本上是重复分析 1000 次，存储处理差异——平均值、标准差、变异系数——并绘制结果。在我当时的电脑上(i5 ),这需要 8 个小时。**

**![](img/4b4c63a096830cbc2bc06fb39103b8e1.png)**

**I fit a model, use the new database, apply the model, store the predictions, and repeat that process 1000 times.**

**![](img/2df66bc99ba8ff7665546efb63f14ff6.png)**

**Bootstrapping looks good. The dataset is ready to go. Image by author.**

**现在，经过 8 个小时的采样后，是时候使用所有不确定性的估计值并绘制处理差异了——平均值、标准偏差、变异系数。**

**![](img/465bb6e900852f1a326398640eaeb1d7.png)**

**BW differences between treatments follow a Normal distribution. There is for sure bias, every now and then. Image by author.**

**![](img/cb2f3f11900837d8b965119f161ef6d3.png)****![](img/3e82bd46df60221363cd5c29f6ca3326.png)**

**Histograms and QQ-plots for the SD show little bias and a good fit. Hence, bootstrapped samples can be trusted. Image by author.**

**![](img/2a98afcec0fd3957ea5aa615aaddfa57.png)****![](img/d14b6bc52c18584c5fd6044a98d78397.png)**

**Histograms and QQ-plots for the CV show little bias and a good fit. Hence, bootstrapped samples can be trusted. Image by author.**

**上面所有的工作——重新采样、建立数据库、检查假设——只有一个目的，那就是绘制下面的图表并安全地解释它们。**

**![](img/80e21c4f0ba8a4709d2cb7169de196ec.png)**

**This plot shows a heavy treatment difference in the middle of the study, which is lost at the end. Image by author.**

**![](img/49f0bbf6bf9a0af8f85890731eb529b9.png)**

**The standard deviation of the difference remains the same across time. Image by author.**

**![](img/c6c86c8d7d58ed27c7bfbb7596261f48.png)**

**The CV of the difference increase exponentially. Image by author.**

**哇，这是相当多的工作来分析和展示。和往常一样，这里所描述的远非完美，它只是分析像这样的数据集的单一方法。**

**如果您发现新的技术可以让分析这类数据变得轻而易举，请随时告诉我！！**

**[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

v**