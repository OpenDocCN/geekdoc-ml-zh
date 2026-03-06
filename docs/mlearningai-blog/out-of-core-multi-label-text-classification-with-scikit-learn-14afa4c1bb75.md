# 使用 scikit-learn 进行核外多标签文本分类

> 原文：<https://medium.com/mlearning-ai/out-of-core-multi-label-text-classification-with-scikit-learn-14afa4c1bb75?source=collection_archive---------4----------------------->

在促进分类和回归任务并支持 NLP/机器学习项目的各种开源库中，scikit-learn 无疑是任何 it 生态系统中最通用和最容易引入的包之一。
事实上，scikit-learn 为一个(ml)门外汉的全栈开发人员实现了机器学习的民主化，这比我们所知道的要多得多。
拥有一个重要的评估器和算法库，它使得快速构建 NLP 或机器学习模块的任务非常快速，
并且对于大部分功能目的来说相当有效。

虽然有太多的文本分类示例处理有限的语料库和有限的数据类别，但下面的文章试图揭示核外学习，即无法放入工作站主存储器的训练数据，以及如何使用 intrepid scikit-learn 处理如此大的语料库文本分类任务。

因此，让我们快速浏览一下 scikit——了解它所有的语法简单性和对大型数据集进行有效文本分类的直接方法。
为了便于说明，我们将采用包含药品名称、患者情况和对前者的评论的药品评论数据集。
我们将通过 skicit-learn 实现的估计器传递合理的大样本集，并训练模型从评论中学习，以反向预测状况——这不是该数据集的最实际应用，但它适合我们目前的目的。

应该注意的是，选择这个特定的数据集是因为它的高维度，此外还有正好适合我们的程序案例
的语料库大小。

关于特定的数据集和数据集的引用，请参见文章结尾。

现在来看实际的代码；请参考 github repo[https://github.com/kundanj/text-class-ooc-sk](https://github.com/kundanj/text-class-ooc-sk)获取相关代码。

*Pre-req* : python 技巧，git repo 中脚本安装请见自述。

**大文本分类:**

首先，让我们直接使用 pandas，让它做它最擅长的事情——分割 CSV 文件以获得我们需要的数据。

```
df=pd.read_csv(FILE_PATH + “/../data/drugsComTrain_raw.csv”)
all_conditions = df[‘condition’]
all_reviews = df[‘review’]
```

或许看一眼数据就能知道我们在处理什么。

```
print(all_reviews.head(10))
print(all_conditions.head(10))
print(all_reviews.shape)
```

所以我们有了语料库(所有样本中的整个字典集)和作为 pandas 系列整齐排列的类，这形成了我们训练练习的基础。

有关类别/条件的快速主列表

```
for category in all_conditions:
    if category not in classes:
        classes.append(category)
```

我们将使用这些数据进行培训。但在此之前，我们需要将数据转换成机器可读的格式。
对于那些迟到的人，机器学习评估人员的工作是数字构建，而不是文本漫谈。所以我们需要将文本转换成数字，并为每个数据样本(评论)中的单词找到正确的加权表示，这将确定正确的适用类别(条件)。
对于标签来说，这很容易做到

`y_all = [ classes.index(x) for x in all_conditions ]`

然而，对于评论来说，它并不像一维系列或数组那样简单。
因此，我们将评论数组矢量化为矩阵，即将可变长度的文本样本转换为固定长度的数字表示。此外，“矢量化矩阵”听起来比简单地说“使用字符串数组”更酷！
出现了著名的单词袋模型，其中每个单词都是大小矩阵(语料库中的单词数:nw) X(样本集大小:ns)中的一个数据点。
这基本上意味着每个样本由包含在样本中的词的指示来表示，这种指示被示为相应词在组合语料库词典中的代表位置。因此，大小为 nw×ns 的矩阵中的样本 j 将是包含 nw 个元素的向量，其中仅沿着 j 填充了表示来自特征集 NW 的对应单词和频率计数的那些数字。这个想法是让样本中各个单词的出现频率决定它的类别。
这实际上听起来相当初级，可能并不总是转化为对类别的最佳猜测，例如，在一个关于技术文章样本的博客中，像“技术”和“计算机”这样的词可能会大量出现在所有文档中，认为这些词代表类别分类可能会产生误导(例如，文章是在谈论移动应用还是基因工程)。因此，我们使用 TF-IDF 转换器对样本进行矢量化，降低样本中所有常见单词的出现频率，削弱它们影响分类的能力。
现在我们似乎有了一种可行的方法来表示每个样本中重要单词的加权数字，这些可以输入到机器学习算法中。然而，我们还有一个问题。
根据我们语料库中的数据类型(885 个类别中的 161297 个样本)和大约 137102 个样本(据报道有 262144 个最大特征)进行训练，我们正在讨论一个 137102 X 262144 X 4(假设仅用整数表示单词)字节的矩阵。这是需要与大多数 scikit 用户熟悉的通常的 CountVectorizer-TF-IDF 传统进行特殊偏离的地方。
进入 HashVectorizer，它以简单的形式，使用低内存特性哈希将文本文档转换成 scipy.sparse 矩阵，该矩阵可扩展用于大型数据集。它是一个无状态的矢量器，所以不需要调用 fit。然而，它有相当多的缺点，不适合 IDF 评分，并可能导致哈希冲突，这就是为什么我们使用大量的 max 功能(> 2**18)。
然而，文档暗示了我们可以将其转化为 TF-IDF 转换的可能性，因此这就是我们将要做的。

正如通常在 NLP 管道中提到的，在特征提取之前，可以对文本样本进行一定量的预处理，这可以包括(单词/字符)标记化，随后是停用词消除、词条化等。其中大部分已经由 HashVectorizer 完成(小写转换，忽略标点符号等)
然而，自定义预处理(例如停用词)的效果可能是问题特定的，可以根据具体情况包含在管道中。
我们有责任在此提出另一个警告——单词袋/n 元语法方法忽略了文本中的位置信息，这意味着上下文相关性、语义等被忽略。

```
 #as the name suggests, split training data
    X_train, X_test, Y_train, Y_test = train_test_split(clean_corpus(all_reviews), y_all, test_size=0.15, random_state=42)
    vectorizer = HashingVectorizer(ngram_range=(1,2), strip_accents='ascii', n_features=2**18)
    X_train_hashed = vectorizer.transform(X_train)
    tfidf_transformer=TfidfTransformer()
    X_train = tfidf_transformer.fit_transform(X_train_hashed)
    X_test_hashed = vectorizer.transform(X_test)
    X_test = tfidf_transformer.transform(X_test_hashed)
```

请注意，您可以使用这个数据集附带的单独测试数据，而不是分割训练数据。
上述大多数函数调用都是参数语义，除了需要快速提及的 n-gram 除了单个单词特征，我们还采用了双单词，例如，潜在地适应“头痛”和“慢性头痛”之间的细微差别。

最后是模型。我们使用 SGD 分类器，在这种情况下，它是线性 SVM 的优化器。我们将使用 SGD 作为拟合线性分类器的方法。
无需深入研究模型背后的数学原理(这超出了本文的范围),线性模型意味着一种代数方程形式，其中估计的目标是加权特征的线性总和——考虑系数和截距。
选择 SGD 是因为它在处理大量样本和特征时非常有用。它正好符合核心外学习的部分适应机制。
我们将线性 SVM 用于参数“铰链”指定的损失函数。

此外，从程序上讲，SGD 对增量拟合有一个方便的适应性，我们将在下面的代码块中看到。SGD 是适合于增量学习的一组估计器中的一个，也就是说，整个语料库不会一次加载到算法中。

虽然在我们的例子中，大规模的语料库可能不适合单个镜头中的模型，但在我们使用 partial_fit
将训练集分成批次的情况下，需要增量步骤。

```
 epoch = 5
    batchsize = 1000
    model = SGDClassifier(loss="hinge", penalty="l2")
    batches = int(X_train.shape[0]/batchsize) + 1
    samples = X_train.shape[0]
    for i in range(epoch):
        for j in range(batches):
            #print('in j...', j, j*batchsize, '----2is:',samples, (j+1)*batchsize )
            model.partial_fit(X_train[j*batchsize:min(samples,(j+1)*batchsize)], Y_train[j*batchsize:min(samples,(j+1)*batchsize)], classes=range(len(classes)))
    print ("Accuracy on testing data :", epoch, model.score(X_test, Y_test))
```

我们得到了 71%的准确率，这是一个很好的起点。如果您需要使用模型，请使用 model.predict 根据症状对医疗状况进行分类，或者使用下面的代码结构。

```
 test_stmt = []
    test_stmt.append(input_statement)
    X_testing_counts = vectorizer.transform(clean_corpus(test_stmt))
    X_testing = tfidf_transformer.transform(X_testing_counts)
    predicted = model.predict(X_testing)
    for doc, category in zip(test_stmt, predicted):
    return classes[category]
```

微调模型超参数和历元迭代可能会有所帮助，但是增加批量大小通常不会有助于提高准确性，特别是在 SGD 中。

这就是 scikit learning 对大文本分类的简明快速的介绍。编码快乐！

*数据集和引用:*

[https://www . ka ggle . com/Jessica Li 9530/kuc-hackathon-winter-2018？select=drugsComTest_raw.csv](https://www.kaggle.com/jessicali9530/kuc-hackathon-winter-2018?select=drugsComTest_raw.csv)

*数据集鸣谢:
数据集最初发布在 UCI 机器学习知识库上。引文:费利克斯·格雷尔、苏亚·卡鲁马迪、哈根·马尔贝格和塞巴斯蒂安·萨恩塞德。2018.基于方面的跨领域和跨数据学习的药物评论情感分析。在 2018 年数字健康国际会议(DH '18)的会议录中。美国纽约州纽约市 ACM，121–125。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)