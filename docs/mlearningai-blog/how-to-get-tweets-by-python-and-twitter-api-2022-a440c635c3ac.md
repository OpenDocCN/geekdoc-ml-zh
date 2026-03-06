# 如何通过 Python 和 Twitter API 2022 获取推文

> 原文：<https://medium.com/mlearning-ai/how-to-get-tweets-by-python-and-twitter-api-2022-a440c635c3ac?source=collection_archive---------0----------------------->

## 深入了解新的 Twitter API 2022 的分步指南

![](img/d3bf45a50bf609d9fd738cf50794ed00.png)

Photo by [Jeremy Bezanger](https://unsplash.com/@unarchive?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/twitter-api?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本教程中，我将向您展示如何快速上手新的 Twitter API v2。我们将会看到:

*   如何创建一个**权限提升**的 Twitter 开发者账号，
*   如何用`tweepy`获取推文，
*   如何将我们的数据导出到 CSV。

所以，让我们开始吧！

# 创建具有提升访问权限的开发人员帐户

为了从 Twitter API 获取数据，我们首先应该创建一个开发者帐户。让我们在 developer.twitter.com 的网站上做这件事。对于一步一步的指导，你可以在下面有用的资源部分找到一个视频。这里我只给出一个简短的说明。

使用您的 Twitter 用户名登录，并确认一些个人信息。欢迎来到开发者门户！至此，您获得了*基本*访问权限。

下一步是创建你的第一个应用程序，并为它获取密钥。我们将使用这些密钥来访问 Twitter API。一旦你给你的应用一个名字，你将得到你的 API 密钥，不记名令牌和访问令牌和秘密。最后，授予**对 app 的读写**权限。

> 把你的 API 密匙和令牌保存在一个安全和私密的地方！如果其他人设法获得密钥，它可以登录你的 Twitter 帐户！如果你丢失了钥匙，你总是可以再生一把新的；)

为了能够使用 Tweepy，我们需要更高级别的 Twitter API 访问权限:我们需要申请**提升的**访问权限。

> 此时，Twitter 会问你一些关于你的应用程序的问题:基本的个人信息，以及你将如何使用 Twitter 数据。回复越详细，越容易获得批准！有时候，Twitter 会主动联系你以获取更多信息。不要担心，只要诚实和礼貌，你会得到批准的。

一旦你的申请被接受(审批过程可能需要几天)，我们就可以开始做**酷的事情了！**

# 使用 Tweepy 获取推文

为了访问 Twitter API，我们使用 Python 库`tweepy`。我们首先安装所需的库:

```
pip install tweepy
pip install configparser
pip install pandas
```

## 将您的密钥保存在配置文件中

我们首先将 API 密钥和令牌保存在一个`config.ini` 文件中，这样我们的主 python 文件可以从那里提取信息，同时保持您的访问信息的私密性。

```
[twitter]api_key = XXXXX
api_key_secret = XXXXXaccess_token = XXXXX
access_token_secret = XXXXX
```

## 主 Python 文件

我们现在准备编写我们的主文件，我称之为`twitter_api.py`。我们首先需要阅读我们的`config.ini`,并访问 Twitter API:

Authenticate to your Twitter API

我们可以使用 API 在我们的时间线中访问公共推文:

Get tweets from timeline

一旦 tweet 被打印出来，我们可以看到数据被保存在一个包含所有 tweet 的 JSON 文件中。

# 用熊猫将数据保存到 CSV

我们现在准备以更友好的格式保存收集的推文。我们将使用熊猫，并将推文保存在 CSV 文件中。

我们首先创建数据框架，将我们感兴趣的字段放入列中。就我而言，我感兴趣的是推文的时间、作者以及推文本身。接下来，我们将我们的`data`创建为一个列表，并对收集的`public_tweets`进行迭代，以将每条 tweets 添加到列表中。

最后，一旦用`pandas`创建了数据帧，我们可以用一行代码轻松地将其保存为 CSV 文件。

搞定了。我们成功地从我们的时间线收集推文！推文很有趣，可以用于许多数据科学应用，如情感分析和自然语言处理。

> 感谢阅读，敬请期待更多内容！

# 有用的资源

对于本教程，我关注了 Twitter API 2022 上的一个最新视频:

参见 Tweepy 的官方文档:

 [## Tweepy 文档- tweepy 4.6.0 文档

### tweepy . Asynchronous . async Stream-异步流引用

docs.tweepy.org](https://docs.tweepy.org/en/stable/) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)