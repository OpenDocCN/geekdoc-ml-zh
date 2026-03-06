# 机器学习:如何分析模型性能

> 原文：<https://medium.com/mlearning-ai/machine-learning-how-to-analyse-model-performance-f58658a20482?source=collection_archive---------2----------------------->

![](img/31cacba2a42dff22cba207410ec34ca4.png)

我们用准确性来测试我们的模型工作得有多好。事实证明，准确性本身可能会误导人。在本课中，我们首先研究为什么精确度本身不一定是模型性能的良好指标，然后我们将了解模型性能的一些其他度量。

> #为了方便起见在一个地方进行所有导入
> 从 sklearn 导入 matplotlib.pyplot 作为 sklearn.datasets 导入 fetch _ 20newsgroups _ 矢量化
> 导入 numpy 作为 np
> 导入 pandas 作为 pd
> 导入 seaborn 作为 sns
> 从 sklearn.linear_model 导入 LogisticRegression
> 从 sklearn.model_selection 导入 train_test_split，cross_val_score
> 从 sklearn.dummy 导入 DummyClassifier
> 从 sklearn.metrics 导入混淆

# 20 个新闻组数据集

在本例中，我们将使用 20 个新闻组数据集，这些数据集可以通过 scikit-learn 的数据集库加载。20 个新闻组数据集包含 20 个主题的大约 18000 个新闻组帖子。

> dataset = fetch _ 20 news groups _ vectorized()
> X，y = dataset.data，dataset.target

接下来，我们可以计算每个类的实例数量。

> 对于 class_name，zip 中的 class_count(dataset . target _ names，NP . bin count(dataset . target)):
> print(class _ name，class _ count)

```
alt.atheism 480
comp.graphics 584
comp.os.ms-windows.misc 591
comp.sys.ibm.pc.hardware 590
comp.sys.mac.hardware 578
comp.windows.x 593
misc.forsale 585
rec.autos 594
rec.motorcycles 598
rec.sport.baseball 597
rec.sport.hockey 600
sci.crypt 595
sci.electronics 591
sci.med 594
sci.space 593
soc.religion.christian 599
talk.politics.guns 546
talk.politics.mideast 564
talk.politics.misc 465
talk.religion.misc 377
```

我们看到这是一个平衡的数据集。也就是说，每个阶层都相当平均地被代表。不幸的是，我们在现实生活中并不总能找到如此均匀分布的数据。因此，为了查看不平衡数据集的影响，让我们将该数据集简化为两个类:sci.space 和 everything。首先，复制目标数据，然后用 0 替换所有非 sci.space 类的实例，用 1 替换所有 sci.space 类的实例。

要查看此更改的效果，请再次查看每个类的计数:

> y_2 = y.copy()
> y_2[y_2！= 14]= 0
> y _ 2[y _ 2 = = 14]= 1
> NP . bin count(y _ 2)

```
array([10721,   593])
```

现在，我们有一个类别不平衡的数据集:也就是说，在表示与空间无关的帖子的负类别中，我们有 10，721 个实例，而在表示关于空间的帖子的正类别中，我们只有 593 个实例。现在我们可以训练一个分类器来预测一个帖子是否是关于空间的。让我们使用逻辑回归分类器。

> X_train，X_test，y_train，y_test = train_test_split(X，y_2，random _ state = 82)
> lr = LogisticRegression(solver = ' lbfgs ')
> lr . fit(X _ train，y_train)
> lr.score(X_test，y_test)

```
0.9671261930010604
```

还不错！没有任何调整，我们得到了 0.967 的精度。96.7%的准确率相当不错，但在我们庆祝我们的数据科学技能之前，让我们看看这在不平衡的数据集环境中意味着什么。

假设我们写了一个分类器，它根本不工作，甚至不看特征，只是简单地返回所有的预测作为主导类。在这种情况下，占主导地位的阶级就是消极阶级。因此，我们的虚拟分类器将 100%的时间预测负类。虚拟分类器的准确性如何？花点时间想一想。

回想一下

> 精确度= # correctpredictions all predictions 精确度= # correctpredictions all predictions

由于我们有 10，721 个阴性样本，我们的虚拟分类器将正确 10，721 次，错误 593 次。

> 准确度=1072110721+593=0.94759 准确度= 1072110721+593 = 0.94759

还是蛮厉害的！

Scikit-learn 实际上有一个虚拟分类器来帮助说明这个确切的场景。

> dummy = dummy classifier(strategy = ' most _ frequency ')
> dummy . fit(X _ train，y_train)
> dummy.score(X_test，y_test)

```
0.9568752209261223
```

请记住，虚拟分类器实际上并不使用特征来进行预测，它只是查看哪个类占主导地位，并每次都返回该类。这里，在我们的测试数据上，我们得到了超过 95%的准确率，几乎与我们的真实分类器一样好。考虑到这两个结果的接近程度，我们真的有信心我们的分类器做得这么好吗？

这个伪分类器非常有用，原因有很多。它给出了一个零精度基线，您可以用它来比较模型的性能。这样我们就知道我们的分类器是否比随机猜测做得更好。

如果模型的精度接近零精度基线，这可能意味着:

*   你的特征不能很好地预测你的问题。
*   您的算法参数可能需要调整。
*   你有很大的阶级不平衡。

# 混淆矩阵

混淆矩阵只是一个显示预测值和实际值的每个组合的表格。它可以很容易地看到您的模型正在犯什么类型的错误，并允许生成许多描述您的模型性能的指标。Scikit-learn 附带了一个函数，可以在给定一组实际值和一组预测值的情况下计算混淆矩阵。

# 为什么我们要使用混淆矩阵？

虽然具有良好的准确性很重要，但这并不能说明全部情况。混淆矩阵通过向我们展示事物是如何分类的，让我们对分类模型的性能有了更深入的了解。它向我们揭示了哪些数据点是正确的，哪些是错误的。

我们稍后会对此进行更多的解释。

> 预测= lr.predict(X_test)
> 混淆=混淆 _ 矩阵(y_test，预测，标签=[1，0])
> 打印(混淆)

```
[[  29   93]
 [   0 2707]]
```

我们一会儿会解释这个。

虽然这给了我们所需要的所有信息，但阅读起来并不容易。为了得到混乱矩阵的更漂亮的图片，我们可以使用[这个来自这里的函数](https://www.kaggle.com/grfiv4/plot-a-confusion-matrix)，或者任何一个你可以在网上找到的类似函数。

> def plot_confusion_matrix(cm，
> target_names，
> title='Confusion matrix '，
> cmap=None，
> normalize=True):
> """
> 给定一个 sklearn 混淆矩阵(cm)，制作一个漂亮的 plot
> Arguments
> ————————-
> cm:来自 sk learn . metrics . Confusion _ matrix 的混淆矩阵
> target_names:给定分类类如[0，1，2]
> 类名，例如:['high '，' low']
> 标题:显示在矩阵顶部的文本
> cmap:matplotlib . py plot . cm 中显示的值的渐变
> 参见 http://matplotlib . org/examples/color/color maps _ reference . html
> PLT . get _ cmap(' jet ')或 plt.cm.Blues
> normalize:如果为 False，则绘制原始数字
> 如果为 True，则绘制比例
> 用法
> —
> #类名称列表
> title = best _ estimator _ name)#图标题
> citation
> ———
> http://scikit-learn . org/stable/auto _ examples/model _ selection/plot _ confusion _ matrix . html
> " " "
> import matplotlib . py plot as PLT
> import numpy as NP
> import ITER tools
> accuracy = NP . trace(cm)/float(NP cmap = cmap)
> PLT . title(title)
> PLT . color bar()
> if target_names not None:
> tick _ marks = NP . arange(len(target _ names))
> PLT . x ticks(tick _ marks，target _ names，rotation = 45)
> PLT . y ticks(tick _ marks，target _ names)
> if normalize:
> cm = cm . astype(' float ')/cm . sum(axis) format(cm[i，j])、
> horizontal alignment = " center "、
> color="white" if cm[i，j]>thresh else " black ")
> else:
> PLT . text(j，I，" {:，} "。format(cm[i，j])、
> horizontal alignment = " center "、
> color="white" if cm[i，j]>thresh else " black ")
> PLT . tight _ layout()
> PLT . y label(' True label ')
> PLT . xlabel(' Predicted label \ nacuracy = {:0.4f }；misclass={:0.4f} '。format(accuracy，misclass))
> plt.show()
> 
> plot _ Confusion _ Matrix(cm =混淆，target_names = ['Space '，' Not Space']，title = '混淆矩阵'，normalize=False)

请注意，上图方框中出现的数字与 scikit-learn 生成的基于文本的矩阵中的数字相同。但是这个带标签的图更适合用来直观地评估一个混淆矩阵。

# 解读矩阵

但是这些数字意味着什么呢？假设我们训练了一个二进制分类模型来识别有猫和没有猫的图像。我们建立了一个图像数据集，这些图像在一张照片中被正确地标记为猫，而在其他照片中没有猫。然后，我们在一个带有标签的测试集上运行这个模型，测试集上有几幅图像，有些是猫，有些没有猫。一些图像被正确地预测为图像中真正的猫，但是一些猫的图像被错误地预测为不是猫。相反的情况会发生，照片被真正归类为不是猫，但有些照片被漏掉了，照片里有一只猫。

然后，我们可以计算我们的模型正确预测猫、正确预测没有猫、错误预测有猫和错误预测没有猫的次数。这些是我们插入混淆矩阵的数字。

根据上图，我们可以将真阳性(TP)定义为模型正确预测阳性类别(cat)的次数，将真阴性(TN)定义为模型正确预测阴性类别(无 cat)的次数。

假阳性(FP)是模型错误预测阳性类别的次数，假阴性(FN)是模型错误预测阴性类别的次数。在统计学中，假阳性被称为 I 型错误，假阴性被称为 II 型错误。

这意味着真阳性和真阴性是正确预测的总数，而假阳性和假阴性是错误预测的总数。

从这个矩阵中可以计算出许多重要的指标。第一，准确性:

准确度= TP+TNTP+TN+FP+FN = 29+270729+2707+0+93 = 0.967 准确度= TP+TNTP+TN+FP+FN = 29+270729+2707+0+93 = 0.967

## 精确

精度是一个值，它告诉我们有多少比例的正面预测是正确的。也就是说，如果模型的精度为 0.5，那么其 50%的正面预测是正确的。让我们计算模型的精度。

精度=TPTP+FP=2929+0=1.0 精度= TPTP+FP = 2929+0 = 1.0

当我们的模型预测一个帖子在组 sci.space 中时，它只有 24%的正确率！

如果不看精度分数，我们会因为简单地接受表面上的精度分数而错过这一点。

## 回忆

回忆是一个值，它告诉我们正确预测的阳性比例。这是一个很好的指标，表明有多少积极的实例被错过。

> 回忆=TPTP+FN=2929+93=0.24 回忆= TPTP+FN = 2929+93 = 0.24

我们发现 100%的帖子都是关于太空的。

# 权衡取舍

在精确度和召回率之间经常要进行权衡。我们也许可以调整我们的模型来提高精确度；也就是说，做出更少的假阳性预测，但这是以更低的召回率为代价的。类似地，如果我们调整模型来提高召回率，那么精度可能会受到影响。

没有一种正确的方法来调整您的模型。你已经根据具体情况作出了决定。给定 20 个新闻组数据集和我们训练的模型，我们对我们的精确度和召回分数满意吗？这取决于我们计划如何使用这个模型和业务用例。假设我们使用这个模型作为自动确定新帖子标签的系统的一部分，这样我们就可以向对空间感兴趣的人推荐帖子。

给定 24%的精度，超过 75%的推荐将是错误的。而且召回率 100%，所有关于太空的帖子都会被正确识别。我们的用户可能会很恼火，因为我们推荐了太多不相关的帖子(误报)。对于这种情况，如果我们提高精确度，使我们的推荐更相关，但可能会错过更多的空间帖子，我们的用户会更高兴。

现在考虑人群中特定疾病的筛查测试。假设我们训练一个模型，在给定病人一些症状的情况下检测疾病。如果我们的模型预测阳性，我们会立即送病人去做进一步的测试和治疗。如果模型预测错误，那么我们假设患者是安全的。这种情况下的低精确度意味着许多预测的阳性病例将被证明没有患病。高召回将意味着大多数阳性病例被发现。这似乎是对的。想象一下，如果召回率低，许多患有该疾病的人将被错误地归类为没有患病。

这是一个召回比精确更重要的案例，因为我们不想让需要治疗的人从缝隙中溜走(假阴性)。

# f 分数

当评估分类器时，将精确度和召回率结合成称为 F1 分数的单个值是方便的。我们使用 F-score 来看这两个指标结合起来是如何达到平衡的。

这是通过以下公式完成的:

> f1 = 2×precision×recall precision+recall = 2×tp2×TP+FN+FP f1 = 2×precision×recall precision+recall = 2×tp2×TP+FN+FP

在计算该值时，精确度和召回率的贡献是相等的。

F1 分数有一个更一般的情况，叫做 F-beta 分数。为了计算该分数，使用参数ββ来调整召回率和精确度的贡献。为了提高精度，使用β <1β<1 is used and to favor recall a value of β> 1β>1 的值。这样，如果您正在构建一个精度比召回更重要的模型，则将ββ的值设置为小于 1。公式是:

> fβ=(1+β2)×精度×召回率(β2×精度)+召回率=(1+β2)×TP(1+β2)×TP+β×FN+FPFβ=(1+β2)×精度×召回率(β2×精度)+召回率=(1+β2)×TP(1+β2)×TP+β×FN+FP

我们不必手工进行这些计算。Scikit-learn 在 sklearn.metrics 库中提供了计算这些值的函数。这些函数中的每一个都将实际目标值和模型做出的预测作为输入。

> 准确度=精确度 _ 分数(y _ 测试，预测)
> 精确度=精确度 _ 分数(y _ 测试，预测)
> 召回=召回 _ 分数(y _ 测试，预测)
> f1 = f1 _ 分数(y _ 测试，预测)
> fbeta _ 精确度= fbeta _ 分数(y _ 测试，预测，0.5)
> fbeta _ 召回= fbeta _ 分数(y _ 测试，预测，2)
> 打印('精确度分数:{:.2f} '。格式(精度))
> 打印('精度分数:{:.2f} '。format(precision))
> 打印('召回分数:{:.2f} '。格式(回忆))
> 打印(' F1 分数:{:.2f} '。format(f1))
> print('Fbeta 分数偏向精度:{:.2f} '。format(FBeta _ precision))
> print(' FBeta 评分偏向召回:{:.2f} '。格式(fbeta_recall))

```
Accuracy score: 0.97
Precision score: 1.00
Recall score: 0.24
F1 score: 0.38
Fbeta score favoring precision: 0.61
FBeta score favoring recall: 0.28
```

请注意，FβFβ得分的β <1β<1 scored higher than the FβFβ with β> 1β>1。这是因为我们有比召回分数更高的精确度分数。

classification_report 函数对于通过一次调用给出这些指标的摘要也很有用。

> report = class ification _ report(y _ test，predictions，target_names=['Not Space '，' Space'])
> 打印(报告)

```
precision    recall  f1-score   support Not Space       0.97      1.00      0.98      2707
       Space       1.00      0.24      0.38       122 accuracy                           0.97      2829
   macro avg       0.98      0.62      0.68      2829
weighted avg       0.97      0.97      0.96      2829
```

我们可以将该报告与关于虚拟分类器的报告进行比较。虚拟分类器可以用于以与真实分类器相似的方式进行预测，除了我们将得到所有作为主导类的结果。在这个例子中，我们知道主导类是否定类，所以我们的虚拟分类器将 0 个实例预测为肯定类。

> dummy _ report = class ification _ report(y _ test，dummy.predict(X_test)，target_names=['Not Space '，' Space '])
> print(dummy _ report)

```
precision    recall  f1-score   support Not Space       0.96      1.00      0.98      2707
       Space       0.00      0.00      0.00       122 accuracy                           0.96      2829
   macro avg       0.48      0.50      0.49      2829
weighted avg       0.92      0.96      0.94      2829/Users/karenfarbman/anaconda3/lib/python3.6/site-packages/sklearn/metrics/_classification.py:1272: UndefinedMetricWarning: Precision and F-score are ill-defined and being set to 0.0 in labels with no predicted samples. Use `zero_division` parameter to control this behavior.
  _warn_prf(average, modifier, msg_start, len(result))
```

未定义的度量警告是由于 TP + FP == 0 的事实。这是一个很好的指示，表明即使精度很高，分类器的性能也不好。即使我们的分类器确实有一些 FP，对于那个类来说，精度和召回率也是相当低的。

# 决策函数—改变概率阈值

给定一些输入，模型为每个输入生成一些概率。这是由 predict_proba()方法给出的。该方法给出了该实例是否属于正类的概率，而不是给出实际的预测。

在下面的代码中，生成了测试数据的概率列表，并打印出前 30 个值。

> probs = lr . predict _ proba(X _ test)[:，1]
> print(probs[1:30])

```
[0.01364891 0.05711429 0.03494059 0.03348212 0.02932255 0.02216111
 0.45071078 0.01932676 0.07011708 0.08472435 0.03919031 0.02427247
 0.01909561 0.02652153 0.06818679 0.02653776 0.09209324 0.03810381
 0.01464452 0.02377862 0.02685651 0.05803195 0.03221372 0.02388465
 0.0661609  0.06428845 0.04730358 0.0219367  0.03626409]
```

我们在上一课中已经看到，概率阈值是 0.5:也就是说，如果概率大于 0.5，我们将预测为正，如果小于 0.5，我们将预测为负。在下图中，所有测试样本的概率绘制在两个分布图中。红色地块为负类，绿色地块为正类。蓝线是概率阈值。

请注意，在阈值的两边都有积极的实例。这意味着阈值左边的所有阳性实例将被错误地分类为阴性，从而给出假阴性。

> pos = [i for i，j in zip(probs，y_test) if j == 1]
> neg = [i for i，j in zip(probs，y _ test)if j = = 0]
> with PLT . xkcd():
> fig = PLT . fig(figsize =(8，4))
> sns.distplot(pos，hist = False，kde = True，color='g '，
> kde_kws = {'shade': True，'线宽

通过调整阈值，我们可以影响召回和精度值。假设我们想确保没有误报:也就是说，我们不想将任何与空间无关的帖子归类为空间。我们可以将阈值向右移动，直到该行右侧没有红色实例。这相应地将意味着更少的阳性实例将被正确识别。或者，换句话说，更多的阳性实例将在阈值的左边结束，并且将被错误地分类为阴性。

# 受试者工作特征曲线

如果我们将阈值设置为 0(将阈值一直向左移动)，那么模型预测所有样本为 1。我们将获得 1 的真实阳性率(tpr ),即所有阳性实例都将被正确预测。我们还会得到 1 的假阳性率(fpr ),因为所有阴性样本都会被错误地预测为阳性。

如果我们将阈值设置为 1，那么模型预测所有样本为 0。我们得到 tpr 为 0，因为没有一个实际的阳性样本将被正确预测，并且得到 fpr 为 0，因为没有一个阴性类别将被预测为阳性。

当我们在这两个极端之间改变阈值时，我们得到不同的 tpr 和 fpr 值。我们可以绘制这些值，以查看阈值移动时这些值之间的关系。

当我们将阈值从 0 改变到 1 时，我们得到的一组点可以被连接来描述一条穿过空间的曲线，该曲线被称为接收器工作特性(ROC)曲线。

sklearn.metrics.roc_curve 函数计算我们可以用来绘制 roc 曲线的 fpr、tpr 和阈值。下面打印前 30 个值。

> fpr，tpr，thresholds = roc_curve(y_test，probs)
> print(FPR[1:30])
> print(TPR[1:30])
> print(thresholds[1:30])

```
[0\.         0\.         0.00036941 0.00036941 0.00073883 0.00073883
 0.00110824 0.00110824 0.00147765 0.00147765 0.00184706 0.00184706
 0.00221648 0.00221648 0.00258589 0.00258589 0.00332471 0.00332471
 0.00369413 0.00369413 0.00480236 0.00480236 0.0059106  0.0059106
 0.00628001 0.00628001 0.00664943 0.00664943 0.00775767]
[0.00819672 0.46721311 0.46721311 0.48360656 0.48360656 0.5
 0.5        0.59016393 0.59016393 0.6147541  0.6147541  0.62295082
 0.62295082 0.64754098 0.64754098 0.68852459 0.68852459 0.70491803
 0.70491803 0.74590164 0.74590164 0.7704918  0.7704918  0.77868852
 0.77868852 0.78688525 0.78688525 0.80327869 0.80327869]
[0.98462604 0.3446592  0.34283523 0.3311512  0.32653259 0.32153467
 0.31893541 0.27469392 0.27468563 0.24375424 0.24159574 0.23921165
 0.23860809 0.21948719 0.20223596 0.18165188 0.17696934 0.16373987
 0.16369255 0.15575538 0.15284347 0.1478981  0.13727432 0.13693788
 0.13573329 0.13529207 0.13304517 0.13221123 0.12896697]
```

> fig = plt.figure(figsize = (6，6))
> plt.plot([0，1]，[0，1]，' k — ')
> plt.plot(fpr，tpr)
> plt.xlabel('假阳性率')
> plt.ylabel('真阳性率')
> PLT . title(' Logistic 回归模型的 ROC 曲线')
> plt.show()

该图的左上角是理想点:它使假阳性率最小化，使真阳性率最大化。因此，接近该拐角的陡峭曲线比保持接近虚线基线的浅曲线更好。虚线代表 50%概率分类器。也就是说，任何像掷硬币一样有效的分类器都会有一条接近那条线的曲线。但这和随机一样好。所以这条线上的曲线比随机曲线好。这个分类器的特殊曲线非常好，因为你可以看到它在变平之前急剧上升。

# 精确回忆曲线

另一个有用的可视化工具是精确回忆曲线。类似于 ROC 曲线，它显示了当阈值从 0 到 1 变化时，精确度与回忆的关系。同样，sklearn.metrics 库提供了一个有帮助的函数。

> pres，rec，thresholds = Precision _ Recall _ Curve(y _ test，predictions)
> fig = PLT . figure(fig size =(6，6))
> plt.plot(rec，pres)
> PLT . xlabel(' Recall ')
> PLT . ylabel(' Precision ')
> PLT . title(' Precision-Recall Curve ')
> PLT . show()

在这种情况下，我们看到随着召回率的增加，准确率平稳下降。我们并不总是有如此平滑的曲线，但总体下降趋势是预料之中的。

请记住，这两种可视化服务略有不同的目的。

*   当类别或多或少平衡时使用 ROC 曲线。
*   当存在类别不平衡时，使用精确召回曲线。

# ROC 曲线下面积

给定 ROC 曲线，我们可以从中计算出一个有用的指标。因为左上角是理想的，我们希望曲线尽可能靠近那个角。如果我们测量曲线下的面积，我们会得到一个有用的度量，告诉我们离理想有多近。在下图中，我们给曲线下的区域加了阴影。请注意，如果曲线越来越接近基线，面积将越来越小。

> fig = plt.figure(figsize = (6，6))
> plt.plot([0，1]，[0，1]，' k — ')
> plt.plot(fpr，tpr)
> plt.fill(fpr，tpr，' grey '，alpha=0.3)
> plt.xlabel('假阳性率')
> plt.ylabel('真阳性率')
> plt.title('逻辑回归模型的 ROC 曲线')

为了计算曲线下面积(AUC ),我们使用 sklearn.metrics 中的 roc_auc_score 函数。该函数将实际标签和预测概率作为输入。

> auc = roc_auc_score(y_test，probs)
> print(' ROC 曲线下面积:{:.3f} '。格式(auc))

```
Area under the ROC curve: 0.990
```

# 交互效度分析

到目前为止，我们一直使用训练测试分割来保留一部分数据进行测试。我们这样做是为了让我们有一些以前看不到的数据来测试模型。如果我们使用相同的数据进行训练和测试，我们将面临模型过度适应训练数据的风险。

但是仍然存在过度适应测试数据本身的风险。如果我们调整我们的模型以在测试数据上表现良好，我们有什么保证它将在新数据上继续表现良好？此外，我们知道，训练算法容易受到数据微小变化的影响。回想一下关于线性分类器的讨论，我们正在寻找一组优化成本函数的参数值。这样的搜索没有绝对正确的答案。我们找到的值取决于我们用来训练模型的数据。

通过将数据分为训练和测试，我们大大减少了可用于学习模型的样本数量，并且结果可以取决于对集合对的特定随机选择。

我们可以通过多次调用 train_test_split 并用每次的结果训练一个模型来说明这一点。train_test_split 函数将数据随机分配给训练集和测试集。这意味着每次你调用这个函数时，不同的数据集被分配给测试集。

> X_train，X_test，y_train，y_test = train_test_split(X，y _ 2)
> lr = LogisticRegression(solver = ' lbfgs ')
> lr . fit(X _ train，y_train)
> print('第一次拆分得分:{:.3f} '。format(lr.score(X_test，y _ test))
> X _ train，X_test，y_train，y_test = train_test_split(X，y _ 2)
> lr = LogisticRegression(solver = ' lbfgs ')
> lr . fit(X _ train，y_train)
> print('第二次拆分分数:{:.3f} '。格式(lr.score(X_test，y_test)))

```
First split score: 0.960
Second split score: 0.955
```

在上面的代码中，train_test_split 被调用了两次，并使用结果对两个模型进行了训练。请注意，不同型号的精确度不同。这意味着，我们冒着随机选择有偏见的训练或测试数据集的风险，最终得到的模型不能很好地概括看不见的数据。

这就是交叉验证的由来。

在我们探索交叉验证之前，先简要说明一下 train_test_split 的随机性。如果我们每次都得到不同的测试集，并且每次都得到不同的准确度分数，那么我们如何知道任何观察到的改进是参数调整的结果还是随机的呢？在模型构建过程中，创建一个保持稳定的测试分割是很有用的。为此，我们使用 random_state 参数(我们在本笔记开始时使用过)。此参数接受一个整数，并将其用作随机生成器的种子。这意味着您可以指定相同的数字，以每次获得相同的分割。在下面的代码中，两个模型再次针对两个数据分割进行训练，这次使用相同的 random_state。

> X_train，X_test，y_train，y_test = train_test_split(X，y_2，random _ state = 40)
> lr = LogisticRegression(solver = ' lbfgs ')
> lr . fit(X _ train，y_train)
> print('第一次拆分得分:{:.3f} '。format(lr.score(X_test，y _ test))
> X _ train，X_test，y_train，y_test = train_test_split(X，y_2，random _ state = 40)
> lr = LogisticRegression(solver = ' lbfgs ')
> lr . fit(X _ train，y_train)
> print('第二次拆分分数:{:.3f} '。格式(lr.score(X_test，y_test)))

```
First split score: 0.964
Second split score: 0.964
```

现在回到交叉验证。交叉验证的工作原理是将数据集分成指定数量(k)的不同集合，称为折叠。通常 k = 5 或 10 倍，但你可以随意使用任何数量的折叠。然后，我们迭代折叠，用 k-1 个折叠训练一个模型，并使用剩余的折叠作为验证的测试集。在每次迭代中，不同的折叠被用作测试集。

这为创建和训练的 k 个模型产生了 k 个准确度分数。但是，每个模型都是在不同的数据集上训练和测试的。此外，每个数据样本都有机会出现在一个模型的测试集中。然后，我们可以找到分数的平均值，以获得模型的整体值。

sk learn . model _ selection . cross _ val _ score 函数执行这个交叉验证过程，为创建的每个模型提供一个分数数组。请记住，这可能是一个昂贵的(就时间而言)过程，因为你必须创建和训练 k 个不同的模型。通常，您首先使用 train_test_split 将数据拆分为训练集和测试集，就像我们在上面所做的那样，交叉验证在训练集上完成，而测试集用于对所选模型的最终评估。

> X_train，X_test，y_train，y_test = train_test_split(X，y_2，random _ state = 40)
> clf = logistic regression(solver = ' lbfgs ')
> cv _ scores = cross _ val _ score(clf，X_train，y_train，cv = 5)
> print(' 5 倍的准确度分数: '，cv_scores)
> print('平均交叉验证分数:{:.3f})。格式(np.mean(cv_scores)))

```
Accuracy scores for the 5 folds:  [0.95167943 0.96169711 0.95816146 0.95639364 0.95875074]
Mean cross validatiion score: 0.957
```

基于分数，模型能够在数据集的所有 5 个折叠上一致地预测高。这让我们确信，该模型在看不见的数据上多次表现良好，这不仅仅是因为在最初的数据分割中运气好。

# 结论:

准确度分数并不能说明全部情况，因此我们使用各种技术来更好地了解我们的分类模型的执行情况。

有了不同的评估度量，我们可以识别模型的错误分类，并使用这些信息在进一步的迭代中改进模型。

我们还想考虑权衡，以便更好地将我们的时间和精力集中在下一个模型迭代上。如何使用该模型将推动这些决策。

我们现在可以使用上面描述的各种模型评估技术以及交叉验证来为我们的下一个项目选择性能最佳的模型。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)