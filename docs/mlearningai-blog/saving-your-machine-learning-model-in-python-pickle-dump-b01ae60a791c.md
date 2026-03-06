# 在 Python 中保存机器学习模型:pickle.dump()

> 原文：<https://medium.com/mlearning-ai/saving-your-machine-learning-model-in-python-pickle-dump-b01ae60a791c?source=collection_archive---------0----------------------->

## 本文通过 pickle 库向您介绍 python 中对象序列化和反序列化的概念。当您阅读完本文时，您将能够将您的模型无缝地序列化/保存为 pickle 文件，并在同一个或另一个 python 文件中反序列化/打开它以备后用，解决您在使用 pickle 时可能遇到的问题。

![](img/8430c09563b88a38ac54c2fd5496e055.png)

# 内容

*   泡菜基础介绍。
*   和泡菜一起工作。
*   常见错误及其解决方法。
*   结论。

## 泡菜基础介绍。

Pickle 是用于对象序列化/反序列化的 python 标准库之一。Pickle 序列化以字节格式将对象保存到文件中，而反序列化是序列化的逆过程。当试图将一个对象序列化到 pickle 文件中时，有必要首先声明**字节/二进制**格式，否则会遇到错误。列表、元组、字典、转换器、模型和许多其他对象都可以被 pickle/serialize，但是，本文主要关注机器学习模型的序列化和反序列化。

## 使用 Pickle:模型序列化

要使用 pickle 库，您必须首先导入该库，代码行为"**导入 pickle "，y** 如果您愿意，也可以使用别名，如下所示"**导入 pickle 作为 pk"** :

```
# Import style 1 (Without Alias)
import pickle

# Import style 2 (With Alias)
import pickle as pk
```

在使用您选择的样式导入 pickle 库之后，就该打开/创建一个文件并将对象转储到该文件中了。打开文件时，需要指定**文本模式**，分别是 ***【写入】******(w)******二进制模式******(b)****进行序列化操作。我将讨论两种模型序列化的方法，它们是:*

> ***方法一***

*要使用“长方法”保存文件，请执行以下操作:*

```
*# Saving model to pickle file
with open("desired-model-file-name.pkl", "wb") as file: # file is a variable for storing the newly created file, it can be anything.
    pickle.dump(model, file) # Dump function is used to write the object into the created file in byte format.*
```

*第一行以二进制写模式打开一个文件，并将新创建的文件存储在“文件”变量中，第二行将模型对象写入文件，并默认保存在与存储 python 文件的文件相同的目录/文件夹中，但您可以在 **open** 函数中指定您自己的文件路径以及 desired-model-file-name.pkl。*

> *方法 2*

*要使用速记方法保存文件，请执行以下操作:*

```
*pickle.dump(model, open("desired-model-file-name.pkl", "wb"))*
```

*这段令人惊叹的一行代码完成了前面方法所做的一切，但只用了一行代码。作为初学者，我建议您坚持使用第一种方法，直到您对文件创建有了更好的理解，然后再使用这种一行程序方法。第一种方法的优势在于，带有函数的**在保存模型后自动为您关闭文件，而对于一行程序方法，您必须利用 **close()** 函数来关闭文件。***

## *使用 Pickle:模型反序列化*

*反序列化，序列化过程的逆过程也一样简单。为了反序列化一个文件，还需要指定**文本模式**，分别是***read******(r)***和 ***二进制模式******(b)***进行反序列化操作 ***。***read 模式告诉编译器文件已经存在，需要做的就是读入二进制文本并将其转换回原始对象。为此，我还将讨论两种方法。*

> *方法 1*

```
*# Opening saved model
with open("desired-model-file-name.pkl", "rb") as file:
    model = pickle.load(file)

# The model has now been deserialized, next is to make use of it as you normally would.
prediction = model.predict([[2,112,68,22,94,34.1,0.315,26]]) # Passing in variables for prediction
print("The result is",prediction[0]) # Printing result*
```

*第一行代码指定要以字节格式打开的文件，然后将该文件存储在“file”变量中，第二行代码加载作为模型的文件的内容，并将其存储在“model”变量中，然后可以正常使用。*

> *方法 2*

```
*# Open saved model using 2nd method
model_mtd2 = pickle.load(open("desired-model-file-name.pkl", "rb"))

# Now, to make use of the model as you normally would
result = model_mtd2.predict([[3,193,70,31,0,34.9,0.241,25]])
print("The result is", result[0])*
```

*和以前一样，这个惊人的一行代码只用一行代码就完成了第一个方法所做的事情，之后这个模型就可以照常使用了。*

# *常见错误及其解决方法*

> *常见错误 1:试图在不指定字节/二进制格式的情况下进行序列化。*

```
*# Attempting to serialize without specifying byte/binary format.
with open("desired-model-file-name.pkl", "w") as file:
    pickle.dump(model, file)

>>> Output Below
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
c:\Users\samsung\Documents\Projects\Article specific\model-serialization-deserialization\model-serialization-deserialization.ipynb Cell 10 in <cell line: 2>()
      1 # Attempting to serialize without specifying byte/binary format.
      2 with open("desired-model-file-name.pkl", "w") as file:
----> 3     pickle.dump(model, file)

TypeError: write() argument must be str, not bytes*
```

*当试图序列化一个对象时，如果忘记指定二进制文本模式(b)，编译器将抛出上面的错误。简单的解决方法是在**‘w’**打开文本模式之前或之后包含**‘b’**。*

> *常见错误 2:试图用读取文本模式进行序列化*

```
*# Attempting to serialize with read text mode
with open("desired-model-file-name.pkl", "rb") as file:
    pickle.dump(model, file)

>>> Output Below
---------------------------------------------------------------------------
UnsupportedOperation                      Traceback (most recent call last)
c:\Users\samsung\Documents\Projects\Article specific\model-serialization-deserialization\model-serialization-deserialization.ipynb Cell 11 in <cell line: 2>()
      1 # Attempting to serialize with read text mode
      2 with open("desired-model-file-name.pkl", "rb") as file:
----> 3     pickle.dump(model, file)

UnsupportedOperation: write*
```

*使用读文本模式告诉你的编译器你正在试图获取信息而不是把信息放入文件，因此当出现这样的错误时，编译器简单地建议你使用写文本模式**‘w’**，你可以把它放在**‘b’**文本模式之前或之后。*

> *常见错误 3:试图在没有写/写文本模式的情况下进行序列化/反序列化*

```
*# Attempting to serialize without specifying write text mode
with open("desired-model-file-name.pkl", "b") as file:
    pickle.dump(model, file)

>>> Output Below
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
c:\Users\samsung\Documents\Projects\Article specific\model-serialization-deserialization\model-serialization-deserialization.ipynb Cell 12 in <cell line: 2>()
      1 # Attempting to serialize without specifying write text mode
----> 2 with open("desired-model-file-name.pkl", "b") as file:
      3     pickle.dump(model, file)

ValueError: Must have exactly one of create/read/write/append mode and at most one plus*
```

```
*# Attempting to deserialize without specifying read text mode
with open("desired-model-file-name.pkl", "b") as file:
    pickle.load(file)

>>> Output Below
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
c:\Users\samsung\Documents\Projects\Article specific\model-serialization-deserialization\model-serialization-deserialization.ipynb Cell 13 in <cell line: 2>()
      1 # Attempting to deserialize without specifying read text mode
----> 2 with open("desired-model-file-name.pkl", "b") as file:
      3     pickle.load(file)

ValueError: Must have exactly one of create/read/write/append mode and at most one plus*
```

*试图在不指定访问模式的情况下访问文件将引发值错误。不指定读模式就不能读，不指定写模式就不能写。要解决此问题，只需在**‘b’**文本模式之前或之后为您想要执行的功能指定适当的模式。*

> *常见错误 4:试图反序列化无效文件*

```
*# Attempting to deserialize invalid file
with open("desired-model-file-name.pkll", "rb") as file:
    pickle.load(file)

>>> Output Below
---------------------------------------------------------------------------
FileNotFoundError                         Traceback (most recent call last)
c:\Users\samsung\Documents\Projects\Article specific\model-serialization-deserialization\model-serialization-deserialization.ipynb Cell 14 in <cell line: 2>()
      1 # Attempting to deserialize invalid file
----> 2 with open("desired-model-file-name.pkll", "rb") as file:
      3     pickle.load(file)

FileNotFoundError: [Errno 2] No such file or directory: 'desired-model-file-name.pkll'*
```

*在键入文件名时很容易忘记字母或符号，如果您得到“没有这样的文件”错误信息，这仅仅意味着您犯了一个印刷错误。要解决这个问题，您必须仔细重写文件路径/名称，或者更好的是，复制并粘贴准确的路径/名称，以避免任何错误。*

# *结论*

*Pickle 是一个非常强大的 python 库，允许用户存储和以后打开对象。它为您提供了存储模型的多个版本的能力，最重要的是，它使得将您的模型集成到诸如 web 应用程序之类的应用程序中变得容易。可以很容易地将保存的模型反序列化到服务器端文件中，通过前端的表单或任何其他方法收集模型参数，如果需要，在任何特征工程之后以数组的形式传递到模型中，并且将无缝地提供结果。点击进入 GitHub repo [。](https://github.com/munas-git/Pickle-article/tree/main)*

*通过阅读本文，您现在知道了如何使用 pickle 库在 python 中存储和打开您的模型和各种其他对象。*

*你也可以在这里阅读我关于使用 **Flask，HTML & CSS** [创建机器学习模型集成的全栈 web 应用的文章。](https://blog.devgenius.io/building-a-simple-web-app-for-your-model-flask-html-css-dd6cbd74d1ed)*

*就这些了，下次再见…*

*[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)*