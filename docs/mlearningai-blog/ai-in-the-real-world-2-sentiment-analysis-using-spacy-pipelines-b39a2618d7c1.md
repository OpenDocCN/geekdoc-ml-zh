# 现实世界中的人工智能-2。使用空间管道的情感分析

> 原文：<https://medium.com/mlearning-ai/ai-in-the-real-world-2-sentiment-analysis-using-spacy-pipelines-b39a2618d7c1?source=collection_archive---------1----------------------->

![](img/40704e711fe395788f146b37962041d2.png)

人们表达思想的方式和语气高度重视世界的发展，如果每个人都相互理解并相互谦让，那么就不会有比幸福与和平更少的东西了。好吧，这让我们开始思考简单句的情感分析如何在任何领域产生更大的影响。情感分析可以用两种学习方法来完成:

*   **无监督学习:**由于我们都对什么是无监督学习有一个公平的想法，所以我们在这里可以做的是为每组单词创建一个嵌入，并找到任何句子嵌入的语义相似性，以获得特定句子可能属于的组。
*   **监督学习:**作为非监督学习方法的对应，这里我们将使用标记数据进行建模。

**空间管道:**

如果你之前没有听说过 spaCy 这个词，那么你应该快速跳转到上面的链接，他们是 NLP 世界的先驱之一。与 TectBlob、NLTK 等相比，spaCy 提供了更健壮、更高效的管道和功能。现在他们增加了将变形金刚也整合到他们的模块中的能力，用于训练和推理目的。不幸的是，spaCy 没有内置的情感分析模块，尽管幸运的是，他们有一个文本分类模块，可以定制训练，并作为一个模型。你所需要做的就是挑选一个数据集，按照 spaCy 的格式标记和配置它们，挑选一个基本模型配置，这就是全部。我已经训练并打包了一个简单的情绪分析模块，可以通过 pip 轻松安装在你的机器上。

**训练:**

现在，培训 spaCy 管道与以往一样简单，我们需要所需格式的数据，选择基本模型配置并培训您的管道。对于情感分析模型，我使用了来自 UCI ML 知识库的数据集[的 IMDB 评论。要培训您自己的管道，请查看下面的](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences)[笔记本](https://github.com/Vishnunkumar/spacysentiment/blob/main/spacy-sentiment.ipynb)。下面是笔记本的一个样本片段

**包装**

在训练和保存使用 spaCy 管道创建的模型后，可以将它们打包并作为编译和可执行版本提供。要打包保存的模型，请使用 spaCy CLI 中给出的以下命令。

```
python -m spacy [package](https://spacy.io/api/cli#package) *input_dir* *output_dir*
```

*   input_dir:包含编译模型的目录
*   output_dir:创建可分发文件的目录，该文件可以使用 pip 安装

其他参数查看 spaCy [包](https://spacy.io/api/cli#package)官网。后期打包您必须遵循传统的方法，将 dist/ folder 上传到您的 PyPi 存储库，如果您愿意的话，让它对社区可用。

**eng _ spacy 情操**

我们已经看到了如何训练和打包 spaCy 管道，现在让我们来看一个实际的管道，下面是一个简单的 python 库，用于英语句子的情感分析。您可以使用后面的[来安装软件包](https://pypi.org/project/eng-spacysentiment/)

`pip install eng-spacysentiment`

*   简单实现

好了，这是一个总结伙伴，我们现在可以轻松地使用 spaCy 训练管道，使用 python 将其打包并部署为可执行文件。一如既往，非常感激和高兴你一直支持我。下次再见，小心伙计们，如果你喜欢这篇文章，请为它鼓掌。跟着我学更多，我也会这样做，这样我们可以互相学习每件事。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)