# 不同类型的数据格式 CSV、拼花和羽毛

> 原文：<https://medium.com/mlearning-ai/different-types-of-data-formats-csv-parquet-and-feather-b9f975e461d4?source=collection_archive---------0----------------------->

![](img/9e62c4145934c2b3e5ee3ed24250d18f.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当我们在机器学习的帮助下进行数据分析或建立预测模型时，我们会遇到各种数据格式。

在这篇博客中，我们将讨论

*   逗号分隔值（csv）文件格式
*   拼花格式
*   羽化格式

## CSV 格式:

大多数表格比赛的标准格式是 CSV。CSV 代表逗号分隔值。它用于存储用逗号分隔的值。它是存储各种表格数据集的最常见的数据类型。

但是使用 CSV 格式有一些缺点。当数据较小(< 3GB，即数据量较少)时，CSV 格式工作得非常好，但是随着内容大小的增长，CSV 文件不再是存储和操作数据的有效方式。CSV 需要更长的时间阅读。当数据很大(≥15 GBs)时，用 pandas 读取 CSV 文件会阻塞所有 ram，因此如果您想要存储大文件，这不是一个非常有效的方法。

```
# for reading the CSV files 
import pandas as pd 
df = pd.read_csv("path to csv file")# for writing to csv fiels 
# considering we have dataframe which we want to write under csv file 
df.to_csv("fle_save_location.csv", index =False) 
```

## 拼花格式(。拼花地板)

对于保存数据框，Parquet 是轻量级的。Parquet 使用高效的数据压缩和编码方案来快速存储和检索数据。带“gzip”压缩的拼花地板(用于存储):它的导出速度比。csv(如果 CSV 需要压缩，那么拼花要快得多)。导入速度大约是 CSV 的 2 倍。压缩率约为原始文件大小的 22%，与压缩的 CSV 文件大致相同。

```
# for reading parquet files
df = pd.read_parquet("parquet_file_path")# for writign to the parquet format 
df.to_parquet("file_path_tostore.parquet")
```

羽化格式(。ftr)

![](img/ffd8ee6609f2db9747b47cea96908284.png)

Photo by [Hari Singh Tanwar](https://unsplash.com/@harisinghtanwar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

就数据检索而言，Feather 格式比 parquet 格式更有效。虽然它比 parquet 格式占用更多的空间，但以这种格式存储将确保有效的数据检索。

带有“ZSTD”压缩的 feather(针对 I/O 速度):与 CSV 相比，feather exporting 的导出速度快 20 倍，导入速度快约 6 倍。存储大约是原始文件大小的 32%,这比 parquet“gzip”和 CSV 压缩差 10%,但仍然不错。

```
# for reading feather format files
df = pd.read_feather("FILE_PATH_TO_FTR_FILE")# for writing data into feather format 
df.to_feather(pingInfoFilePath)
```

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)