# NLP:文本清洗和预处理综合指南

> 原文：<https://medium.com/mlearning-ai/nlp-a-comprehensive-guide-to-text-cleaning-and-preprocessing-63f364febfc5?source=collection_archive---------2----------------------->

*以正确的方式处理原始文本*

![](img/79ae2707ee16a6a61b85a5b1d1a834e9.png)

Photo by [Rey Seven](https://unsplash.com/@rey_7?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

自然语言处理，简称 NLP，是人工智能中处理语言学和人类语言的一个领域。NLP 处理计算机和人类语言之间的交互。它使计算机能够理解人类语言，并对其进行编程，以执行分类、摘要、命名实体识别等任务。

但是在计算机程序能够分析人类语言之前，需要进行大量的清理和预处理步骤。文本数据的预处理和清理对模型的性能有着巨大的影响。因此，文本清理和处理在 NLP 项目的生命周期中占有非常重要的地位。

在本文中，我们将介绍一些文本清理和处理技术，并将使用 Python 编程语言实现它们。

**移除 HTML 标签**

原始文本可能包含 HTML 标记，尤其是在使用 web 或屏幕抓取等技术提取文本的情况下。HTML 标签是噪音，对理解和分析文本没有多大价值。因此，它们应该被删除。我们将使用*beautiful soup*库来移除 HTML 标签。

```
from bs4 import BeautifulSoupdef remove_html_tags(text):
    return BeautifulSoup(text, 'html.parser').get_text()remove_html_tags( ‘<p>A part of the text <span>and here another part</span></p>’)#output
>> A part of the text and here another part
```

**案例标准化**

这是 NLP 中最常见的预处理步骤之一，将文本转换成相同的大小写，通常是小写。

```
def to_lowercase(text):
    return text.lower()print("Learning NLP is Fun....")
>> learning nlp is fun
```

但是这一步会导致一些 NLP 任务中的信息丢失。例如，在情感分析任务中，用大写字母书写的单词可以表示强烈的情感，如愤怒、兴奋等。在这种情况下，我们可能希望以不同的方式执行这一步，甚至可能避免这一步。

**标准化重音字符**

有时，人们会使用重音符号，如{\\ T10 }é,}等。表示发音时对特定字母的强调。在某些情况下，重音符号还可以澄清单词的语义，如果没有重音符号，可能会有所不同。虽然您可能很少遇到带重音符号的字符，但是将这些字符转换成标准 ASCII 字符是一个很好的做法。

```
import unicodedatadef standardize_accented_chars(text):
 return unicodedata.normalize(‘NFKD’, text).encode(‘ascii’, ‘ignore’).decode(‘utf-8’, ‘ignore’)print(standardize_accented_chars('Sómě words such as résumé, café, prótest, divorcé, coördinate, exposé, latté.'))>> Some words such as resume, cafe, pretest, divorce, coordinate, expose, latte.
```

**处理网址**

很多时候，人们使用 URL，尤其是在社交媒体上，来为上下文提供额外的信息。这些 URL 不会在样本之间进行归纳，因此是噪声。我们可以使用*正则表达式删除 URL。*

```
impor re 
def remove_url(text):
 return re.sub(r’https?:\S*’, ‘’, text)print(remove_url('using [https://www.google.com/](https://www.google.com/) as an example'))>> using as an example
```

**注意:**在一些任务中，URL 可以给数据添加额外的信息。例如，在旨在检测社交媒体评论是否是广告的文本分类任务中，评论中 URL 的存在可以提供有用的信息。在这种情况下，我们可以用一个定制的令牌来替换 URL，如***<URL>***，或者在特征向量中添加一个额外的二进制特征来对应 URL 的存在。

**扩张收缩**

缩写是单词或音节的缩写。它们是通过从单词中去掉一个或多个字母而产生的。有时，多个单词组合在一起构成一个缩略词。例如，*将我的意志承包成我的意志，不要成不要。*考虑到*我将*和*我将*不同可能会导致模型性能不佳。因此，将每个收缩转换成它的扩展形式是一个很好的实践。我们可以使用*缩写库*将缩写转换成它们的扩展形式。

```
import contractionsdef expand_contractions(text):
    expanded_words = [] 
    for word in text.split():
       expanded_words.append(contractions.fix(word)) 
    return ‘ '.join(expanded_words)print(expand_contractions("Don't is same as do not"))>> Do not is same as do not
```

**删除提及和标签**

这一步在处理社交媒体文本数据时生效，例如， *Tweets。*提及和标签不能在样本间推广，在大多数 NLP 任务中是噪声。因此，最好移除这些。

```
import redef remove_mentions_and_tags(text):
    text = re.sub(r’@\S*’, ‘’, text)
    return re.sub(r’#\S*’, ‘’, text)#testing the function on a single sample for explaination
print(remove_mentions_and_tags('Some random @abc and#def'))>> Some random and 
```

上述输出可能对人类有很大意义，但有助于提高模型的性能。

**注意:**在这一步中，我们将删除“@”和“#”后面的文本。此外，这一步应该在从文本中删除特殊字符之前执行(这是我们接下来要研究的步骤)

**删除特殊字符**

特殊字符是非字母数字字符。像%、$、&等字符是特殊的。在大多数 NLP 任务中，这些字符对文本理解没有任何价值，并且会在算法中引入噪声。我们可以使用正则表达式来删除特殊字符。

```
import re
def remove_special_characters(text):
    # define the pattern to keep
    pat = r'[^a-zA-z0-9.,!?/:;\"\'\s]' 
    return re.sub(pat, '', text)

print(remove_special_characters(“007 Not sure@ if this % was #fun! 558923 What do# you think** of it.? $500USD!”))>> '007 Not sure if this  was fun! 558923 What do you think of it.? 500USD!'
```

**删除数字**

文本中的数字不会给数据增加额外的信息，也不会给算法带来干扰。因此，从文本中删除数字是一个很好的做法。同样，我们可以使用 regex 来完成这项任务。

```
# imports
import re 
def remove_numbers(text):
    pattern = r'[^a-zA-z.,!?/:;\"\'\s]' 
    return re.sub(pattern, '', text)

print(remove_numbers(“You owe me 1000 Dollars”))>> You owe me Dollars
```

**去掉双关语**

再说一次，双关不会给 NLP 中的数据增加额外的信息，因此，我们把它们去掉了。

```
import stringdef remove_punctuation(text):
    return ''.join([c for c in text if c not in string.punctuation]) remove_punctuation('On 24th April, 2005, "Me at the zoo" became the first video ever uploaded to YouTube.')>> On 24th April 2005 Me at zoo became the first video ever uploaded to Youtube
```

**引理**

词汇化从单词的屈折形式中产生单词的词根形式。比如对于词根'*打*'，'*打* ' 会是它的屈折形式。请注意，*播放和播放*的意思几乎相同，如果我们的模型认为*播放*与*播放*相同会更好。为了实现这样的转换，我们使用了词汇化。词汇化利用词汇和词形分析来生成单词的词根形式。我们将使用 spaCy 库来执行词汇化。

```
import spacy
def lemmatize(text, nlp):
   doc = nlp(text)
   lemmatized_text = []
   for token in doc:
     lemmatized_text.append(token.lemma_)
   return “ “.join(lemmatized_text)print(lemmatize('Reading NLP blog is fun.' ,nlp ))>> Read NLP blog is fun.
```

在词汇化之前，词干化被用来将词形变化的单词还原成它们的词根形式。但是词干分析并不考虑语言的词汇，而是使用一些基于规则的方法来消除单词的屈折变化。在许多情况下，词干生成的单词不是语言词汇的一部分。因此，词汇化几乎总是比词干化更受青睐。尽管如此，我还是提供了执行词干分析的代码。

```
import nltk
def get_stem(text):
    stemmer = nltk.porter.PorterStemmer()
    return ' '.join([stemmer.stem(word) for word in text.split()])print(get_stem("Sharing and caring is a good habit"))>> Shar and car is a good habit
```

注意到*关心*变成了*车*和*共享*变成了*沙尔山。*

**删除停用词**

像*我，我，我，等等，*这样的停用词不会添加任何有助于建模的信息。保持它们会增加噪声并增加特征向量的维数，严重影响计算成本和模型精度。因此，建议删除它们。我们将使用 spacy 库删除停用词。Spacy 在停用词集中有 326 个词。在某些情况下，我们可能希望添加一些自定义的停用词，例如，这些词是我们任务的停用词，但可能不在空间的停用词集中。此外，在一些 NLP 任务中，我们可能希望从 spacy 的停用词集中删除一些词。例如，在情感任务中，我们希望在文本中保留否定词，如*‘not，nenot，not，etc’*，因此，我们会将它们从 spacy 的停用词集中删除。

```
def remove_stopwords(text,nlp):       
    filtered_sentence =[] 
    doc=nlp(text)
    for token in doc:

        if token.is_stop == False: 
          filtered_sentence.append(token.text)       return “ “.join(filtered_sentence)nlp = spacy.load("en_core_web_sm", disable=["parser", "ner"])print(remove_stopwords('I love going to school',nlp))
>> love going school
```

**结论**

我们已经讨论了一些常见和有用的文本清理和处理技术。您可能已经注意到，我已经分别实现了这些函数，有些函数可以组合起来增强代码的性能。为了更好地解释各个步骤，我将所有步骤分开保存。在项目中实现这些步骤时，可以将一些步骤组合起来。此外，在某些 NLP 任务中可能不需要其中的一些。我试图解释在什么情况下可以忽略哪些步骤。但是，包含这些步骤的决定在很大程度上取决于问题陈述。此外，可能还有其他一些库用最少的代码执行相同的步骤，但是我试图用代码解释这些步骤，让人们可以思考幕后发生了什么。理解了这些步骤，移植到那些库就不是一件棘手的事情了。谢谢！！！下一篇文章再见。

**参考文献**

1.  [https://spacy.io/](https://spacy.io/)
2.  [https://www.w3schools.com/python/python_regex.asp](https://www.w3schools.com/python/python_regex.asp)
3.  [https://pypi.org/project/contractions/](https://pypi.org/project/contractions/)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)