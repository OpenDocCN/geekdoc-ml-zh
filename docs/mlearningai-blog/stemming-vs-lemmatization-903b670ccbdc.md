# 词干化与词汇化

> 原文：<https://medium.com/mlearning-ai/stemming-vs-lemmatization-903b670ccbdc?source=collection_archive---------8----------------------->

## **学习词干分析和词汇化基础的终极资源。**

![](img/00cd361cf647b5a0176536ad421375a8.png)

Photo by [Brett Jordan](https://unsplash.com/es/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我肯定你熟悉自然语言处理(NLP)，但如果你不熟悉，你可以读一下我的文章“ [**【了解自然语言处理(NLP)**](/codex/understanding-natural-language-processing-nlp-fe52f4f66824)**”。我将在这里帮助您理解用例。**

**NLP 通常用于检查文本数据，机器学习模型将通过将文本转换为向量(数字表示)来帮助您分析文本。但是首先，我们必须将文本数据转换成我们将要开发的模型能够理解的格式。除非将文本转换成向量，否则我们的模型将无法理解文本。因此，将文本转换成矢量是至关重要的。**

**要做到这一点，我们必须首先将文本简化为最简单的形式，这需要使用词干化和词汇化等技术。这些基本单词将被转换成向量。搜索引擎和聊天机器人利用词干分析和词汇化来确定单词的意思。因此，将文本转换成向量是开始自然语言处理的第一步。**

> ****简而言之，词干化和词汇化都试图将单词简化为最简单的形式。****

**[](/codex/python-vs-r-72d025abe089) [## Python vs R

### 了解 Python 和 R 之间基本区别的终极指南

medium.com](/codex/python-vs-r-72d025abe089)** 

## **什么是词干？**

**术语化是从单词中去除词缀以恢复其基本形式的过程。这就像把树枝修剪到树干一样。例如,“吃”、“吃”和“被吃”这三个词的词根是“吃”。**

**搜索引擎使用词干来索引单词。因此，搜索引擎可以简单地保存词干，而不是保存一个单词的所有版本。词干化最小化了索引的大小，同时提高了检索的准确性。**

> ****为了进一步理解这一点，考虑下面的例子。****

*****要开始，我们必须导入自然语言工具包(nltk)。*****

```
**import** nltk
```

*****导入 PorterStemmer 类实现波特斯特梅尔算法。*****

```
**from** nltk.stem **import** PorterStemmer
```

*****然后，如下图所示，创建波特斯特梅尔类的一个实例。*****

```
ps **=** PorterStemmer()
```

*****现在输入你要词干的单词。*****

```
words **=** ["active", "actives", "activate", "activated", "activating"]

**for** w **in** words:
    print(w, " : ", ps**.**stem(w))**--------------------------------------------------------------------
# OUTPUT
--------------------------------------------------------------------**active  :  activ
actives  :  activ
activate  :  activ
activated  :  activ
activating  :  activ
```

**像“主动”、“激活”、“被激活”、“激活”这些词，都改成了它们的核心词“activ”。“activ”这个词没有任何意义吧？词干算法通过从单词中移除后缀来运行。从更广泛的意义上来说，它删除了单词的开头或结尾。**

**这就是去梗的过程。让我们进入词汇化。**

## **什么是词汇化？**

**符号化类似于词干化，但是词干化并不总是提供有意义的表示，但是符号化将帮助你获得容易理解的有意义的表示。**

> ****为了进一步理解这一点，考虑下面的例子。****

*****要开始，我们必须导入自然语言工具包(nltk)。*****

```
**import** nltk
```

*****导入 WordNetLemmatizer 类实现 Lemmatizer 算法*****

```
**from** nltk.stem **import** WordNetLemmatizer
```

*****创建 lemmatizer 类的实例*****

```
lemmatizer **=** WordNetLemmatizer()
```

*****现在输入您想要词条的单词。*****

```
print("active", " : ", lemmatizer**.**lemmatize("active"))
print("actives", " : ", lemmatizer**.**lemmatize("actives"))
print("activate", " : ", lemmatizer**.**lemmatize("activate"))
print("activated", " : ", lemmatizer**.**lemmatize("activated"))
print("activating", " : ", lemmatizer**.**lemmatize("activating"))**--------------------------------------------------------------------
# OUTPUT
--------------------------------------------------------------------**active  :  active
actives  :  active
activate  :  activate
activated  :  activated
activating  :  activating
```

**这将比词干更有意义。Lemmatizer 减少写作中的歧义。本质上，它将把所有意思相同但表达不同的单词返回到它们的基本形式。它降低了所提供文本中的单词密度，并有助于为机器学习准备正确的特征。数据越干净，你的机器学习模型就越智能、越准确。NLTK Lemmatizer 还将节省内存和计算成本。但是，等等，我们为什么要这么做？**

## **为什么需要词干化和词汇化？**

**当评估文本或短语时，你必须识别基本单词(词干)。这会让你了解他们的感受。当执行文本分析并试图发现餐馆评论(好的或坏的)，或者确定电子邮件是否是垃圾邮件时。**

**假设你正在做一个需要你识别电影感觉的项目。你有评论，可以选择它们是有利的还是不利的。**

**要做到这一点，你必须首先确定词根；这个词将帮助你分析情绪。如果不进行词干化或词干化，要完全完成它是不可行的。**

## **为什么词汇化比词干化更可取？**

**另一方面，emmatization 是一个更强大的过程，它考虑到了单词的形态学检查。它返回其所有屈折形式的基本形式，即引理。为了编写词典并找到单词的正确形式，大量的语言学专业知识是必要的。词干化是一个广泛的过程，但是词汇化是一个智能的操作，它在字典中寻找正确的形式。因此，词汇化有助于形成优秀的机器学习特征。**

## **结论**

**当你查看" activate "和" activated "的词干时，结果是相同的" activ ",然而，NLTK lemmatizer 为" activate "和" activated "这两个单词" activate "产生一个单独的引理。所以，如果我们需要创建一个特征集来训练计算机，*将是理想的。***

***[](/mlearning-ai/analyzing-ibm-employee-attrition-ec9b8b9f5b0e) [## 分析 IBM 员工流失

### 识别影响员工流失的因素

medium.com](/mlearning-ai/analyzing-ibm-employee-attrition-ec9b8b9f5b0e) 

感谢您的阅读！如果你关注我或与他人分享这篇文章，并关注更多有趣的故事，我将不胜感激。最美好的祝愿。

## 你会支持 awesome❤️

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)***