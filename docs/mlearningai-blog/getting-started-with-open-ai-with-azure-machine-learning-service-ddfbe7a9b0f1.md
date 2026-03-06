# 使用 Azure 机器学习服务开始使用 Open AI

> 原文：<https://medium.com/mlearning-ai/getting-started-with-open-ai-with-azure-machine-learning-service-ddfbe7a9b0f1?source=collection_archive---------13----------------------->

# 如何使用 Python 和 Azure 机器学习服务开始使用 Open AI Api

# 先决条件

*   Azure 帐户
*   Azure 存储
*   Azure 机器学习服务
*   Azure Open AI Api 帐户

# 创建开放 API 试用帐户

*   前往[https://beta.openai.com/api/signup](https://beta.openai.com/api/signup)
*   创建一个试用帐户
*   查看文档
*   对于本文，我们将使用 python 客户端
*   在你的设置上抓住钥匙

# Azure 机器学习服务

*   登录 Azure 门户网站
*   转到 Azure 机器学习服务
*   单击工作区启动
*   或者去 ml.azure.com
*   创建或启动您的计算实例
*   使用 python 3.8 和 Aml SDK 创建新笔记本
*   安装开放 ai

```
pip install openai
```

*   重启内核
*   现在写代码
*   首先初始化密钥

```
OPENAI_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

*   现在调用客户端

```
import os
import openai# Load your API key from an environment variable or secret management service
openai.api_key = OPENAI_API_KEYresponse = openai.Completion.create(engine="davinci", prompt="How is the stock market doing today?", max_tokens=50)
```

*   以下是回应

```
<OpenAIObject text_completion id=cmpl-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx at 0x7fbbea0396d0> JSON: {
  "choices": [
    {
      "finish_reason": "length",
      "index": 0,
      "logprobs": null,
      "text": "\u201d \u201cWhat\u2019s the price of gas today?\u201d and \u201cWhat would house prices be?\u201d Okay, now apply these to blockchain research. \u201cIs Bitcoin\u2019s value going up?\u201d;"
    }
  ],
  "created": 1639665062,
  "id": "cmpl-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "model": "davinci:2020-05-03",
  "object": "text_completion"
}
```

*最初发表于*[T5【https://github.com】](https://github.com/balakreshnan/Samples2022/blob/main/openai/getstarted.md)*。*

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)