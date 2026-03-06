# 机器学习和泛化误差— Veni，Vidi，VC

> 原文：<https://medium.com/mlearning-ai/machine-learning-and-generalization-error-veni-vidi-vc-791c56e67c9a?source=collection_archive---------0----------------------->

在前一篇[文章](/nerd-for-tech/machine-learning-and-generalization-error-is-learning-possible-cff8721285e0)中，我们看到了 Hoeffding 不等式，稍加修改，可以用于估计一个假设在未知数据上的推广程度。问题的关键在于，我们需要摆脱对单一假设的可行性的老式验证，转而从一组假设中找到最佳假设。问题是，我们不能盲目地将赫夫丁不等式应用于这种情况——相反，我们需要通过使用所有 M 个假设的联合界限来大大放宽界限…