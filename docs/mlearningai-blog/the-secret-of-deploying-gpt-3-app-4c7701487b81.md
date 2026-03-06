# 部署 GPT-3 应用程序的秘密

> 原文：<https://medium.com/mlearning-ai/the-secret-of-deploying-gpt-3-app-4c7701487b81?source=collection_archive---------3----------------------->

## 如何部署 GPT 三号应用程序？我们将在几分钟内部署最先进的 NLP 模型。

![](img/724c91da2b8ee5eae9d6a405d5216f23.png)

Photo by [Robert Gramner](https://unsplash.com/@robert_gramner?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**自由层账户**

我们将使用两个 API 来部署 GPT-3。两者都可以通过免费层获得。不需要信用卡。源代码存放在您的 Github 存储库中。仅此而已。

GPT-3 可以通过 OpenAI API 获得。它需要注册。免费等级将授予您 18 美元的免费积分。不需要信用卡。否则注册过程是不言自明的。

我们现在已经创建了帐户。登录到 API。然后单击“查看 API 密钥”部分。我们能够在这个页面中生成新的密钥。注意不要共享这些密钥。

**部署**

我们通过 Streamlit 生成应用程序。Streamlit 有空闲层。我们将使用它来部署我们的应用程序。这个应用程序的源代码存放在你的 Github 存储库中。

在[这里](https://github.com/tmgthb/gpt-3)有一个示例 Github 库。存储库包括 app.py 文件。它是用 Python 编码的。我在这里分享一个例子 app [。它会帮助你开始。](https://github.com/tmgthb/gpt-3/blob/main/gpt3.py)

Github 库也包含 Requirements.txt。在这里看一个例子[。它定义了我们的 Streamlit 应用程序所需的包。](https://github.com/tmgthb/gpt-3/blob/main/requirements.txt)

Streamlit 需要访问您的 Github 存储库。OpenAI 密钥被插入到 app [高级设置](https://blog.streamlit.io/secrets-in-sharing-apps/amp/)中。

我们现在可以部署应用程序了。

**注册你的应用**

该应用程序现已推出。恭喜你！这可能看起来太简单了，但是你有一个可以向任何人展示的应用程序。

部署 GPT 3 号伴随着一项责任。这样的大型语言模型容易被误用。有毒语言就是这样一种应用。

GPT-3 应用程序由 OpenAI 验证，以防止滥用。此处解释了[部署文档和审查流程。](https://beta.openai.com/docs/going-live)

请务必提交您的应用程序进行验证。这样，您的 API 访问就不会被撤销。

最后更新日期:2022 年 2 月 22 日

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)