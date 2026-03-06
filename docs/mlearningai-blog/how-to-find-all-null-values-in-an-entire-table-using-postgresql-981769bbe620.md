# 如何使用 PostgreSQL 查找整个表中的所有空值

> 原文：<https://medium.com/mlearning-ai/how-to-find-all-null-values-in-an-entire-table-using-postgresql-981769bbe620?source=collection_archive---------5----------------------->

## 我知道的最简单的查询

![](img/45772a5986e78eb2c43ee4a13c8fcc16.png)

Photo by [Glenn Carstens-Peters](https://unsplash.com/@glenncarstenspeters?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 问题阐述

如何使用 PostgreSQL 选择一个表中任意一列有一个或多个 Null 值的所有行？

## 解决办法

虽然可能有几种方法来做这件事，但这是我知道的最简单的方法。

```
SELECT * FROM table_name WHERE NOT (table_name IS NOT NULL);+-------------+-------+---------------+--------+
| employee_id |  name | department_id | salary |
+-------------+-------+---------------+--------+
|        A101 | James |          NULL |  60000 |
|        A120 | Chris |            D3 |   NULL |
|        A309 | Steve |          NULL |   NULL |
+-------------+-------+---------------+--------+
```

## 解释

这个简单的查询选择至少有一个 Null 值的所有行，不管它在表的哪一列。直觉是`WHERE NOT (table_name IS NOT NULL)`从表中保留了“非非空行”(因此是所有的空行)。下面是来自 PostgreSQL 文档的相关资源。有疑问请看看！

> 如果`***expression***`是行值的，那么当行表达式本身为空或者当所有行的字段都为空时`IS NULL`为真，而当行表达式本身为非空并且所有行的字段都为非空时`IS NOT NULL`为真。由于这种行为，`IS NULL`和`IS NOT NULL`并不总是返回行值表达式的逆结果；特别是，包含 null 和非 null 字段的行值表达式在两个测试中都将返回 false。— PostgreSQL 文档

## 结束语

我找到了几篇文章，解释了如何在整个表中的“特定列”中，而不是“任何列”中查找具有空值的行。当第一次查看数据库并且不知道在哪里可以找到空值时，这种技术非常有用。所以我决定今天和大家分享这个话题。我希望你学到了新的东西！

## 参考

[PostgreSQL 文档](https://www.postgresql.org/docs/13/functions.html)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)