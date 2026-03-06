# Various types of support vector Machines in Machine-Learning

> 原文：<https://medium.com/mlearning-ai/various-types-of-support-vector-machines-in-machine-learning-3a09ca465850?source=collection_archive---------12----------------------->

In machine learning, there are several models available to use. In this article, we are going to give a brief introduction to some type of support vector machines or in short form SVM.

# The Original SVM

The original model's purpose was to distinguish two classes of data, in other words, it was a binary classifier. Its goal was to find a line or hyperplane to have the biggest margin with each of the data in classes, So we can say it was trying to maximize the space from the line to each data for classes.

## Original SVM with support of multiple classes

The problem of the original model was that it can only classify two classes, in other words, it was a binary classifier. there are multiple ways of how to improve it to make a multiple class classifier.
One way of it is to have the original binary classifier and use it on every two classes at a time and at last, made a vote between each result of the classification process. This method is often called one-vs-one.
Another method is called one-vs-rest it tries to look at all classes as a binary classification, meaning at a time it chooses one class and considers other classes as one class. Again at the end of the process, it has a voting process to find the suitable class for each data.

# Twin support vector machine

One another type of SVM is the Twin support Vector machine or TWSVM [1]. This type of SVM is again a binary classifier that aim’s to have two hyperplanes to distinguish data. There is a little difference here that using two hyperplanes would change some facts about SVM. The difference here is we aren’t going to find the biggest margin. The goal here is to find two hyperplanes that try to fit with each data, meaning each hyperplane is going to have the closest relationship with each class (Think of a regression line for each class). At last, there is a voting process that tries to classify the test data into the closest hyperplane.

# K-SVCR

This type of support vector machine is suited for more than two classes classification. It is a mixed type of classification and regression model that uses the method called one-vs-one-vs-rest [2]. To get a better understanding of how this model works we can say, this model at a time evaluates every two classes and tries to maximize the margin between two classes. This type of SVM is closer to the original SVM than TWSVM and the computational complexity is more than the original SVM because we have added the evaluation of other classes in the time of classification.

# LSTKSVC

我们要介绍的最后一种 SVM 被命名为最小平方孪生多类支持向量机[3]。这种类型的 SVM 是我们上面介绍的两种 SVM 的混合物。为了找到最优解，它创建了两个超平面，并使用一对一对其余的方法。这种类型的 SVM 是由 Márcio Dias de Lima、Nattane Costa 和 Rommel melgao barb OSA 在文章中提出的，要了解关于这种方法的更多信息，请阅读文章 [*对最小二乘孪生多类分类支持向量机的改进*](https://www.researchgate.net/publication/326034418_Improvements_on_least_squares_twin_multi-class_classification_support_vector_machine) *。*

# 参考

[1]用于模式分类的双支持向量机([https://pubmed.ncbi.nlm.nih.gov/17469239/](https://pubmed.ncbi.nlm.nih.gov/17469239/)

[2]凯-SVCR。一种用于多类分类的支持向量机([https://www . research gate . net/publication/223547199 _ K-SVCR _ A _ support _ vector _ machine _ for _ multi-class _ classification](https://www.researchgate.net/publication/223547199_K-SVCR_A_support_vector_machine_for_multi-class_classification))

[3]对最小二乘孪生多类分类支持向量机的改进([https://www . research gate . net/publication/326034418 _ Improvements _ on _ least _ squares _ twin _ multi class _ classification _ support _ vector _ machine](https://www.researchgate.net/publication/326034418_Improvements_on_least_squares_twin_multi-class_classification_support_vector_machine))

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)