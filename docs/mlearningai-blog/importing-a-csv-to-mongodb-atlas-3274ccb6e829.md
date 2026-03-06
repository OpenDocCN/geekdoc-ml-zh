# 如何使用 MongoDB 数据库工具在 MongoDB 中导入 csv

> 原文：<https://medium.com/mlearning-ai/importing-a-csv-to-mongodb-atlas-3274ccb6e829?source=collection_archive---------1----------------------->

![](img/746c914f7d83c843dfd2ae08896b9c56.png)

Photo by [Rubaitul Azad](https://unsplash.com/@rubaitulazad?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

MongoDB 是开发人员中最受 NoSQL(不仅代表 SQL)数据库欢迎的工具。在 MongoDB 数据库中，数据存储为保存在集合中的文档。

在这篇文章中，我将讲述使用 MongoDB 数据库工具将本地 csv 文件导入到我们的云托管 MongoDB Atlas 数据库的过程。这在构建分析或数据驱动的应用程序时非常有用。

注意:本教程主要针对 Debian 用户。

先决条件:

*   安装了 Debian Linux
*   MongoDB Atlas 数据库集群

# 安装 MongoDB 数据库工具

要上传我们的 csv 文件，我们首先需要在我们的机器上安装 MongoDB 数据库工具。这将允许我们使用 mongoimport 工具和许多其他与 mongodb 相关的工具，例如:

```
mongodump
mongorestore
bsondump
mongoimport
mongoexport
mongostat
mongotop
mongofiles
```

要做到这一点，你需要选择你的操作系统并在这里下载软件包。如果你像我一样使用 Debian(在我的例子中是 WSL2 ),你可以直接使用以下命令之一:

# Debian 11

```
wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-debian11-x86_64-100.6.1.deb 
sudo apt install ./mongodb-database-tools-debian11-x86_64-100.6.1.deb
```

# Debian 10

```
wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-debian10-x86_64-100.6.1.deb 
sudo apt install ./mongodb-database-tools-debian10-x86_64-100.6.1.deb
```

然后应该安装所有提到的工具！

# 使用 mongoimport 命令上传 csv 文件

mongoimport 命令可以与以下语法一起使用:

`mongoimport <options> <connection-string> <file>`

其中，连接字符串是您的 mongodb 集群连接字符串，格式如下:
`mongodb+srv://<username>:<password>@<host>/<database>` 或

文件参数是你想要上传的 csv 文件的路径，比如`imdb_data.csv`

选项是您可以使用的许多其他配置，例如

*   `--headerline`:让 mongodb 使用 csv 的第一行作为字段引用
*   `--collection=<collection_name>`:定义在您的数据库中使用哪些集合，否则 mongo 将使用 csv 文件的名称创建一个集合

要获得所有选项的详细信息，您可以使用`mongoimport --help`查阅帮助

在我的例子中，我使用下面的命令来确保事情做对了:

```
mongoimport \
 --uri=mongodb+srv://USERNAME:PASSWORD@HOST \ 
--db=DB\ 
--type=csv \ 
--headerline \ 
--DATA.csv \
```

额外收获:对于 json 文件来说，这甚至更简单

```
mongoimport --uri=mongodb+srv://USERNAME:PASSWORD@HOST --db=DB data.json
```

就是这样！你可以在下面评论你的想法和问题👇

*原载于 2022 年 11 月 26 日*[*https://dev . to*](https://dev.to/duranbe/importing-a-csv-to-mongodb-atlas-on-debian-with-mongoimport-26i6)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

MongoDB 文档:[https://www.mongodb.com/docs/database-tools/mongoimport/](https://www.mongodb.com/docs/database-tools/mongoimport/)