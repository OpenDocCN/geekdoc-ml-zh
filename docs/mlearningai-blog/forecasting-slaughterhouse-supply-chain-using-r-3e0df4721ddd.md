# 用 R 预测屠宰场供应链

> 原文：<https://medium.com/mlearning-ai/forecasting-slaughterhouse-supply-chain-using-r-3e0df4721ddd?source=collection_archive---------0----------------------->

大多数人会自动将数据科学与预测联系起来，从而与预测联系起来。虽然分析数据的艺术和科学不仅仅是纯粹的预测，预测也不等于预测，但它确实是你应该熟悉的支柱之一。

在这个例子中，我分析了一个包含屠宰场猪供应链的时间序列数据集。目标是“准确”预测动物的最终体重。如果这种预测能够实现，公司将能够更好地预测何时发送动物。

最终，由于优先级和业务需求的变化，项目被暂停，但是数据和分析是一个很好的案例研究。尽管许多人期待看到由预测算法产生的代码、技术和图表(是的， [Prophet](https://machinelearningmastery.com/time-series-forecasting-with-prophet-in-python/) 包括在内……)，最好你还记得这个数据科学项目被叫停了。

这是有原因的。项目可能会被搁置，不管你提出的算法有多先进或多有创意。有时，项目耗时太长，业务需求和/或预算发生变化，或者冠军离开。不管项目的潜力是什么，这些发展对任何项目都是有害的。这里指的是积累数据的速度、数据的价值以及不切实际的商业案例。要求根据三年的历史数据提供 10 个月的预测是不现实的。

无论如何，所有的背景知识不应该损害分析数据的乐趣，但它应该正确地看待这个数据科学项目(以及为什么没有项目实际上是数据科学项目，但我将留待以后讨论)。

享受表演吧！

A lot of libraries on this one, although most are not necessarily needed. [**Forecast**](https://cran.r-project.org/web/packages/forecast/forecast.pdf), **caret**, [**timetk**](https://business-science.github.io/timetk/), [**tseries**](https://cran.r-project.org/web/packages/tseries/tseries.pdf), and [**TSstudio**](https://rstudio-pubs-static.s3.amazonaws.com/464590_529f604d55674bd3a046d7e76f862a1f.html) do most of the heavy lifts on the forecasting part.

Then its time to import, wrangle, and diagnose the data using **dplyr** and the [**skimr**](https://cran.r-project.org/web/packages/skimr/vignettes/skimr.html) package.

![](img/ea091c1d745779a05269947d4dba40c3.png)

We have over 4000 observations and 60 variables, but the most important variable is always the time window, which shows 3 years of data. That is not a lot if you want to be able to forecast 10 months ahead.

![](img/459dd3e194e15b9c8e768a368e6b333a.png)

Lots of variables with right skewed distributions, pointing at some weird cases, and / or a need to normalize the data.

![](img/68ab3845b924e9e79beee2d03b7e107d.png)

The last variable is NOT the time-window, its actually a time-metric showing when data was pulled into the database.

Early plots to get a feeling of the data.

![](img/fc7f210aa91ee94300df62f4459920b9.png)![](img/39d0c2619751f13f2f53ce9a7b32b689.png)![](img/5106e427856d391236ba487aac2ce37a.png)![](img/6b5c82947c4f935287e0c5191bf946b8.png)![](img/0ba6747f0fd569929a3569a9039af4c2.png)![](img/fd3370b173ffc515bdd3e58d85514390.png)![](img/3cd57cb62d0e838883a96622c220ff5e.png)

The data itself contains a lot of variables that contain some heavy outliers, probably even mistakes. The dataset is mostly complete.

Lots of code to produce lots of plots across quite some variables. Best to always start by checking if you can recreate biologically known relationships. If not, you have just obtained your first hint that something is fishy about the data.

![](img/528c2d5acaf385d88a102642c3684817.png)

preProcessing via the **Caret** package shows that many variables need to have some form of normalization applied to them.

![](img/f483f1114ead79199814232e123add46.png)![](img/f622f692f8b4cf128d5401b655eeb7b6.png)![](img/d2adba9b0e404244981a6a42200f6aba.png)![](img/8d7cab262e8b5b374b99507a855a2170.png)![](img/d0c43acae8621b414a43f8a2dc1405d7.png)![](img/bd6f5ca8b90cd46ebc41c2bc9500af33.png)![](img/5d13d1279fd222ea62c5d7d1fd4dc260.png)![](img/30b5977f0de01155f588987d3a1ed651.png)![](img/20c1c7b47a0948604f3ed97813c160c2.png)![](img/16f2a79e835a4d9d19e0b91018944b01.png)![](img/a02492919ef648bf9c0f1ab35ee4c9d6.png)

Several plots to look for interesting relationships, but also to just get some understanding of the dataset. Plotting the data, looking for confirmatory biological processes and perhaps strange anomalies can take quite some time and should never be discarded, or rushed. Building the algorithm itself is peanuts, but dangerous peanuts, if you do not know what should be included and why.

我没有直接研究几个预测包，而是进行了一个线性回归，其中包含了与生物学相关的变量。预测将针对最终重量。由于最终目标是及时预测数据，而时间是一个臭名昭著的自私变量，通过交叉相关，我还将要求在正常残差输出的基础上绘制自相关图。

![](img/60dd9e05352b256ea9016529a05dee80.png)![](img/0e44db95771397b0d9000f26a4dd76e9.png)

Residuals are not that bad, considering that the model only contains a couple of variables in an inherent heterogenous process. Autocorrelation is also not extreme. The first bar in any autocorrelation plot can be discarded — it is the correlation value of a variably with itself which should be 1 of course.

![](img/ad5e4b29bf3830b7d4a66704fbcdd81d.png)

The calibration plot could be way worse, although — as always — the prediction error increases as the weight of the animals (and thus their variance) increases.

接下来，我想从时间序列的角度更深入地研究这些数据。请注意，我刚刚展示的线性回归不包括任何时间序列成分——时间数据是按*年*和*月*分类的。这对更传统的时间序列算法不起作用，例如[指数平滑](https://en.wikipedia.org/wiki/Exponential_smoothing)或 [ARIMA](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average) 。

Datasets created using the **dplyr** pipe and **timetk** functions. Extremely easy for aggregating data across time & variables.

![](img/480157450534efcbd07fea157ec46b1d.png)

Dataset grouped by month, rearing type and if the animals were castrated or not. Values are mean values by grouping factor.

![](img/4da2e5cd8b5817c6fc291c0f521bc993.png)

Correlation plot of the **timetk** produced dataset — shows some interesting correlations that could come in handy when applying Vector Auto Regressive (VAR) models.

使用同一个管道操作符，我请求异常检测、季节性诊断、时间序列分解和时间序列回归，但是现在是基于每周数据。这些指标可以产生大量的信息，它们的价值并不总是一目了然。无论如何，所有这些只是熟悉数据和内幕的漫长而艰辛的过程的一部分。

![](img/1454d8711a80ec851b360654a9b1481f.png)![](img/4c342229e9005cb7c91e0b3be20ec1cf.png)

Several anomalies detected for the groupings I made. Perhaps more interesting is that some groups do not even have enough data to allow time-series forecasting. The plot to the right shows seasonal break-up.

![](img/412a6c01eb5c276714ece351ac17b3ba.png)![](img/bedd2fb71c97f778af0343b365cc6b77.png)

Plot to the left shows time-series decomposition. Plot to the right shows an attempt at time-series regression. Although the left plot seems immediately informative (see differences in slope and seasonality), it is perhaps the right plot that is most intriguing. Despite being an overall plot, the regression line seems to hint at recur rig events.

在下面的代码中，我将重新尝试我之前做的线性回归。数据没有被标准化，因为我想保持原始规模的预测(我知道，我可以使用 grand-mean 和标准差进行转换和反转换)。与第一次尝试相比，我添加的是交叉验证和逐步回归。选择工具是评论的食粮，但我不是在寻找一个最好的模型。我只是在用交叉验证的方法寻找这个模型的表现。

![](img/5c1df258a437ac578f7588ac6c521aa3.png)![](img/bdbdd34442e55f1279d09c6bab09e636.png)

Plot to the left reaffirms the need to normalize data. The variance plot hints at several factors that might be important, but unclear is why, how, and via what route.

![](img/5c49aa8f4d206402926018a0bf769a60.png)

The calibration plot showing predictions on the test dataset could have been much much worse, considering the scaling of the data. Actually, most predictions only start getting messy once the bodyweight exceeds 150KG. Nevertheless, much could have been done to improve the model.

为了测试来自这个模型的预测是否有意义，我制作了一个虚构的 N=1 数据集。

![](img/e35570c6c256366b054036c55a58c7b1.png)

Looks normal.

让我们看看在构建更大的数据集时这是否也有效。请记住，逐步线性回归模型中使用的数据集是在批处理级别上操作的，没有聚合。因此，为了进行未来 6 个月的预测，我创建了一个引导数据集(单次迭代)来应用回归模型。

下面代码片段的最后两行显示了构建时序( [ts](https://stat.ethz.ch/R-manual/R-devel/library/stats/html/ts.html) )对象的最初步骤。

![](img/dd46ff2648d879175b7e724677d2387b.png)

The simulated dataset for prediction

![](img/d0fdaba1110a8eccee932b6ae54f8f02.png)

The predictions over time.

![](img/1470f9c189802ce15bd12d1b006d5d08.png)![](img/fc3732c6585a5d4bc19505d89c06610e.png)

Preparatory steps to finally dive into the traditional forecasting models.

最后两张图显示了深入研究数据的准备步骤，并开始添加季节性因素，这是线性回归仅包含时间*月份交互无法真正做到的。

首先，我创建了一个[季节性天真](https://campus.datacamp.com/courses/forecasting-in-r/benchmark-methods-and-forecast-accuracy?ex=2)模型。顾名思义，它是天真的，因此它将使用 2020 年 6 月预测 2021 年 6 月的值。考虑到时间序列数据中季节性的规律性存在，这是一个具有一定相关性的简单模型。事实上，在没有趋势或潜在的不规则性的情况下，季节性的天真根本就不是天真。然而，为了真正看到它有多天真，我将它与指数平滑模型进行了比较——普通版本和袋装版本。此外，我想通过自举重采样更深入地了解数据的内在差异。

![](img/fd248be4ea158ae74c5cdff0444e0b0b.png)

The seasonal naïve model will forecast July to December 2021 by forwarding the respective values from 2020\. Hence, a forecast for September 2021 will equal the observed mean for September 2020.

![](img/feb276265da32f33646f54df4c5867eb.png)

The bootstrapped samples already hint at larger than comfortable prediction intervals for any model that I will make.

![](img/d7ceb7b2221f3828b68f7459859e8eb7.png)

As suspected, all three models are really heavy on the 95% Prediction Interval. In fact, these intervals are so wide that all three models would except a naïve forecast.

![](img/0236526bd3ba53fc2ee78206e210e9bb.png)

Comparison of several models — Bagged ETS (BETS), Exponential Smoothing (ETS), Linear regression (LR) and Seasonal Naïve (SN). Perhaps it would be best to just ensemble all three models. Even better would be to recognize that the granularity of the data does not allow long forecasts.

这篇文章的下一部分将更多地关注预测的技术部分，尽管这绝不是一个详尽的部分。关于一般的预测，特别是几种算法，已经有完整的书籍问世。所以，让我们看看什么是更有可能的。请记住，这一部分的目的是强调一些技术上的可能性，而不是指出我所做的事情是否有意义——我们已经看到数据不太可能足以回答业务问题。

我将再次从一个简单的线性回归开始。此外，我将通过 [Dickey-Fuller 测试](https://en.wikipedia.org/wiki/Dickey%E2%80%93Fuller_test)请求关于单位根存在的信息。有了这些信息，我可以筛选数据的[平稳性](https://en.wikipedia.org/wiki/Stationary_process)(一段时间内的恒定均值和方差)。此外，为了通知建造者一个 [ARIMA](https://machinelearningmastery.com/arima-for-time-series-forecasting-with-python/) 或 ARMA 模型，我还将请求[自相关](https://machinelearningmastery.com/gentle-introduction-autocorrelation-partial-autocorrelation/)和[周期图](https://en.wikipedia.org/wiki/Periodogram)图。不要以为最佳实践需要迭代方法，而不是我在这里展示的单一方法。因此，首先要评估数据是否稳定。然后，通过从原始数据中减去 Lag-1 值来区分数据，然后寻找自相关。

对于那些想知道我们为什么关心数据是否稳定的人来说，答案很简单——这是 AR(I)MA 模型所固有的，其中 I 实际上意味着“集成”,这意味着差分，这意味着转换数据以获得稳定的形式。

所有这些都是基于非平稳数据无法预测的假设，但事实是许多模型并不需要平稳数据，比如指数平滑模型(Prophet 模型就是从它衍生而来的)。

Code to preprocess the data, plot the data, regress the data, and request tests / plots assessing the usability of the data for ARIMA modelling.

![](img/d1c99c68b979c3705dd1d8dcfaf9fbc4.png)

The p-value is 0.1433 which in traditional Frequentist reasoning means that we cannot reject the null-hypothesis of non-stationary. Hence, the data requires differencing. Nevertheless, the p-value is continuous and not binary, and a value of 14% does not scare me. For the time being, I will leave the data as it is, especially since other models do not require differencing. In fact some of them — like the Prophet model — will get hindered by a lack of patterns.

![](img/05e18083ab3d5484fde8fd7b1b6cddec.png)

Plots show that the residuals coming from the model are not normal and not homogenous. Heterogenous data can be modeled, but not using THIS particular model.

[ACF 和 pACF 图](https://people.duke.edu/~rnau/411arim3.htm)非常方便，但不直观，可以帮助您指定 ARIMA 模型的 AR(自相关)和 MA(移动平均)部分，这是所有预测模型的教父。这两个图显示了残差之间的时间相关性。我认为使用以下[站点](https://people.duke.edu/~rnau/411arim3.htm)的信息可以最好地解释如何解释 ACF 和 pACF 图:

> ACF 图仅仅是一个时间序列与其自身滞后之间相关系数的条形图。PACF 图是序列与其自身滞后之间的偏相关系数的图。部分自相关是变量和滞后本身之间的相关量，它不能用所有低阶滞后的相关来解释。

pACF 用于指定 ARMA 或 ARIMA 模型的 AR 部分，ACF 用于指定 MA 部分。在模型代码中，那分别是 *p* 和 *q* 超参数。如今，许多库允许筛选会话在所有可能的 AR(I)MA 模型中迭代，以指定最佳模型，这使得该过程有点像黑箱。在 ACF 和 pACF 图的下方，你可以看到周期图。这些图清楚地暗示了区分数据的需要，应该成为你的指导原则——而不是增强的 Dickey Fuller 测试(想想 [Kolmogorov-Smirnov](https://en.wikipedia.org/wiki/Kolmogorov%E2%80%93Smirnov_test) 测试)。

![](img/804c57e7e319676043a910f87d65d2ba.png)

ACF, pACF, periodogram, and cumulative periodogram results. The ACF and pACF plots cannot be interpreted in this form, and the data needs differencing first.

尽管我们知道模型需要有所区别——至少要符合 ARIMA 模型的要求——但我们可以应用没有这些要求的简单模型和季节性简单模型。事实上，这两种模式都要求很少。

Code to use, plot, and compare a naïve and seasonal naïve model.

![](img/916cb3f808aff2ea00fd6c5a2b728eb4.png)

Forecasts from June 2020 on, based on two models. The forecasts coming from the naïve are ridiculous. The seasonal naïve are not that naïve. However, in the end, the naïve actually performs better.

![](img/dc9932b6564afea6e288096d9897a389.png)![](img/62627ae4fa4a43b44bb989b7661ec0d8.png)

Accuracy metrics for the naïve (left) and the seasonal naïve model (right). Based on the RMSE, the naïve actually performs better.

让我们应用全部的[指数平滑模型](https://machinelearningmastery.com/exponential-smoothing-for-time-series-forecasting-in-python/) (ESM)。这并不是要挑选最好的一个，而是要了解他们每个人看待数据的不同方式。尽管 ESM 模型开始时可能非常简单，但它们可以变得复杂得多

一般来说，ESM 模型有三个可以建模的部分——误差、趋势和季节(使用加法或乘法“变换”)。对于季节性，可以采用许多其他方法，如霍尔特-温特斯法。

现在，我只是应用了整个套件，但是更好的做法是理解每个模型，并考虑其潜在的假设和适用性。

![](img/d04311bc52619e4f94cf8b7644294ca4.png)

Some forecast via a slope, others try to use the ‘pattern’ in the data.

![](img/ee6430d6431f497d4a3e96fdcf6b7a2c.png)

Output from an automatically selected ESM model. It is model with a multiplicative error part, an additive trend, and no seasonality included.

![](img/88c307b52cc95406135c95fc2c6be553.png)![](img/a58c8c65099c9cb42dc08370063803e0.png)![](img/21751049fd6b4dedbd843fcb54350968.png)

Here you see the ESM(A,M,N) and its bootstrapped forecasts. That is quite a range.

Code to cross-validate models and compare their errors.

![](img/05b9c3b8ddccd25371bcf9a33176c6a8.png)

The first model, which is just a simple exponential smoothing model, beats the other more complex models.

现在是迈向 ARIMA 模式的时候了。由于我们懒惰且被宠坏，我们要求模型在模型网格中运行，并得出最‘合适’的一个。

Library scripts make it look like child’s play to create an ARIMA model. This is also one the biggest setbacks of the past years — it all becomes way too easy.

![](img/3d7a271d3ab9cc558e5dc328c3a42698.png)

ARIMA (0,0,1)(1,0,0)[12] means that the algorithm selected a model with q=1 and P=1 in which seasonality was included via 12-month period.

![](img/eb4ea29e60bfdd87ccad3ca2284e607b.png)![](img/bceff8ba964cb07dd8b2214647dde082.png)![](img/e77a9291e3edfbc29df9c9b4a0c9b3b1.png)![](img/3504f44f18843d158737a6eb0e779515.png)

The ACF and pACF look good and well within bounds. Also the residuals look normal. As such, the predictions coming from the ARIMA are not that strange. What is surprising is that no differencing was applied in the end by this automatic model, even though the Augmented Dickey-Fuller test did seem to hint at it. Goes to show how tricky cut-off for p-values are.

下面你可以看到研究向量自回归模型(VAR)的代码。这些模型专门用于模拟多元时间序列。尽管数据集本身并不难制作，而且模型也确实运行了，但风险值模型无法产生任何合理的预测，这是一个明显的暗示，表明有些事情不太顺利。

我将在另一篇文章中使用更合适的数据——使用 SAS——强调 VAR 建模，但如果您有此打算，至少您可以看到代码来自己开始。

当然，我们也可以模拟数据并应用指数平滑法。

![](img/07c6cd2d255ed8a4b6a47aae7af14be8.png)

Simulated timeseries.

![](img/bad13fdd8b06c48dc061b89d0429423a.png)

Exponential Smoothing models based on the original and simulated data. Does not really make a difference.

我们还可以使用 bagging 技术来构建 ESM 模型。引导和打包都依赖于模拟的(引导数据集)。他们只是处理可用数据范围的方式不同 bootstrap 使用数据范围来构建预测范围，而 [bagging](https://en.wikipedia.org/wiki/Bootstrap_aggregating) 则从 bootstrap 中积累数据。

![](img/9ccf8a7b959dda66d0f6f93d7056e506.png)![](img/977bc910cdf8f9e94bf1375e28d2bb03.png)![](img/a082dc1cfc0a59323545ae829b67a324.png)

Bagged ESM has a much smaller Prediction Interval! This is due to using the simulated data via an ensemble method.

最后，但同样重要的是，让我们通过一个很好的旧的训练测试分割来比较几种算法。

![](img/0ee449e2dfc4e2c6f66f11e7709704a9.png)![](img/774385e343181ca270bd328a624ac824.png)

We fit five models — ARIMA, ESM, PROPHET, Linear Regression, and a [MARS](https://en.wikipedia.org/wiki/Multivariate_adaptive_regression_spline) model which is an additive spline model.

![](img/22e8e638cf9a6062700cb92c9e44415a.png)

The MARS model was the best fit, but the Linear Regressions should for sure be noted. More advanced does not mean better. The R² metrics do hint at the sparsity of the data.

![](img/5455d8a60464675ce416cd6f8dc758d7.png)

First forecast

![](img/1bd06a4ca28931900ac287984fba9835.png)

Refit forecast which does not include the same date as the one in the original plot. The refit part of the code is especially handy as new data comes in. Here, it was just to highlight how to use it.

好吧，那是一篇包含了各种预测和预报的文章。和往常一样，这不是一篇详尽的帖子——尽管你现在可能感到筋疲力尽。记住，以上都只是工具，只是分析数据的一小部分。最后，研究/商业问题必须有意义，数据必须能够提供比以前更多的知识。

如果你发现了错误，新技术，或者只是想练习，你知道在哪里可以找到我！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)