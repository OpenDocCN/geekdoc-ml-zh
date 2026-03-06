# 第五天寻找最佳媒体标题

> 原文：<https://medium.com/mlearning-ai/day-5-searching-for-the-best-medium-titles-with-python3-5c14e31285e?source=collection_archive---------5----------------------->

## 使用交互措施作为 50 天的指标

欢迎来到第五天。

![](img/121516944bd9da678e7d60c11754b1c5.png)

Photo by [Alexandru Zdrobău](https://unsplash.com/@alexandruz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

昨天，我们发现下面的代码使得 df_analysis 可访问，并与我们数据中的正确位置相互连接。

```
flat_list_title **=** [v[‘article_title_stats’] **for** k,v **in** df_analysis.items()] *# v is still a dict of the values*…
```