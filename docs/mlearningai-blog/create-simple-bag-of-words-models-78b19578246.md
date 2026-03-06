# 创建简单的单词袋模型

> 原文：<https://medium.com/mlearning-ai/create-simple-bag-of-words-models-78b19578246?source=collection_archive---------5----------------------->

在本系列的前一篇博客中，我们了解了什么是单词袋模型，以及我们如何手动创建一个简单的模型。

如果你没有读过这篇博客，请在这里阅读:

![](img/8074ef9cef446974407011307b8bb776.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇博客中，我们将学习如何使用 Scikit-Learn 和 Keras 创建简单的弓模型。

# 使用 Scikit-Learn 创建弓

有不同类型的评分方法可用于将文本数据转换为数字向量。你可以在这里阅读这些技术[。](/@priyansh-kedia/methods-for-scoring-words-in-nlp-8a1f0b55605a)

Scikit-Learn 提供了将文本数据转换成数值向量的不同方法。其中两种方法是:

*   **计数矢量器**
*   **tfidf 矢量器**

## 计数矢量器

> 将文本文档集合转换为令牌计数矩阵

```
from sklearn.feature_extraction.text import CountVectorizer# input text data
text = ["When your only tool is a hammer, all problems start looking like nails."]# create the instance of vectorizer
vectorizer = CountVectorizer()# fit is used to learn the vocabulary
vectorizer.fit(text)# print the generated vocabulary
print(vectorizer.vocabulary_)# vectorize the input text
vector = vectorizer.transform(text)print(vector.shape)
print(vector.toarray())
```

在上面的代码片段中，我们简单地向矢量器`fit`方法提供了一个样本文本数据，它从输入文本中学习词汇。

接下来，我们使用`transform`方法将文本转换成数字向量。

## tfidf 矢量器

> 将原始文档集合转换为 TF-IDF 特征矩阵。

```
from sklearn.feature_extraction.text import TfidfVectorizer# input text data
text = ["When your only tool is a hammer, all problems start looking like nails.",
  "When your",
  "problems start looking"]# create the instance of vectorizer
vectorizer = TfidfVectorizer()# fit is used to learn the vocabulary
vectorizer.fit(text)# print the generated vocabulary and idf values
print(vectorizer.vocabulary_)
print(vectorizer.idf_)# vectorize an input text
vector = vectorizer.transform([text[0]])print(vector.shape)
print(vector.toarray())
```

在上面的代码片段中，我们简单地向矢量器`fit`方法提供了一个样本文本数据，它从输入文本中学习词汇。

接下来，我们使用`transform`方法将文本转换成数字向量。

# 使用 Keras 创建弓

我们可以使用 **keras 预处理**模块的**标记器**类。

> 这个类允许对文本语料库进行矢量化，方法是将每个文本转换为整数序列(每个整数是字典中一个标记的索引)或向量，其中每个标记的系数可以是二进制的，基于字数，基于 tf-idf…

Tokenizer 类使用起来非常方便，因为它提供了对不同类型的现成评分方法的支持。

```
from keras.preprocessing.text import Tokenizer# define 4 documents
docs = ["No man is an island", "Entire of itself,", "Every man is a piece of the continent,", "A part of the main."]# create the tokenizer
t = Tokenizer()# fit the tokenizer on the documents
t.fit_on_texts(docs)# summarize what was learned
print(t.word_counts)
print(t.document_count)
print(t.word_index)
print(t.word_docs)# integer encode documents
encoded_docs = t.texts_to_matrix(docs, mode='count')
print(encoded_docs)
```

在上面的代码片段中，我们首先在所有文档上安装标记器来学习词汇。这是使用`fit_on_texts`完成的。然后我们用`texts_to_matrix`把文字转换成相应的矢量。

需要注意的重要一点是该函数的第二个参数`mode`。

我们可以使用`mode`来指定我们希望在对文本进行矢量化时使用的评分方法的类型。**该值可以是“二进制”、“计数”、“tfidf”、“频率”中的一个。**

```
#       if mode == 'count':#             x[i][j] = c#       elif mode == 'freq':#             x[i][j] = c / len(seq)#       elif mode == 'binary':#             x[i][j] = 1#       elif mode == 'tfidf':#             tf = 1 + np.log(c)#       idf = np.log(1 + self.document_count /#             (1 + self.index_docs.get(j, 0)))#             x[i][j] = tf * idf
```

上述公式可在`texts_to_matrix`方法的文档说明中找到。

在这篇博文中，我们了解了使用 Scikit-Learn 和 keras 创建单词袋模型的不同方法。

**参考文献**

[](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) [## sk learn . feature _ extraction . text . count vectorizer-sci kit-learn 0 . 24 . 2 文档

### class sk learn . feature _ extraction . text . count vectorizer(*，input='content '，encoding='utf-8 '，decode_error='strict'…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) [](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) [## sk learn . feature _ extraction . text . tfidf vectorizer-sci kit-learn 0 . 24 . 2 文档

### class sk learn . feature _ extraction . text . tfidf vectorizer(*，input='content '，encoding='utf-8 '，decode_error='strict'…

scikit-learn.org](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) [](https://keras.io/api/preprocessing/text/#tokenizer-class) [## Keras 文档:文本数据预处理

### 从目录中的文本文件生成 tf.data.Dataset。如果您的目录结构是:然后调用…

keras.io](https://keras.io/api/preprocessing/text/#tokenizer-class)