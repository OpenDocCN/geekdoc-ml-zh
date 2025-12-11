# 6 分类讨论

> 原文：[`mlbook.explained.ai/catvars.html`](https://mlbook.explained.ai/catvars.html)

[特伦斯·帕特](http://parrt.cs.usfca.edu) 和 [杰里米·霍华德](http://www.fast.ai/about/#jeremy)

版权所有 © 2018-2019 特伦斯·帕特。保留所有权利。

*请勿在网络上复制或以任何方式重新分发。*

这本书是从 markup+markdown+python+latex 源文件生成的，使用了 [Bookish](https://github.com/parrt/bookish)。

您可以通过访问此页面的注释版本来对**此页进行评论或标注**。您会看到现有的标注部分以黄色突出显示。它们是**公开可见的**。或者，您可以直接将评论、建议或修正发送给 特伦斯。

目录

+   获取基线

+   编码分类变量

+   从字符串中提取特征

+   合成数值特征

+   对分类变量进行目标编码

+   注入外部邻里信息

+   我们的最终模型

+   分类特征工程摘要

“*提出特征是困难的，耗时，需要专家领域知识。当应用学习时，我们花费大量时间调整特征。*” — 安德鲁·吴在 2011 年斯坦福大学关于人工智能的全体会议上的讲话

创建一个好的模型更多的是关于**特征工程**，而不是选择正确的模型；嗯，假设您首选的模型是一个好的模型，比如随机森林。特征工程意味着改进、获取甚至**合成**那些是模型目标变量强预测者的特征。合成特征意味着从现有特征中推导出新的特征或从其他数据源注入特征。例如，我们可以从公寓的经纬度中合成纽约市某个地区的名称。如果我们不提供有用的东西供其咀嚼，那么我们的模型无论多么复杂都没有意义。如果没有关系可发现，因为特征不可预测，那么没有任何机器学习模型能够给出准确的预测。

到目前为止，我们已经删除了非数值特征，例如公寓描述字符串和管理员 ID 的分类值，因为机器学习模型只能从数值值中学习。但是，这些字符串值中可能存在我们可以利用的潜在预测能力。在本章中，我们将学习特征工程的基础知识，以便从纽约市公寓租金数据集中找到的非数值特征中提取更多价值。

租金价格的预测能力主要来自公寓位置、卧室数量和浴室数量，因此我们不应该期望模型性能有大幅提升。本章的主要目标是学习在实际工作中使用的技术。或者，如果你参加了 Kaggle 比赛，前 100 名和第 1000 名的差距往往非常小，所以在这种情况下，特征工程带来的微小优势也可能是有用的。

## 6.1 获取基线

请参阅**第 3.2.1 节** *加载数据和嗅探训练数据*，了解从 Kaggle 下载 JSON 租金数据并创建 CSV 文件的说明。

不要忘记[笔记本](https://mlbook.explained.ai/notebooks/)，它汇总了各章节中的代码片段。

为了衡量模型准确性的任何改进，让我们使用位于我们开始 Jupyter 的`data`目录下的`rent.csv`中清理后的数值特征来获取基线。正如我们在**第五章** *探索和去噪您的数据集*中所做的那样，让我们加载数据，去除极端价格，并移除不在纽约市内的公寓：1

```py
df = pd.read_csv("data/rent.csv", parse_dates=['created'])
df_clean = df[(df.price>1_000) & (df.price<10_000)]
df_clean = df_clean[(df_clean.longitude!=0) | (df_clean.latitude!=0)]
df_clean = df_clean[(df_clean['latitude']>40.55) &
                    (df_clean['latitude']<40.94) &
                    (df_clean['longitude']>-74.1) &
                    (df_clean['longitude']<-73.67)]
df = df_clean

```

现在仅使用数值特征训练一个随机森林，并打印出袋外（OOB）![图片](img/ec985123b9b52e80981e6500795e8d16.png)得分：

```py
numfeatures = ['bathrooms', 'bedrooms', 'longitude', 'latitude']
X, y = df[numfeatures], df['price']
rf = RandomForestRegressor(n_estimators=100, n_jobs=-1, oob_score=True)
rf.fit(X, y)
oob_baseline = rf.oob_score_
print(oob_baseline)
```

0.8677576247438816

0.868 的 OOB 得分相当不错，因为它接近 1.0。（回想一下，得分为 0 表示我们的模型确实知道比简单地猜测所有公寓的平均租金价格要好，而 1.0 则意味着完美预测租金价格。）让我们也了解一下随机森林为了捕捉这些特征与租金价格之间的关系需要做多少工作。`rfpimp`包提供了一些我们可以使用的简单度量：森林中所有决策树的总节点数和典型树的高度（以节点为单位）。

```py
print(f"{rfnnodes(rf):,d} tree nodes and {np.median(rfmaxdepths(rf))} median tree height")

```

2,432,836 个树节点和 35.0 的中位树高

树高很重要，因为这是随机森林预测机制所采取的路径，因此树高会影响预测速度。

让我们也为特征重要性设置一个基线，这样当我们引入新特征时，我们可以了解它们的预测能力。经度和纬度实际上是称为“位置”的一个元特征，因此我们可以使用`importances()`函数的`features`参数将这两个特征结合起来。在本章中，我们将多次请求特征重要性，所以让我们创建一个方便的函数：

» *由左侧代码生成*

![图片](img/ab971df2b73436db3be3a4d088755d5d.png)(images/catvars/catvars_cats_6.svg)

```py
def showimp(rf, X, y):
    features = list(X.columns)
    features.remove('latitude')
    features.remove('longitude')
    features += [['latitude','longitude']]

    I = importances(rf, X, y, features=features)
    plot_importances(I, color='#4575b4')

showimp(rf, X, y)
```

现在我们已经为模型和特征性能设定了一个良好的基准，让我们尝试通过将一些现有的非数值特征转换为数值特征来改进我们的模型。

## 6.2 编码分类变量

`interest_level` 特征是一个分类变量，似乎表示对公寓的兴趣，无疑是从网页活动日志中提取的。它是一个分类变量，因为它从有限的选择集中取值：低、中、高。更具体地说，`interest_level` 是一个**序数**分类变量，这意味着即使它们不是实际数字，值也可以排序。查看每个序数值的计数是了解序数特征的好方法：

```py
print(df['interest_level'].value_counts())
```

low 33270 medium 11203 high 3827 Name: interest_level, dtype: int64

序数变量最容易从字符串转换为数字，因为我们可以为每个可能的序数值分配一个不同的整数。进行转换的一种方法是用 Pandas 的 `map()` 函数和一个字典参数：

```py
df['interest_level'] = df['interest_level'].map({'low':1,'medium':2,'high':3})
print(df['interest_level'].value_counts())
```

1 33270 2 11203 3 3827 Name: interest_level, dtype: int64

关键是要确保我们为每个类别使用的数字保持相同的顺序，例如，中等值 2 大于低值 1。我们也可以选择 `{'low':10,'medium':20,'high':30}` 作为编码，因为随机森林（RF）关心的是特征顺序而不是尺度。

让我们看看使用数值特征和这个新转换的 `interest_level` 特征的随机森林（RF）模型的表现：

```py
def test(X, y):
    rf = RandomForestRegressor(n_estimators=100, n_jobs=-1, oob_score=True)
    rf.fit(X, y)
    oob = rf.oob_score_
    n = rfnnodes(rf)
    h = np.median(rfmaxdepths(rf))
    print(f"OOB R² {oob:.5f} using {n:,d} tree nodes with {h} median tree height")
    return rf, oob

X, y = df[['interest_level']+numfeatures], df['price']
rf, oob = test(X, y)

```

使用 3,025,158 个树节点和 36.0 的中值树高，OOB R² 0.87037

那个 0.870 分数只是略好于我们的基线 0.868，但仍然很有用。当你接近 1.0 的阈值时，提高模型性能变得越来越困难。分数的差异实际上代表了剩余准确性的几个百分点，因此我们可以对这个提升感到满意。此外，记住使用单个 OOB 分数是一个相当粗略的指标，它汇总了模型在 50,000 条记录上的性能。可能某种类型的公寓获得了很大的准确度提升。这种准确度提升的唯一代价是树节点数量的增加，但森林中典型的决策树高度与基线保持相同。使用此模型进行预测应该与基线一样快。

记住序数变量和名义变量之间的区别的简单方法是：序数变量有顺序，而名义来自拉丁语中“名称”一词（*nomen*）或法语中的 (*nom*）。

序数分类变量在数值编码上很简单。不幸的是，还有一种叫做**名义**变量的分类变量，其类别值之间没有有意义的顺序。例如，在这个数据集中，列 `manager_id`、`building_id` 和 `display address` 是名义特征。由于类别之间没有顺序，很难以有意义的方式将名义变量编码为数字，尤其是当有非常多的类别值时：

```py
print(len(df['manager_id'].unique()), len(df['building_id'].unique()), len(df['display_address'].unique()))
```

(3409, 7417, 8692)

所说的“高基数”（多值）分类变量，例如`manager_id`、`building_id`和`display address`，最好使用更高级的技术来处理，例如[嵌入](https://developers.google.com/machine-learning/crash-course/embeddings/video-lecture)或*均值编码*，正如我们在**第 6.5 节**“目标编码分类变量”中将要看到的，但在这里我们将介绍两种简单的编码方法，这些方法通常很有用。

第一种技术被称为*标签编码*，它简单地将每个类别转换为数值，就像我们之前做的那样，同时忽略这些类别实际上并没有顺序的事实。有时随机森林可以从以这种方式编码的特征中获得一些预测能力，但通常需要更大的森林中的树。

要对任何分类变量进行标签编码，将列转换为有序分类变量，然后将字符串转换为相关的分类代码（这是由 Pandas 自动计算的）：

```py
df['display_address_cat'] = df['display_address'].astype('category').cat.as_ordered()
df['display_address_cat'] = df['display_address_cat'].cat.codes + 1

```

类别代码从 0 开始，而“非数字”(`nan`)的代码是-1。为了将所有内容都带入 0 或以上的范围，我们在类别代码上加上 1。（Sklearn 在其`OrdinalEncoder`转换器中具有等效的功能，但它无法处理同时包含整数和字符串的对象列，并且对于表示为`np.nan`的缺失值会报错。）

让我们看看这个新特征是否可以提高基线模型的表现：

```py
X, y = df[['display_address_cat']+numfeatures], df['price']
rf, oob = test(X, y)

```

OOB R² 0.86583，使用 3,120,336 个树节点，中值树高为 37.5

不幸的是，![分数](img/ec985123b9b52e80981e6500795e8d16.png)与基线分数大致相同，并且它增加了树的高度。这不是一个好的权衡。

让我们尝试另一种编码方法，称为*频率编码*，并将其应用于`manager_id`特征。频率编码将类别转换为它们在训练中出现的频率。例如，以下是前 5 个`manager_id`的类别值计数：

```py
print(df['manager_id'].value_counts().head(5))
```

e6472c7237327dd3903b3d6f6a94515a 2509 6e5c10246156ae5bdcd9b487ca99d96a 695 8f5a9c893f6d602f4953fcc0b8e6e9b4 404 62b685cc0d876c3a1a51d63a0d6a8082 396 cb87dadbca78fad02b388dc9e8f25a5b 370 名称：manager_id，数据类型：int64

这种转换背后的想法是，特定经理管理的公寓数量可能存在预测能力。

函数`value_counts()`给我们提供了编码，从那里我们可以使用`map()`将`manager_id`转换为一个名为`mgr_apt_count`的新列：

```py
managers_count = df['manager_id'].value_counts()
df['mgr_apt_count'] = df['manager_id'].map(managers_count)

```

再次，我们实际上可以除以`len(df)`，但这只是缩放列，不会影响预测能力。

让我们看看模型在将这两个分类变量编码后的表现如何：

```py
X, y = df[['display_address_cat','mgr_apt_count']+numfeatures], df['price']
rf, oob = test(X, y)

```

OOB R² 0.86434，使用 4,569,236 个树节点，中值树高为 41.0

![图片](img/72336ba67d853507cece336c37351eb6.png)(images/catvars/catvars_cats_18.svg)

模型的准确率没有提高，树的复杂性增加，因此我们应该避免引入这些特征。特征重要性图确认了这些特征并不重要。

对于这个数据集，我们可以得出结论，对于高基数分类变量的标签编码和频率编码没有帮助。然而，这些编码技术可能在其他数据集上是有用的，因此值得学习。我们将在[chp:onehot]中学习另一种分类变量编码技术，称为“独热编码”，但在这里我们避免使用独热编码，因为它会为我们的数据集添加数千个新列。

## 6.3 从字符串中提取特征

公寓数据集还有一些变量既不是数值型也不是分类型，`description`和`features`。这些任意的字符串没有明显的数值编码，但我们可以从中提取一些信息来创建新的特征。例如，带有停车、门卫、洗碗机等的公寓可能会获得更高的价格，所以让我们从字符串特征中合成一些布尔特征。

首先，让我们将字符串列规范化为小写，并将任何缺失值（表示为“不是一个数字”`np.nan`值）转换为空字符串；否则，应用`split()`到列上将会失败，因为`split()`不适用于浮点数（`np.nan`）。

```py
df['description'] = df['description'].fillna('')
df['description'] = df['description'].str.lower() # normalize to lower case
df['features'] = df['features'].fillna('') # fill missing w/blanks
df['features'] = df['features'].str.lower() # normalize to lower case

```

现在，我们可以通过应用`contains()`到字符串列上创建新的列，每个新列一次：

```py
# has apartment been renovated?
df['renov'] = df['description'].str.contains("renov")

for w in ['doorman', 'parking', 'garage', 'laundry', 
          'Elevator', 'fitness center', 'dishwasher']:
    df[w] = df['features'].str.contains(w)
df[['doorman', 'parking', 'garage', 'laundry']].head(5)
```

|   | doorman | parking | garage | laundry |
| --- | --- | --- | --- | --- |
|  |
| --- |
| 0 | False | False | False | False |
| 1 | True | False | False | False |
| 2 | False | False | False | True |
| 3 | False | False | False | False |
| 4 | False | False | False | False |

另一个技巧是计算字符串中的单词数量

```py
df["num_desc_words"] = df["description"].apply(lambda x: len(x.split()))
df["num_features"] = df["features"].apply(lambda x: len(x.split(",")))

```

对于其他`photos`字符串列中的照片 URL 列表，我们做不了太多，但照片的数量可能略微预示着价格，因此我们也为这个特性创建一个：

```py
df["num_photos"] = df["photos"].apply(lambda x: len(x.split(",")))

```

让我们看看我们的新特征如何影响基准数值特征之上的性能：

```py
textfeatures = [
    'num_photos', 'num_desc_words', 'num_features',
    'doorman', 'parking', 'garage', 'laundry', 
    'Elevator', 'fitness center', 'dishwasher',
    'renov'
]
X, y = df[textfeatures+numfeatures], df['price']
rf, oob = test(X, y)

```

OOB R² 0.86242 使用 4,767,582 个树节点，中值树高度为 43.0

» *由代码生成左侧*

![](img/catvars_cats_24.svg)

```py
showimp(rf, X, y)
```

令人失望的是，这些特征并没有提高准确率，而且模型的复杂性更高，但这确实提供了有用的营销信息。像停车、洗衣和洗碗机这样的公寓特征很吸引人，但几乎没有证据表明人们愿意为它们支付更多（也许除了有门卫的情况）。

## 6.4 合成数值特征

如果你有很多兄弟姐妹长大，你很可能熟悉早上等待使用洗手间的情况。也许卧室与洗手间比例中存在一些预测能力，所以让我们尝试合成一个新列，它是两个数值列的比例：

```py
df["beds_to_baths"] = df["bedrooms"]/(df["bathrooms"]+1) # avoid div by 0
X, y = df[['beds_to_baths']+numfeatures], df['price']
rf, oob = test(X, y)

```

OOB R² 0.86831 使用 2,433,878 个树节点，平均树高为 35.0

将数值列组合起来是一个值得记住的有用技术，但不幸的是，这种组合在这里对我们的模型影响并不显著。也许我们将一个数值列与目标组合起来会更好，比如卧室与价格的比率：

```py
df["beds_per_price"] = df["bedrooms"] / df["price"]
X, y = df[['beds_per_price']+numfeatures], df['price']
rf, oob = test(X, y)

```

OOB R² 0.98601 使用 1,313,390 个树节点，平均树高为 31.0

哇！这几乎是一个完美的分数，这应该触发一个警报，表明它太好了，不可能是真的。事实上，我们确实有一个错误，但这不是代码错误。我们实际上已经将价格列复制到了特征集中，就像通过查看答案来准备小测验一样。这是一种*数据泄露*的形式，这是一个泛指使用直接或间接暗示目标变量的特征。

为了说明这种泄露如何导致过拟合，让我们将 20%的数据分成验证集，并计算训练集的`beds_per_price`特征：

```py
from sklearn.model_selection import train_test_split
df_train, df_test = train_test_split(df, test_size=0.20)
df_train = df_train.copy()
df_train['beds_per_price'] = df_train['bedrooms'] / df_train["price"]
df_train[['beds_per_price','bedrooms']].head(5)
```

|   | beds_per_price | bedrooms |
| --- | --- | --- |
|  |
| --- |
| 23525 | 0.0003 | 1 |
| 22736 | 0.0002 | 1 |
| 22860 | 0.0006 | 3 |
| 25510 | 0.0003 | 1 |
| 40540 | 0.0008 | 2 |

作为一条一般原则，你不能使用验证集的信息来计算验证集的特征。在特征工程期间只能使用直接从训练集计算出的信息。

虽然`df_test`中确实有验证集的价格信息，但我们不能用它来计算验证集的`beds_per_price`。在生产环境中，模型将没有验证集的价格信息，因此`df['beds_per_price']`必须仅从训练集创建。（如果模型在实际中包含公寓价格，我们就不需要这个模型了。）为了避免这种常见的陷阱，想象一下逐个计算特征并将验证记录发送给模型，而不是一次性全部发送，这很有帮助。

一旦我们有了训练集的`beds_per_price`特征，我们可以计算一个将卧室映射到`beds_per_price`特征的字典。然后，我们可以使用`map()`函数在卧室列上合成验证集的`beds_per_price`：

```py
bpmap = dict(zip(df_train["bedrooms"],df_train["beds_per_price"]))
df_test = df_test.copy()
df_test["beds_per_price"] = df_test["bedrooms"].map(bpmap)
avg = np.mean(df_test['beds_per_price'])
df_test['beds_per_price'].fillna(avg, inplace=True)
print(df_test['beds_per_price'].head(5))
```

2125 0.000332 47172 0.000667 5584 0.000000 860 0.000294 49025 0.000332 名称：beds_per_price，数据类型：float64

`fillna()`代码处理了验证集有不在训练集中的卧室数量这种情况；例如，有时验证集有一个 7 卧室的公寓，但在`bpmap`中没有等于 7 的卧室键。

现在我们有了训练集和验证集的`beds_per_price`，我们可以仅使用训练集来训练 RF 模型，并仅使用验证集来评估其性能：

```py
X_train, y_train = df_train[['beds_per_price']+numfeatures], df_train['price']
X_test, y_test = df_test[['beds_per_price']+numfeatures], df_test['price']

rf = RandomForestRegressor(n_estimators=100, n_jobs=-1)
rf.fit(X_train, y_train)
oob_overfit = rf.score(X_test, y_test) # don't test training set
print(f"OOB R² {oob_overfit:.5f}")
print(f"{rfnnodes(rf):,d} nodes, {np.median(rfmaxdepths(rf))} median height")

```

OOB R² -0.41233 1,169,770 个节点，平均树高为 31.0

验证集上的-0.412 的![](img/ec985123b9b52e80981e6500795e8d16.png)指标非常糟糕，表明模型缺乏泛化能力。在这种情况下，过拟合意味着模型过分强调了`beds_per_price`特征，这个特征在训练数据中对价格有很强的预测能力，但在验证数据集中却没有。 (该特征仅使用训练数据集的数据计算。)即使在模型的复杂度上，我们也能嗅到过拟合的迹象，因为该模型的节点数只有仅使用数值特征训练的模型的一半。从目标变量中提取特征并不总是错误，我们只需要更加小心。

## 6.5 目标编码分类变量

创建包含目标变量信息的特征被称为*目标编码*，通常用于从分类变量中有效地推导特征。最常见的目标编码之一被称为*平均编码*，它用与该类别相关的平均目标值替换每个类别值。例如，在我们的公寓数据集中，物业管理员可能会在特定的价格范围内出租公寓。物业管理员 ID 本身具有很小的预测能力，但将 ID 转换为他们管理的公寓的平均租金价格可能具有预测性。同样，某些建筑可能比其他建筑有更昂贵的公寓。使用 Pandas 通过按`building_id`分组数据并请求平均值，很容易得到每栋建筑的平均租金价格：

```py
df.groupby('building_id').mean()[['price']].head(5)
```

|   | 价格 |
| --- | --- |
| building_id |
| --- |
| 0 | 3195.9321 |
| 00005cb939f9986300d987652... | 3399.0000 |
| 00024d77a43f0606f926e2312... | 2000.0000 |
| 000ae4b7db298401cdae2b0ba... | 2400.0000 |
| 0012f1955391bca600ec30103... | 3700.0000 |

不幸的是，正如我们在上一节中看到的，在结合目标信息时，很容易过拟合模型。防止过拟合并非易事，最好依赖于经过验证的库，例如 sklearn 贡献的[category_encoders](http://contrib.scikit-learn.org/categorical-encoding/)包。 (为了防止过拟合，想法是从每个类别的训练数据目标子集中计算平均值。)您可以在命令行上使用 pip 安装`category_encoders`：

```py
pip install category_encoders
```

{待办：也许可以展示我的平均编码器。速度更快。值得在某处解释。rent/mean-encoder.ipynb 看起来我们需要一个 alpha 参数，将稀有猫类推向平均值；应该会工作得更好。}

这是如何使用`TargetEncoder`对象对数据集中的三个分类变量进行编码并获取 OOB 分数的方法：

```py
from category_encoders.target_encoder import TargetEncoder
df = df.reset_index() # not sure why TargetEncoder needs this but it does
targetfeatures = ['building_id']
encoder = TargetEncoder(cols=targetfeatures)
encoder.fit(df, df['price'])
df_encoded = encoder.transform(df, df['price'])

X, y = df_encoded[targetfeatures+numfeatures], df['price']
rf, oob = test(X, y)

```

使用 2,746,024 个树节点和 39.0 的中值树高度，OOB R² 为 0.87236

那个![](img/ec985123b9b52e80981e6500795e8d16.png)分数略好于仅数值特征的基线 0.868。然而，考虑到模型使用目标编码容易过拟合的趋势，测试模型时使用验证集是个好主意。让我们将 20%的数据作为验证集，并为数值特征得到一个基线：

```py
df_train, df_test = train_test_split(df, test_size=0.20)

# TargetEncoder needs the resets, not sure why
df_train = df_train.reset_index(drop=True)
df_test = df_test.reset_index(drop=True)

X_train = df_train[numfeatures]
y_train = df_train['price']
X_test = df_test[numfeatures]
y_test = df_test['price']

rf = RandomForestRegressor(n_estimators=100, n_jobs=-1)
rf.fit(X_train, y_train)
s_validation = rf.score(X_test, y_test)
print(f"{s_validation:4f} score {rfnnodes(rf):,d} tree nodes and {np.median(rfmaxdepths(rf))} median tree height")

```

0.845264 分数，2,126,222 个树节点，平均树高为 35.0

验证分数和 OOB 分数非常相似，证实了 OOB 分数是验证分数的一个很好的近似。有了这个基线，让我们看看当我们正确地对验证集进行目标编码时会发生什么，也就是说，只使用训练集的数据（警告：这需要几分钟才能执行）：

```py
enc = TargetEncoder(cols=targetfeatures)
enc.fit(df_train, df_train['price'])
df_train_encoded = enc.transform(df_train, df_train['price'])
df_test_encoded = enc.transform(df_test)

X_train = df_train_encoded[targetfeatures+numfeatures]
y_train = df_train_encoded['price']
X_test = df_test_encoded[targetfeatures+numfeatures]
y_test = df_test_encoded['price']

rf = RandomForestRegressor(n_estimators=100, n_jobs=-1)
rf.fit(X_train, y_train)
s_tenc_validation = rf.score(X_test, y_test)
print(f"{s_tenc_validation:.4f} score {rfnnodes(rf):,d} tree nodes and {np.median(rfmaxdepths(rf))} median tree height")

```

0.8488 分数，2,378,442 个树节点，平均树高为 38.0

![![](img/46f7e418ff7e78793994baaebbaf0320.png)](images/catvars/catvars_targencode_6.svg)

数字加目标编码特征的验证分数（0.849）低于仅数字特征的验证分数（0.845）。模型发现目标编码特征对训练集价格有很强的预测性，导致它过度拟合，过分强调这个特征。这种泛化损失解释了验证分数的下降。特征重要性图提供了这种过度强调的证据，因为它显示目标编码的 `building_id` 特征是最重要的。

虽然对这组数据没有帮助，但许多实践者和 Kaggle 竞赛获胜者报告说目标编码是有用的。了解这项技术并学会正确应用它是值得的（使用仅来自训练集的数据计算验证集特征）。

当我们从给定数据集中提取特征的技巧用尽时，有时从外部数据源提取特征是有益的。

## 6.6 注入外部社区信息

我们的数据集包含经纬度坐标，但一个更明显的价格预测因子可能是一个表示社区的分类变量，因为一些社区比其他社区更受欢迎。然而，鉴于我们上面看到的分类变量的问题，一个数值特征将更有用。与其识别社区，不如将靠近高度受欢迎社区的邻近性作为一个数值特征。福布斯杂志有一篇文章，[根据当地人的意见，纽约市最值得居住的 10 个社区](https://www.forbes.com/sites/trulia/2016/10/04/the-top-10-new-york-city-neighborhoods-to-live-in-according-to-the-locals/#17bf6ff41494)，从中我们可以获取社区名称。然后，使用地图网站，我们可以估算这些社区的经纬度并将它们记录如下：

```py
hoods = {
    "hells" : [40.7622, -73.9924],
    "astoria" : [40.7796684, -73.9215888],
    "Evillage" : [40.723163774, -73.984829394],
    "Wvillage" : [40.73578, -74.00357],
    "LowerEast" : [40.715033, -73.9842724],
    "UpperEast" : [40.768163594, -73.959329496],
    "ParkSlope" : [40.672404, -73.977063],
    "Prospect Park" : [40.93704, -74.17431],
    "Crown Heights" : [40.657830702, -73.940162906],
    "financial" : [40.703830518, -74.005666644],
    "brooklynheights" : [40.7022621909, -73.9871760513],
    "gowanus" : [40.673, -73.997]
}

```

为了综合新的特征，我们计算了所谓的 *曼哈顿距离*（也称为 *L1 距离*），这是从每个公寓到每个社区中心的距离，它衡量一个人必须走过多少街区才能到达社区。相比之下，*欧几里得距离*将衡量飞鸟飞行的距离，穿过建筑物的中间。

```py
for hood,loc in hoods.items():
    # compute manhattan distance
    df[hood] = np.abs(df.latitude - loc[0]) + np.abs(df.longitude - loc[1])

```

在数值特征和这些新的社区特征上训练模型，使我们的性能有了相当大的提升：

```py
hoodfeatures = list(hoods.keys())
X, y = df[numfeatures+hoodfeatures], df['price']
rf, oob_hood = test(X, y)

```

OOB R² 0.87213 使用 2,412,662 个树节点，平均树高为 43.0

OOB 得分为 0.872，明显优于基线 0.868 的数值特征。树的数量显著增加，但节点数量大致相同。模型需要更努力地将类似公寓与类似的租金价格联系起来，但这种额外的复杂性对于性能的提升是值得的。

新的邻近度特征有效地将公寓相对于理想社区进行三角定位，这意味着我们可能不再需要公寓的经纬度。删除经纬度并重新训练模型显示出类似的 OOB 得分和更浅的树：

```py
X = X.drop(['longitude','latitude'],axis=1)
rf = RandomForestRegressor(n_estimators=100, n_jobs=-1, oob_score=True)
rf.fit(X, y)
print(f"{rf.oob_score_:.4f} score {rfnnodes(rf):,d} tree nodes and {np.median(rfmaxdepths(rf))} median tree height")

```

0.8700 得分，2,421,776 个树节点和 41.0 的中位树高

按照一般规则，注入外部数据绝对值得一试。作为另一个例子，考虑预测商店或网站的销售。我们发现，注入表示发薪日或国家假日的列通常有助于预测销售量的波动，而这些波动用现有的特征解释得并不好。

## 6.7 我们的最终模型

在本章中，我们探讨了针对分类变量的许多常见特征工程技术，现在让我们将它们全部放在一起，看看组合模型的表现如何：

```py
X = df[['interest_level']+textfeatures+hoodfeatures+numfeatures]
rf, oob_combined = test(X, y)

```

使用 4,842,474 个树节点和 44.0 的中位树高，OOB R² 为 0.87876

虽然从绝对值来看，OOB 得分为 0.879 并不比我们的仅数值模型基线 0.868 大太多，但这意味着我们已经从数据中榨取了额外的 8.321%相对精度（使用`((oob_combined-oob_baseline) / (1-oob_baseline))*100`计算）。

![图片](img/5ef6c0f12152db6ffb39acb86f885f8e.png)(images/catvars/catvars_cats_42.svg)

需要注意的是，随机森林模型不会因为所有这些特征的组合而混淆，这也是我们推荐使用随机森林的原因之一。随机森林简单地忽略那些预测能力不强的特征。右侧的特征重要性图显示了模型认为具有预测性的特征。出于模型准确性的考虑，保留所有特征是个好主意，但为了简化模型以便于解释（在解释模型行为时），我们可能想要移除不重要的特征。

## 6.8 分类特征工程总结

{待总结：总结本章的技术}
