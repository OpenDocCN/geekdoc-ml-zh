# 数据科学家的数据库？用 MongoDB 和 Pandas+Streamlit 实现简单快速的解决方案(第 2 部分)

> 原文：<https://medium.com/mlearning-ai/database-for-datascientist-easy-and-quick-solution-with-mongodb-and-pandas-streamlit-part-2-e8ec1eeb9a58?source=collection_archive---------1----------------------->

![](img/86368bdd460b4e889c95b8e36609c80e.png)

Photo by [Kelly Sikkema](https://unsplash.com/es/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **简介**

在上一篇文章中，我们已经看到了如何使用 MongoDB 创建数据库，以及如何在 Pandas 中使用它。在本文中，我们将看到如何将 MongoDB 与 Streamlit 一起使用，并作为 web 应用程序进行部署。

## **什么是细流？**

[Streamlit](https://streamlit.io/) 是一个开源的 Python 库，可以轻松创建和共享漂亮的、定制的机器学习和数据科学 web 应用。

对于数据科学家和机器学习工程师来说，这是为他们的项目创建 web 应用程序的一个很好的工具。这是非常容易使用，并有很多功能。

Streamlit 的好处在于它非常易于使用。您只需几行代码就可以创建一个 web 应用程序，不需要 HTML、CSS 或 JavaScript。

另一个好处是，你可以在不同的平台上免费部署，比如 Heroku、AWS 和 Google Cloud。

![](img/98959717f608d18517a8a12dbc941283.png)

Image taken from the Streamlit website: [https://streamlit.io/](https://streamlit.io/)

**安装**

安装 Streamlit 非常简单，可以在您的专用 python 环境中使用 pip 来完成。

```
**pip** **install streamlit**
```

安装 Streamlit 后，要验证它是否安装正确，可以在终端中运行以下命令。

```
`**streamlit hello**
```

这将在您的浏览器中打开一个新的选项卡，并带有一个简单的 web 应用程序，如下图所示。

![](img/f1ac99eeb7e5b8a1289f59930f5243bc.png)

Source: [https://pythonforundergradengineers.com/streamlit-app-with-bokeh.html](https://pythonforundergradengineers.com/streamlit-app-with-bokeh.html)

## **使用 Streamlit 创建一个基本的 web 应用程序，并将其插入 MongoDB**

在下一节中，我们将使用 Streamlit 创建一个基本的 web 应用程序，该应用程序从 MongoDB 中检索数据并将其显示在一个表中。

该应用程序非常基本，它只是为了展示如何使用 MongoDB 的 Streamlit，所以不要期望有很多功能，它只会显示用 st.dataframe() :-)构建的表中的数据。

**如果你对 Streamlit 更高级的特性感兴趣，请在下面写评论，我会再写一篇关于它的文章:-)。**

## **数据集:西雅图 Airbnb 房源**

我在这个例子中使用的数据集是西雅图 Airbnb 房源数据集。它**包含了西雅图 Airbnb 的房源信息。**数据集在 [Kaggle](https://www.kaggle.com/airbnb/seattle/data) 上可用。

我不会在这里详细解释我是如何在我的 MongoDB 数据库上加载数据集的，但是对于这个练习，你可以使用 **MongoDB Compass GUI** 上传 csv 文件，你可以在这里找到文档[。](https://docs.mongodb.com/compass/master/import-export/)

这很容易做到。您也可以使用 pymongo Python API for MongoDB 以编程方式实现这一点。

请看下面的截图，使用 MongoDB Compass 将 csv 文件添加到您的数据库是多么容易。

1.  **将数据集添加到 MongoDB Compass**

![](img/79cca56c94b85f9d2a2e0f977da267b4.png)

MongoDB Compass Console Import FIle

2.**导入 CSV 和 JSON 文件到 MongoDB Compass**

![](img/bd10abeca07773851b5fa55a696efe91.png)

MongoDB Compass Select File Window (CSV or JSON)

3.**数据集可视化为 MongoDB Compass 中的集合**

![](img/840b6036421095592486877931a53a48.png)

Seattle Dataset as a Mongo Collection on MongoDB Compass

## 代码:让我们构建一个简单的 web 应用程序

现在我们在 MongoDB 中有了数据集，我们可以开始构建我们的 web 应用程序了。代码非常简单，它只是展示了如何使用 MongoDB 的 Streamlit。

**这是一个 python 文件**，所以我们需要用。py 扩展名。我把它叫做 app_st.py(不是很有创意，我知道:-))。

1.  **导入库**

```
**import pandas as pd****import numpy as np****import pymongo****from pymongo import MongoClient****import streamlit as st**
```

2.**连接到 MongoDB**

导入库之后，我们需要连接到我们的 MongoDB 数据库。我们可以使用 pymongo 库中的 MongoClient()函数来实现。我们需要指定 MongoDB 数据库的主机和端口。

在我的例子中，我使用默认的主机和端口，所以我不需要指定它们。我屏蔽了我的用户名和密码，当然:-)。

```
#connect to mongodb**client = MongoClient('mongodb+srv://User:mypsw@cluster0.ragcpnf.mongodb.net/?retryWrites=true&w=majority')**
```

3.**获取数据库和集合**

连接到 MongoDB 后，我们需要指定数据库和我们想要使用的集合。在我的例子中，我使用数据库“Marcello_test”(我的名字……)和集合“Seattle_listing”。

```
#  Get an existing database named "Marcello_test"**db1 = client.Marcello_test**#   Get a collection named "Seattle_listing"**collection_main = db1.Seattle_listing**
```

4.**解析从 MongoDB 到 Pandas 的文档**

现在我们有了集合，我们可以解析熊猫的文档。我们可以使用 pymongo 库中的 **find()** 函数来实现。我们还可以指定要从集合中检索的字段。在我的例子中，我正在检索所有的字段。

```
# save the documents in a dataframe**df = pd.DataFrame(*list*(collection_main.find()))**# drop the _id field, not needed, it is created automatically by MongoDB**df1 = df.drop(['_id'], *axis*=1)**
```

5.**用 Streamlit 创建网络应用**

现在我们有了熊猫数据框架中的数据，我们可以用 Streamlit 创建我们的 web 应用程序。我们可以使用 **st.dataframe()** 函数来显示表格中的数据。我们还可以使用 **st.title()** 函数为我们的 web 应用程序添加一个标题。

```
# setting the screen size**st.set_page_config(*layout*="wide",*page_title*="Seattle Airbnb Data")**# main title**st.title('Seattle Airbnb Listings MongoDB')** # main text**st.subheader('This app is a Streamlit app that retrieve mongodb data and show it in a dataframe')** # subheader# write text and description**st.write('Data: Sample of Seattle listing with Name, Description, Price')** # display the dataframe in a table**st.dataframe(df1)** 
```

全部完成！！只用几行代码，我们就创建了一个 web 应用程序，它从 MongoDB 中检索数据，并将其显示在一个表格中。

6.**使用 Streamlit 在本地运行 web 应用**

现在代码已经准备好了，我们可以使用 Streamlit 在本地运行 web 应用程序。我们可以在终端中使用下面的命令。

```
**streamlit run app_st.py**
```

该代码将使用本地主机，在您的浏览器中打开一个新的选项卡，web 应用程序如下图所示。

瞧啊。我们已经创建了一个 web 应用程序，它从 MongoDB 中检索数据并将其显示在一个表中。

Simple Streamlit App deployed on localhost

# 作者:

我写关于数据科学、python 编码项目和数据驱动营销的文章。我还为数据新手或数据入门者提供数据和业务指导。

你可以在[媒体](/@Marcello_Dichiera)和[推特](https://twitter.com/ItalyMarcello)上关注我，或者访问我的[网站](https://marcello-personal-website.netlify.app/)和 [Github 页面](https://github.com/marcello-calabrese)。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)