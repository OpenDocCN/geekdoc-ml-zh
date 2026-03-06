# 着手进行手套嵌入

> 原文：<https://medium.com/mlearning-ai/get-going-with-glove-embedding-53b69e14f97d?source=collection_archive---------3----------------------->

![](img/b60da81b509424b537897f049b1dbc30.png)

Photo by [Nick Morrison](https://unsplash.com/@nickmorrison?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你想在你的项目中加入手套嵌入吗？各种术语给你带来麻烦了吗？恭喜你。你来对地方了。

***注:*** *本文不讨论手套嵌入背后的数学。*

在本文中，我们将学习如何使用手套嵌入将任何文本数据转换成数字。我们将学习使用短文本语料库的步骤，然后我们将应用这些步骤来获得 IMDB 电影评论数据集的嵌入。我们将使用获得的嵌入在同一数据集上训练二元情感分类器。

我们开始吧！

# **简介**

有各种预先训练好的手套单词嵌入可供下载。关于不同手套嵌入的训练语料库的更多信息可以在 [*this*](https://nlp.stanford.edu/projects/glove/) 网站上找到。在本教程中，我们将使用 glovetwitter27b50d 嵌入，它有 50 个维度，并在 twitter 的 2B 推文中进行了训练。

嵌入可以作为文本文件使用，其中每一行都有一个包含单词及其矢量表示的字符串。我们将把这个文本文件的内容转换成一个字典。

```
# Read the text file
glovetwitter27b50d = "pathe_to_glovetwitter27b50d.txt"
file = open(glovetwitter27b50d)
glovetwitter27b50d = file.readlines()

# Convert the text file into a dictionary
def ConvertToEmbeddingDictionary(glovetwitter27b50d):
    embedding_dictionary = {}
    for word_embedding in tqdm(glovetwitter27b50d):
        word_embedding = word_embedding.split()
        word = word_embedding[0]
        embedding = np.array([float(i) for i in word_embedding[1:]])
        embedding_dictionary[word] = embedding
    return embedding_dictionary
embedding_dictionary = ConvertToEmbeddingDictionary(glovetwitter27b50d)

# Let's look at the embedding of the word "hello."
embedding_dictionary['hello']
```

```
Output:
array([ 0.28751  ,  0.31323  , -0.29318  ,  0.17199  , -0.69232  ,
       -0.4593   ,  1.3364   ,  0.709    ,  0.12118  ,  0.11476  ,
       -0.48505  , -0.088608 , -3.0154   , -0.54024  , -1.326    ,
        0.39477  ,  0.11755  , -0.17816  , -0.32272  ,  0.21715  ,
        0.043144 , -0.43666  , -0.55857  , -0.47601  , -0.095172 ,
        0.0031934,  0.1192   , -0.23643  ,  1.3234   , -0.45093  ,
       -0.65837  , -0.13865  ,  0.22145  , -0.35806  ,  0.20988  ,
        0.054894 , -0.080322 ,  0.48942  ,  0.19206  ,  0.4556   ,
       -1.642    , -0.83323  , -0.12974  ,  0.96514  , -0.18214  ,
        0.37733  , -0.19622  , -0.12231  , -0.10496  ,  0.45388  ])
```

我们将使用下面的样本语料库来学习如何使用手套嵌入来转换任何文本数据集。

```
sample_corpus = ['The woods are lovely, dark and deep',
                 'But I have promises to keep',   
                 'And miles to go before I sleep', 
                 'And miles to go before I sleep']
```

我们将使用 Keras 标记器从数据中提取标记。Tokenizer 为每个标记分配一个索引，我们可以使用 texts_to_sequences 函数将任何文本转换成一系列索引。

```
# This is the maximum number of tokens we wish to consider from our dataset.
# When there are more tokens, the tokens with the highest frequency are chosen.
max_number_of_words = 5

# Note: Keras tokenizer selects only top n-1 tokens if the num_words is set to n
tokenizer = Tokenizer(num_words=max_number_of_words)
tokenizer.fit_on_texts(sample_corpus)
sample_corpus_tokenized = tokenizer.texts_to_sequences(sample_corpus)
print(tokenizer.word_index)
```

```
Output:
{'and': 1, 'i': 2, 'to': 3, 'miles': 4, 'go': 5, 'before': 6, 'sleep': 7, 'the': 8, 'woods': 9, 'are': 10, 'lovely': 11, 'dark': 12, 'deep': 13, 'but': 14, 'have': 15, 'promises': 16, 'keep': 17}
```

```
print("But I have promises to keep: ", sample_corpus_tokenized[1])
```

```
Output:
But I have promises to keep:  [2, 3]
```

上面的句子被转换成两个标记的列表，因为我们已经将标记的最大数量设置为 5。选择标记“I”和“to”的索引是因为它们在前 4 个频繁出现的标记中。

既然我们已经从文本语料库中选择了一组标记，我们必须为它们开发一个嵌入矩阵。嵌入矩阵将具有等于嵌入维度的**列和等于记号数量**的**行。**

```
# Create embedding matrix
total_number_of_words = min(max_number_of_words, len(tokenizer.word_index))
embedding_matrix = np.zeros((total_number_of_words,50))
for word, i in tokenizer.word_index.items():
    if i >= total_number_of_words: break
    if word in embedding_dictionary.keys():
        embedding_vector = embedding_dictionary[word]
        embedding_matrix[i] = embedding_vector
```

在上面的例子中，嵌入矩阵的维数是 4×50，因为我们有 4 个记号，并且我们使用 50 维嵌入。

人工神经网络和最大似然算法不能处理可变长度的输入，因此我们需要将每个输入序列的嵌入转换为固定的长度。有许多方法可以做到这一点，但最简单的方法是对句子中每个标记的嵌入进行求和，并对向量进行归一化。

```
def convertToSentenceVector(sentences):
    new_sentences = []
    for sentence in sentences:
        sentence_vector = []
        for word_index in sentence:
            sentence_vector.append(embedding_matrix[word_index])
        sentence_vector = np.array(sentence_vector).sum(axis=0)
        embedding_vector / np.sqrt((embedding_vector ** 2).sum())
        new_sentences.append(sentence_vecto

sample_corpus_vectorized = convertToSentenceVector(sample_corpus_tokenized)

# Print the 50-dimensional embedding of the first sentence in our text corpus.
print(sample_corpus_vectorized[0])
```

```
Output:
[-0.43196   -0.18965   -0.028294  -0.25903   -0.4481     0.53591
  0.94627   -0.07806   -0.54519   -0.72878   -0.030083  -0.28677
 -6.464     -0.31295    0.12351   -0.2463     0.029458  -0.83529
  0.19647   -0.15722   -0.5562    -0.027029  -0.23915    0.18188
 -0.15156    0.54768    0.13767    0.21828    0.61069   -0.3679
  0.023187   0.33281   -0.18062   -0.0094163  0.31861   -0.19201
  0.35759    0.50104    0.55981    0.20561   -1.1167    -0.3063
 -0.14224    0.20285    0.10245   -0.39289   -0.26724   -0.37573
  0.16076   -0.74501  ]
```

现在，我们可以执行上面讨论的步骤来获得 IMDB 电影评论数据集的嵌入。

> **情感分类:IMDB 电影评论数据集**

```
# Read the data
df = pd.read_csv("IMDB_Dataset.csv")
X = df['review']
y = df['sentiment']
# Convert labels to numbers
le = LabelEncoder()
y = le.fit_transform(y)
# Split the data into train and test sets.
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.10, random_state=0)
```

```
#Set the maximum number of tokens to 50000
max_number_of_words = 50000

# Tokenize training set
tokenizer = Tokenizer(num_words=max_number_of_words)
tokenizer.fit_on_texts(X_train)
X_train = tokenizer.texts_to_sequences(X_train)
X_test = tokenizer.texts_to_sequences(X_test)

# Create embedding matrix
total_number_of_words = min(max_number_of_words, len(tokenizer.word_index))
embedding_matrix = np.zeros((total_number_of_words+1,50))
for word, i in tokenizer.word_index.items():
    if i >= total_number_of_words: break
    if word in embedding_dictionary.keys():
        embedding_vector = embedding_dictionary[word]
        embedding_matrix[i] = embedding_vector

# Get a fixed-size embedding for every comment using the function defined earlier
X_train = convertToSentenceVector(X_train)
X_test = convertToSentenceVector(X_test)
```

现在让我们训练一个人工神经网络。

```
# Define a sequential model
model = Sequential()
model.add(Dense(100, input_shape = (50,), activation = "relu"))
model.add(Dense(1000, activation = "relu"))
model.add(Dropout(0.2))
model.add(Dense(1, activation = "sigmoid"))
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
model.summary()

# Fit the model
model.fit(X_train, y_train, batch_size=32, epochs=10, validation_split=0.1)

# Get the classification report on the test set
y_pred = model.predict(X_test)
y_pred = y_pred.round()
print(classification_report(y_test, y_pred))
```

```
Output:
              precision    recall  f1-score   support

           0       0.82      0.77      0.79      2553
           1       0.77      0.82      0.80      2447

    accuracy                           0.80      5000
   macro avg       0.80      0.80      0.80      5000
weighted avg       0.80      0.80      0.80      5000
```

由人工神经网络产生的分类报告不是很好，因为人工神经网络不能很好地处理序列数据。为了获得更好的结果，我们现在将在同一数据集上使用双向长短期记忆(LSTM)网络。双向 LSTM 是一种递归神经网络，可以处理不同大小的输入。

```
max_number_of_words = 50000
max_length = 100

tokenizer = Tokenizer(num_words=max_number_of_words)
tokenizer.fit_on_texts(X_train)
X_train = tokenizer.texts_to_sequences(X_train)
X_test = tokenizer.texts_to_sequences(X_test)

# We can use padding to make the length of every comment equal to max_length
X_train = pad_sequences(X_train, maxlen=max_length)
X_test = pad_sequences(X_test, maxlen=max_length)
```

先前生成的嵌入矩阵将被用作嵌入层中的权重。

```
# Define a sequential model
model = Sequential()
# Add an embedding layer and pass the embedding matrix as weights
model.add(Embedding(max_number_of_words+1, 50, input_shape = (100,), weights=[embedding_matrix]))
model.add(Bidirectional(LSTM(50, return_sequences=True, dropout=0.1, recurrent_dropout=0.1)))
model.add(GlobalMaxPool1D())
model.add(Dense(50, activation="relu"))
model.add(Dropout(0.1))
model.add(Dense(1, activation="sigmoid"))
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
model.summary()

# Fit the model
model.fit(X_train, y_train, batch_size=32, epochs=10, validation_split=0.1);

# Get the classification report
y_pred = model.predict(X_test)
y_pred = y_pred.round()
print(classification_report(y_test, y_pred))
```

```
Output:
              precision    recall  f1-score   support

           0       0.87      0.83      0.85      2553
           1       0.83      0.87      0.85      2447

    accuracy                           0.85      5000
   macro avg       0.85      0.85      0.85      5000
weighted avg       0.85      0.85      0.85      5000
```

呜哇！我们得到了更好的分类报告。

现在，您对手套嵌入有了更好的理解，您已经准备好将其应用于各种 NLP 问题。

***注:*** *你可以从* [*这个*](https://www.kaggle.com/code/adwaitkesharwani/get-going-with-glove-embedding) *链接访问完整的代码。*

# **参考文献**:

1.  [https://www.tensorflow.org/text/guide/word_embeddings](https://www.tensorflow.org/text/guide/word_embeddings)
2.  [https://www . ka ggle . com/code/jhoward/improved-lstm-baseline-glove-dropout](https://www.kaggle.com/code/jhoward/improved-lstm-baseline-glove-dropout)
3.  [https://www . ka ggle . com/code/abhishek/approximating-almost-any-NLP-problem-on-ka ggle](https://www.kaggle.com/code/abhishek/approaching-almost-any-nlp-problem-on-kaggle)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)