# 为什么 OpenAI Whisper 的评价不完全可信

> 原文：<https://medium.com/mlearning-ai/why-the-evaluation-of-openai-whisper-is-not-entirely-credible-77f35ae6c34b?source=collection_archive---------2----------------------->

## 一半评估任务的结论是有问题的。罪魁祸首？文本规范化器。

9 月， [OpenAI 发布了 Whisper](https://openai.com/blog/whisper/) ，这是一款新的多用途语音识别模型。与以前的工作相比，它在更大的数据集上进行训练，并且特别擅长零射击任务，即，它没有被训练过的任务。

沿着该模型发布的[研究论文](https://cdn.openai.com/papers/whisper.pdf)描述、评估并…