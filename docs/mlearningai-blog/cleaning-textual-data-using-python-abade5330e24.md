# 使用 Python 进行数据预处理

> 原文：<https://medium.com/mlearning-ai/cleaning-textual-data-using-python-abade5330e24?source=collection_archive---------4----------------------->

![](img/3ee2472b2a01108c587c004c01bc1052.png)

Photo by [Andreas Fickl](https://unsplash.com/@afafa) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

文本是非结构化数据的一种形式。根据维基百科，非结构化数据被描述为没有以预定义方式组织的*信息。我们不能给机器文本数据进行处理和分析。相反，我们将数据转换成一种格式，以便机器能够更好地理解它。我们需要应用一些技术来清理我们的文本数据，然后将其发送到机器进行进一步处理。*

*在接下来的文章中，我将分享可以轻松应用于文本数据的不同方法和技术。我们将使用 python 进行预处理。*

# ***导入库***

```
*from nltk.tokenize import word_tokenize
from nltk.corpus import stopwordsimport re
import string
import re
import nltk
from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmer
from nltk.stem import WordNetLemmatizer*
```

***1。** **去掉标点:***

*标点符号是类似**"的字符。,;:!@#$?"** 根据应用程序，如果你愿意，你可以删除它，尤其是当你在处理 Twitter 数据的时候。*

*![](img/f57cfe0b5ed341d408d8caba34dcdf68.png)*

***2。正常化:***

*重要的是规范我们单词的大小写，这样每个单词都是相同的大小写，计算机就不会把同一个单词当作两个不同的符号来处理。*

*![](img/44544c47e4c2e80e3980007c6444ae4b.png)*

***3。Unicode:***

*有时数据包含一个 unicode 字符，当我们以 ASCII 格式看到它时，它是不可读的。这些字符主要用于表情符号或非 ASCII 字符。*

*![](img/3919f9782ac4f263b67771df61996060.png)*

***4。删除提及:***

*![](img/0d2a914cf76b4143d334ebaeeb9b671b.png)*

***5。移除标签:***

*![](img/61d9068497e94b5681b5555f8bdd2a51.png)*

***6。删除网址***

*![](img/49e15cf986dc9814bf8a4c99c6706d0f.png)*

*7。移除数字*

*![](img/7f15e6a3b8b7bf49837a51f8319611c0.png)*

*8。移除停用词*

*停用词是那些经常出现的词，如 **a/is/the/about** ，它们可能提供一些信息或在某些情况下引入不必要的干扰，因此在许多情况下需要将其删除。*

*![](img/b3fc139c7f64ae133b55b1f310b2109b.png)*

*9。词干*

*词干化是将屈折词缩减为词干、词根或词根形式(通常是书面形式)的过程。*

*![](img/fb726499fcc275d0c0fcb0fe857a873b.png)*

***10。引理化***

*词元化是将一个单词的词尾变化形式组合在一起的过程，这样它们就可以作为一个单独的项目进行分析，通过单词的词元或词典形式来识别。*

*![](img/966ae5e3945db3edcace45814b10c89f.png)*

*这就是你在 Python 中预处理文本数据的方式。
*希望这篇文章能对你的分析之旅有所帮助。
感谢您的阅读！**

***我们来连线:***

*   *[领英](https://www.linkedin.com/in/uzair-adamjee-28768a102/)*
*   *[Instagram](https://www.instagram.com/uzairadamjee/)*
*   *[推特](https://twitter.com/UzairAdamjee)*

*[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)*