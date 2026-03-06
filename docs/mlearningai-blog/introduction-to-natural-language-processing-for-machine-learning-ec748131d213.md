# 机器学习的自然语言处理导论

> 原文：<https://medium.com/mlearning-ai/introduction-to-natural-language-processing-for-machine-learning-ec748131d213?source=collection_archive---------2----------------------->

![](img/a42de31c18ed057d27d65b0735d68055.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们周围有很多文本。我们在书籍、文章、评论和报纸上看到它。使用这些文本并将其转换为机器学习和深度学习算法可以轻松理解的形式将是非常明智的。结果，他们会得到处理过的文本，并给出不同用例的预测。

![](img/7a2c5b7bc64d701e4f636633d288cdb0.png)

Photo by [Sangga Rima Roman Selia](https://unsplash.com/@sxy_selia?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自然语言处理(NLP)是指将自然文本转换成可用于机器学习的形式。换句话说，它包括在软件的帮助下改变语音和文本。自然语言有不同的转换方式，让我们回顾一下其中的一些方法。自然语言处理(NLP)的目标是从文本中提取洞察力和有价值的信息，分别用于机器学习预测。现在，让我们深入下面的一些自然语言处理方法。

以下是分别用于转换文本和语音的一些自然语言处理(NLP)方法。

1.  情感分析
2.  命名实体识别(NER)
3.  词干化和词汇化
4.  标记化
5.  一袋单词
6.  句子分割
7.  文本摘要
8.  文本嵌入

下面让我们简单讨论一下上面提到的话题。

# **1。情绪分析**

![](img/0548383aa9b50c958afd090c34aa7739.png)

Photo by [Debby Hudson](https://unsplash.com/@hudsoncrafted?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果借助机器学习和数据科学来分析和理解文本中的情感，那将是非常好的。有时，在一篇文章中可能会描绘出不同的情感，如快乐、悲伤、欲望和愤怒。使用自然语言处理(NLP)，可以提取不同文本中存在的含义和情感，以便整体上可以用于不同的用例，例如通过他们的评论分别理解购买特定产品的客户的行为。

# **2。命名实体识别**

![](img/1e465db4e7876d1b4a0d10af2dfdcb92.png)

Photo by [TJ Arnold](https://unsplash.com/@missinformed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

命名实体识别，简称 NER，用于自然语言处理，其中不同的名词、副词、日期和其他重要的实体被分割在不同的文本中。换句话说，它识别文本中的不同实体，并对这些值进行分类，以便这些值可以很好地进行机器学习理解和预测。这可以用于纠正不同句子中存在的语法错误，这些句子可以用于不同的机器学习目的。此外，文本中的重要信息可以被容易地识别，并且将分别对不同的机器学习目的产生影响。

# **3。词干化和词汇化**

![](img/8029e04bb97162aba16a50163d4839c8.png)

Photo by [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

词干提取是一个用词根替换有意义的单词的过程。需要注意的一点是，在词干化的过程中，词根可能有意义，也可能没有意义。事实上，它甚至可能不在字典里。完成该过程是因为它保证了减少相似但具有不同形式的单词。因此，这减少了将给予机器学习模型的向量的维度。重要的是要理解，当向量的维度和稀疏度增加时，机器学习算法在不同用例上表现不佳的可能性更高。因此，我们使用堵塞的过程。

词汇化类似于词干化，只是前者中的词根有一些来自文本的含义。使用这一过程时，通常会保留文本的含义。这与词义保留在词根中的词汇化过程有关。因此，词汇化比词干化更可取。

# **4。标记化**

![](img/6b69e1c9bd8859a5f5537757ee1741b8.png)

Photo by [Jeremy Zero](https://unsplash.com/@jeremy0?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当执行标记化时，文本被分成不同的句子、单词或字母，结果它们可以被提供给机器学习模型。看一看文本处理和预测的最新模型，我们可以理解这些模型在不同的段落中与标记一起工作。因此，将给定的文本转换成标记对于深度学习和机器学习来说非常方便。因此，人们必须花时间理解整个文本，并在公正的标记的帮助下提取一些有用的信息。

# **5。一袋话**

![](img/1f4f03155e8b527b49523c745b78db06.png)

Photo by [Arno Senoner](https://unsplash.com/@arnosenoner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当使用单词袋时，文本被转换成单词作为表格中的列，并且基于它们的计数，为列添加数值。举例来说，文本被转换成一个表，表中的列是文本中的单词，显示的值是考虑的单词数。使用 2-gram 方法也很好，其中每两个单词作为列，它们的出现分别添加到创建的表中。在执行机器学习分析时，这种方法非常方便。然而，在这种方法中文本中的语义没有被记录，因此，存在意义的损失。

# **6。句子分割**

![](img/da4e054de49e3589157caa740986bf2c.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

句子分割是将整个文本分成不同句子的过程。这将标记文本中的句子，它们将被索引和考虑。因此，它将使机器学习任务变得容易。例如，可以在我们的数据中添加一些有用的功能，如查找句子的平均长度，以获得可靠的预测。结果，将产生有价值的特征，这些特征将分别有助于机器学习过程。

# **7。文本摘要**

![](img/0405bc2950d599399abc270969063dd4.png)

Photo by [Aaron Burden](https://unsplash.com/@aaronburden?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

顾名思义，文本摘要是从文本中提取有用信息，并用几句话概括整个文本的过程。这在机器学习中非常方便，因为这将完全降低向量的维数，并分别提高计算速度。因此，概括的文本可以再次被标记化，并且可以执行各种其他自然语言处理任务。

# **8。文本嵌入**

![](img/f034105c2195763727d40d847382f0ec.png)

Photo by [Chris Liverani](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

通常，文本嵌入包括将给定文本转换成包含与其他单词相似度值的向量。这导致保留单词和句子的上下文和语义意义。此外，作为该方法的结果，可以生成文本的情感和其他有用的机器学习预测。在我们已经讨论过的其他方法中，如单词袋，文本的上下文和语义含义没有被保留。然而，在文本嵌入中，需要考虑对不同文本的上下文理解。当执行自然语言处理时，我强烈建议应用文本嵌入，因为它们对不同的机器学习任务给出的预测是好的。

# 结论

总而言之，我们已经讨论了不同的自然语言处理(NLP)方法，这些方法可以用来从文本中提取有价值的信息。一旦文本被转换成有用的形式，它就可以被赋予不同的机器学习和深度学习模型。因此，这将导致更好的机器学习性能和数据科学生产力周期的整体改善。希望这篇文章对你有所帮助。请随意分享你的想法。谢谢！