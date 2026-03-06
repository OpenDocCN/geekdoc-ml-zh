# 使用分类变量

> 原文：<https://medium.com/mlearning-ai/working-with-categorical-variables-10d30936e496?source=collection_archive---------6----------------------->

![](img/6b53ed5bfdb17f9d08e989e5c9eb0825.png)

Photo by [v2osk](https://unsplash.com/@v2osk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

世界分为许多类别，数据也是如此。思考类别是孩子理解世界的第一步。我们教孩子世界的分类。猫、狗、老虎、鹿都是动物。然后我们教猫、狗、羊是家养的，狮子、老虎、羊是野生动物。红色、黄色和蓝色是颜色。当孩子稍微大一点的时候，他/她会被介绍到有序分类的世界——第一、第二、第三以及冷、暖和热的温度。对于我们人类来说，分类对于理解世界非常重要。不幸的是，大多数机器学习算法不能处理分类变量。一些算法可以处理分类数据。[决策树——取决于它的实现——可以直接从分类数据中学习，不需要数据转换。](https://en.wikipedia.org/wiki/Decision_tree_learning#Advantages)

那么应该怎么做呢？

机器学习算法理解分类变量的第一步是将它们转换成数值。

## 什么是分类变量？

在我们开始将分类变量转换成数值之前，理解各种类型的分类变量是很重要的。在数据科学中，有两种类型的分类变量——名义变量和序数变量。

*名义变量*没有特定的顺序，没有办法比较一个值和另一个值。例如，猫、狗、山羊、牛、狮子、老虎(动物)或钢笔、橡皮擦、铅笔(书写工具)或食草动物、食肉动物和杂食动物(基于饮食习惯)。人们无法以任何顺序排列这些值。

*序数变量*有一定的顺序。示例—第一、第二或第三(考试排名)或 A+、B、C-(考试成绩)或冷、暖、热(物体温度)。这些值是有顺序的，我们可以按某种顺序排列它们，比如物体的温度冷<暖<热。来自用户的反馈通常也是分类变量(优秀、良好、满意和差或强烈同意、同意、中立、不同意和强烈不同意)。

# 编码器

## 为机器学习算法转换数值

![](img/e2bb7b110acb0798027060c4a96d4ecc.png)

Photo by [Jorge Ramirez](https://unsplash.com/@jorgedevs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

有许多方法可以将分类变量转换为数值。

1.  一键编码
2.  标签编码
3.  顺序编码
4.  赫尔默特编码
5.  二进制编码
6.  频率编码
7.  平均编码
8.  证据权重编码
9.  概率比编码
10.  哈希编码
11.  后向差分编码
12.  省略一个编码
13.  詹姆斯-斯坦编码
14.  m 估计编码
15.  温度计编码器

# 标签编码

假设数据中的分类变量有 *n* 种类别(不同的值)。在[标签编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)中，目标变量用 0 到 *n-1* 之间的值编码。这些作业之间没有关系。标签编码器通常根据字母顺序分配值。

sklearn_lbl_encode_1

这些号码是按字母顺序排列的。*冷*编码为 *0* ，*热*编码为 *1* ，*极热*编码为 2，以此类推。

sklearn_lbl_enc_1_result

如果添加了一个新的类别，则根据*字母顺序*改变编码。

sklearn_lbl_encode_2

引入了新的编码*微温*为 *2* 并且*非常热*改为 *3* 。

sklearn_lbl_enc_2_result

熊猫函数`factorize`也执行标签编码。在这种情况下，根据列中值出现的顺序*进行编码。*

pandas_factorize_1

*热*被编码为 *0* ，因为它是第一个值，然后是*极热*、*暖*和*冷*。

pandas_factorize_results_1

当引入一个新的分类值时，分配的标签会改变。新的分类值*微温*被分配给 *1* 。

pandas_factorize_2

pandas_factorize_results_2

# 一键编码

标签编码的缺点之一是，即使这些编码之间没有关系或顺序，机器学习算法也可能以某种顺序或关系来考虑它们。所以 ML 算法可能会认为*0<1<2<3<4*所以*热<微温<很热<暖<冷*这可能不是一个正确的关系。这可能会导致较差的性能和意外的结果。

在这种情况下，可以应用[一键编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)。这种编码需要将分类数据提供给许多 scikit-learn 估计器，如线性模型、具有标准内核的支持向量机、神经网络以及聚类算法。事实上，OHE 可以用于任何在训练期间同时查看所有特征的机器学习算法。

在 OHE 中，每个类别都被转换为一个新列，并为该列赋值 0 或 1。实现如下。

## 使用 get_dummies()

在下面的实现中，列*颜色*被一键编码。

在实现中，自动删除列颜色，并为类别的每个不同值创建 3 个新列。0 或 1 的值基于该观察(行)中的类别值。

## 使用 sci-kit 学习库

在 sci-kit learn library 中使用 OHE 时，有两个步骤。在步骤 1 中，对列进行编码，在步骤 2 中，将编码值添加回 dataframe。Python 中的实现如下

*继续关注此空间，了解与其他编码器相关的详细信息*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)