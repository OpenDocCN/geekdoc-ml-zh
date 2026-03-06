# 如何轮询气流作业(即 DAG 运行)

> 原文：<https://medium.com/mlearning-ai/how-to-poll-an-airflow-job-i-e-dag-run-14cdb9fa1967?source=collection_archive---------4----------------------->

曾经想知道气流 DAG 运行何时完成？也许您的用例涉及到这个完成的工作是某种工作流依赖，或者也许它被用在 CI/CD 管道中。我敢肯定，除了我们在[redic ics](https://www.redactics.com/)的场景之外，这里还有无数种可能的场景，即使用 Airflow 来克隆数据库，以进行数据库迁移的预演，但您可以做出判断！

[这里是回购](https://github.com/Redactics/airflow-dagrun-poller)

在这个 repo 中，您可以找到一个样例`Dockerfile`，一个样例 Kubernetes 作业，它提供了关于如何使用这个轮询器脚本以及脚本本身的一些上下文。这没什么大不了的，但我希望它能节省一些开发人员的时间，如果他们需要重新创建这个功能的话！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)