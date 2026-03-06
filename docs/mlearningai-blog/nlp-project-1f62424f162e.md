# NLP 项目

> 原文：<https://medium.com/mlearning-ai/nlp-project-1f62424f162e?source=collection_archive---------0----------------------->

## 让我们从 GPT-3 和 GPT-尼奥这样的 NLP APIs 开始吧。

![](img/e32ac44b46589fd468db080a0c648849.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**GPT-3**

开始使用 GPT-3 就像点击这里的[注册一个邮件等待列表一样简单。](https://openai.com/blog/openai-api/)测试版计划在几周后获得批准。然后，你将能够为 OpenAI API 创建一个帐户，并在 3 个月内花费 18.00 美元的免费信用。有一个入门 [**教程**](https://beta.openai.com/docs/developer-quickstart/python-bindings) **。**

在使用 GPT-3 之前，有一些重要的事实需要了解。微软拥有 GPT-3 的独家授权。它不是开源的。[定价政策](https://beta.openai.com/pricing)基于消耗量和使用的发动机。

OpenAI API 包括名为 Ada、Babbage、Curie 和 Davinci 的引擎。[引擎](https://beta.openai.com/docs/engines)-文档允许选择正确的引擎。例如达芬奇引擎提供最高的语言能力。居里发动机速度更快，价格只有 1/10。

OpenAI API 的游乐场允许测试 GPT-3 在各种 NLP 任务下的能力。我们在这里做了一些问答配对来展示达芬奇引擎。GPT 3 号能够提供事实。

```
**Q: Which year Elvis Presley died? 
A:** Elvis Presley died in 1977
```

然而，正如下面看到的，它能够犯非常简单的错误。

```
**Q:What day is today? 
A:** Today is Monday, January 1, 2018.
```

这个缺陷可以通过编程来修复。“最先进”的模型仍然有很大的局限性。如果我们在生产环境中使用它们，我们需要理解这一点。

GPT-3 是可编程的，下面的代码来自 Jupyter 笔记本。必须首先安装库。第二步是使用密钥连接到 API。最后一步是打印输出。

```
!{sys.executable} -m pip install openaiimport os
import sys
import openaiopenai.api_key = "Insert here your Open AI API secret key"
response = openai.Completion.create(engine="davinci", prompt="I expect", max_tokens=5)print(response["choices"][0]["text"])
```

“引擎”参数让我们选择引擎。“max_tokens”定义了要生成的字数。“prompt”参数是句子输入的开始。秘密密钥可以在开放的 AI API 中获得。输出是 JSON 响应。我们可以从 JSON 中选择想要打印的数据。

GPT-3 能够进行各种类型的查询。这里使用的是通过陈述“openai”的“完成”。完成”。这将提供一个或多个预测完井。其他查询类型是:“搜索”、“分类”和“答案”。例如，“答案”将提供对问题的回答。

**GPT-尼奥**

GPT-近地天体是 GPT-3 的开源替代品。开始时需要三行代码:

```
#Install Transformers library:import sys
!{sys.executable} -m pip install git+[https://github.com/huggingface/transformers](https://github.com/huggingface/transformers)# Import the pipeline:
from transformers import pipeline#Generate text, which starts with "My day..."
generator("My day", do_sample=True, min_length=100)[{'generated_text': 'My day off, as we live in the city, was nice. I went in the city for dinner with my family. My dad and uncle got me a new kitchen appliance-- a food processor! What I love about the city is-- I can'}]
```

以上实现依赖于 HuggingFace 的 [Transformer-](https://huggingface.co/transformers/) 开源库。第二种方法是使用 HuggingFace API 及其免费或付费的[定价方案](https://huggingface.co/pricing)。第三个选择是使用[Colab&Google Cloud-bucket](https://colab.research.google.com/github/EleutherAI/GPTNeo/blob/master/GPTNeo_example_notebook.ipynb#scrollTo=M0R1owh2qvp8)实现。

GPT-近地天体是 GPT-3 的有用的开源替代物。然而，与 GPT-3 的最大发动机相比，GPT-尼奥模型的尺寸仍然[小](https://venturebeat.com/2021/05/15/gpt-3s-free-alternative-gpt-neo-is-something-to-be-excited-about/)。在选择这两种模式之前，每个人都需要仔细审查成本。通过 HuggingFace API 使用 GPT-Neo 在商业使用中每月收费。在谷歌云中使用 bucket 也是有成本的。第三种选择是通过 OpenAI API 基于实际使用付费。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) 

🔵 [**成为作家**](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)