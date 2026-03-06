# VADER(效价感知词典和情感推理机)情感分析

> 原文：<https://medium.com/mlearning-ai/vader-valence-aware-dictionary-and-sentiment-reasoner-sentiment-analysis-28251536698?source=collection_archive---------1----------------------->

![](img/ee5cec4c651ddc007ecd48d50eb387ff.png)

Stock Market, people, painting, chairs, chart, chairman, wall, artwork [13]

情感分析是一种通过计算确定一篇文章是积极的、消极的还是中性的方法。它也被称为信息提取，因为它涉及评估演讲者的观点或心态[1]。

为什么进行情感分析如此困难？

1.**讽刺**:根据不同的发送者或场合，一句讽刺话中隐含的词语或文本数据有不同的含义。讽刺是当你说了一些与你想说的截然相反的话。

2.**解码** —句子的主语和宾语是什么，动词或形容词与谁有关？

3.**命名实体识别** —这个人到底在说什么？

4.**Tweets**-大写字母、拼写错误的单词、标点符号、缩写和语法结构。

**情感分析是如何工作的？**

传统的方法包括字典或基于意义的分析。但是通过使用机器学习，我们可以从文本中生成“特征”,并使用这些特征来预测“标签”

示例-拆分文本à单词à通过使用数据可视化方法(单词云)对情感进行分类，使用这些单词找到它们的频率

典型的情感分析包括这些步骤，

文本输入à预处理和数据清理

标记化(单词和句子)

删除停用词

规范化单词(词干化和词条化)

文本矢量化

**情感分类的基本工作流程如下:**

1.  *将数据拆分为训练和测试数据。*
2.  *选择模型架构。*
3.  *用“训练数据”训练模型。*
4.  使用测试数据评估模型的性能。
5.  *将训练好的模型应用于新数据以进行预测，在这种情况下，预测值将是介于-1.0 和 1.0 之间的数字。*

**VADER(价觉词典和情感推理机)情感分析:**

这是一个词汇数据库和基于规则的情绪分析工具，针对社交媒体情绪进行了优化。它利用了多种技术。情感词典是词汇特征(例如，单词)的集合，这些词汇特征根据它们的情感极性被分类为积极的或消极的。它不仅展示了积极和消极的分数，还展示了情绪积极或消极的程度[1]。

命令安装**Vader 情操【5】**【6】:

***pip 安装向导***

请参考[1]来理解 Vader perspection 获得的结果，我遵循了这篇[11]文章来使用 python 实现 Vader perspection:

检查[6]VADER 分析评论。

**使用 VADER 的好处【9】【10】:**

*   它在社交媒体文本上表现良好，同时易于推广到不同的学科。
*   对于许多实现来说，这种方法很容易理解，例如评估公众情绪、执行市场分析或改善客户服务。
*   它擅长分析大型数据集。

**劣势**

*   有可能带有偏见的术语，定义，表情符号
*   讽刺
*   由于拼写错误的单词和语法错误，评估可能会遗漏重要的单词

**参考文献**:

[1][https://www . geeks forgeeks . org/python-opinion-analysis-using-Vader/](https://www.geeksforgeeks.org/python-sentiment-analysis-using-vader/)

[2][https://real python . com/情操分析-python/#机器学习-工具](https://realpython.com/sentiment-analysis-python/#machine-learning-tools)

[https://www.mdpi.com/2076-3417/11/18/8438/htm](https://www.mdpi.com/2076-3417/11/18/8438/htm)

[https://github.com/cjhutto/vaderSentiment](https://github.com/cjhutto/vaderSentiment)

[https://pypi.org/project/vaderSentiment/](https://pypi.org/project/vaderSentiment/)

[6][https://towards data science . com/sensational-analysis-using-Vader-a 3415 fef 7664 #:~:text = VADER % 20(% 20 valence % 20 aware % 20 dictionary % 20 for，intensity % 20(strength)% 20 of % 20 emotion。%20a 的% 20 情感% 20 分数% 20，每个% 20 单词% 20 in % 20 text](https://towardsdatascience.com/sentimental-analysis-using-vader-a3415fef7664#:~:text=VADER%20(%20Valence%20Aware%20Dictionary%20for,intensity%20(strength)%20of%20emotion.&text=The%20sentiment%20score%20of%20a,each%20word%20in%20the%20text)。

[7][https://stack overflow . com/questions/4806176/情感分析意见挖掘中最具挑战性的问题是什么](https://stackoverflow.com/questions/4806176/what-are-the-most-challenging-issues-in-sentiment-analysisopinion-mining)

[8]休顿，C.J .和吉尔伯特，E.E. (2014)，题为“VADER:一个基于简约规则的社交媒体文本情感分析模型”

[9][https://www . code project . com/Articles/5269447/Pros-and-consensus-of-NLTK-sensation-Analysis-with-VADE](https://www.codeproject.com/Articles/5269447/Pros-and-Cons-of-NLTK-Sentiment-Analysis-with-VADE)

[10][https://medium . com/analytics-vid hya/simplizing-social-media-opinion-analysis-using-Vader-in-python-f 9 e 6 EC 6 fc 52 f](/analytics-vidhya/simplifying-social-media-sentiment-analysis-using-vader-in-python-f9e6ec6fc52f)

[11][https://www . thepythoncode . com/article/Vader personance-tool-to-extract-sensitive-values-in-texts-using-python](https://www.thepythoncode.com/article/vaderSentiment-tool-to-extract-sentimental-values-in-texts-using-python)

[12]https://sentence.yourdictionary.com/angry

【13】[https://www.peakpx.com/en/hd-wallpaper-desktop-abvkw](https://www.peakpx.com/en/hd-wallpaper-desktop-abvkw)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)