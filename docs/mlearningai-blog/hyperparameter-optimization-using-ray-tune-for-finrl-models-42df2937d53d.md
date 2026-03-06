# 使用光线调节的 FinRL 模型的超参数优化

> 原文：<https://medium.com/mlearning-ai/hyperparameter-optimization-using-ray-tune-for-finrl-models-42df2937d53d?source=collection_archive---------3----------------------->

在之前关于使用 [Optuna](/analytics-vidhya/hyperparameter-tuning-using-optuna-for-finrl-8a49506d2741) 和[超参数扫描](/analytics-vidhya/weights-and-biases-ify-stable-baselines-models-in-finrl-f11b67f2a6a7)从权重和偏差进行超参数优化的文章中，我们已经介绍了针对 FinRL 模型使用不同超参数优化(HPO)工具的实施细节。 [FinRL](https://github.com/AI4Finance-Foundation/FinRL) 是一个开源项目，主要致力于为金融领域的强化学习提供一个研究框架。它还有 [FinRL-Meta](https://github.com/AI4Finance-Foundation/FinRL-Meta) ，这是一个金融领域所有 RL 研究的一站式平台。因此在本教程中，我们将在 FinRL 中为 RLlib 模型的 HPO 实现[光线调整](https://docs.ray.io/en/latest/tune/index.html)。