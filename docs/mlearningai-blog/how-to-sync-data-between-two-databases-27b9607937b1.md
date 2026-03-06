# 如何在两个数据库之间同步数据

> 原文：<https://medium.com/mlearning-ai/how-to-sync-data-between-two-databases-27b9607937b1?source=collection_archive---------8----------------------->

我们如何同步两个数据库，既复制所有初始数据，然后查找新的和更新的后续记录，并只对这些更改进行增量复制？

# 自动化(免费)解决方案

[redtics](https://www.redactics.com/?utm_campaign=delta_database_syncs&utm_medium=web&utm_source=hashnode)将在你自己的基础设施上运行，开发者可以免费使用(我们正在开源我们以开发者为中心的产品)。这是一个纯 SQL 解决方案，非常轻量级。增量更新接近实时复制(并且轻量级足以处理激进的表操作并发)，并且足够快，对于许多用例来说，每隔几分钟左右运行一次。

# 自制的解决方案

如果你想自己做这件事，这里有一个很好的方法:

1.  确保您希望同步的每个表都有数字主键，就像大多数基于 SQL 的数据库一样(通常是自动递增的)。
2.  然后，考虑到新主键的大小总是在增加，为了识别新行，您可以创建 SQL 查询来搜索主键值比您的最后一个主键大的记录。
3.  对于更新，请确保您的主表有一个更新日期字段，该字段在每次更新记录时都会更新。然后，您可以使用 SQL 来搜索比您最近的更新更新的最后更新日期。如果您需要搜索比当前时间稍早的日期，这是没有问题的，因为对现有数据重新应用相同的更新通常没有坏处。
4.  如果您想向目标数据库写入新数据，您需要一种策略来防止主键冲突。我们喜欢的一种方法是创建一个类似于`source_primary_key`的新列，使用上述两种方法生成 CSV(没有标题)(一个 CSV 用于新行，一个 CSV 用于更新)，然后将这个 CSV 通过管道传输到 awk 以删除主键列，并将其值移动到 source_primary_key 列。例如，如果主键列是第一列，source_primary_key 是第五列:`cat /path/to/csv.csv | gawk -vOFS=, -F "," "{print $2,$3,$4,$5,$1}" | [your SQL client]`。我们发现[这个库](https://github.com/dbro/csvquote)对于处理包含引号的数据是必要的。
5.  对于行删除，我们通常建议软删除记录，这将被视为更新，因为将`disabled`字段的值设置为 true 也会更新该行的上次更新日期。

请让我知道，如果你有任何方法的运气！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)