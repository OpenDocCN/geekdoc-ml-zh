# TFGNN 快速入门

> 原文：<https://medium.com/mlearning-ai/a-very-quick-start-to-tfgnn-fe10f222c36d?source=collection_archive---------0----------------------->

[TFGNN](https://github.com/tensorflow/gnn) 是基于 TensorFlow 的 GNN 库，它同时实现了 MessagePassing 和 GraphNets 框架，这意味着你可以在框架中轻松设置上下文(全局)值。

开始前的一些有用链接:[图形神经网络的温和介绍【TFGNN 入门者的最佳入门博客)，TFGNN 发表的](https://distill.pub/2021/gnn-intro/)[论文](https://arxiv.org/abs/2207.03522) (CoRR 2022)。

![](img/4ed9c9e3e269ea5db4b1aaacc6272d7f.png)

## **基本数据结构:GraphTensor**

构建 GNN 训练管道的第一步是构建数据集。TFGNN 的基本数据结构是 tfgnn。 **GraphTensor** ，带**节点集/边集/上下文**，每个集都有自己的**特征。**

详细介绍文档:[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/graph _ tensor . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/graph_tensor.md)

## 输入施工管线

在建模之前，我们首先需要逐步构建我们的训练集。[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/input _ pipeline . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/input_pipeline.md)

**第一步:描述图模式:**[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/schema . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/schema.md)

在这一步，我们定义了**节点集/边集特征**和**上下文特征**；以下是一些注意事项:

*   在 tfgnn 文档中提到‘标量特征不需要维度标识。’但是实践表明，如果要在建模中直接处理那些标量特征，可以将它们的形状设置为“{ dim { size: 1} }”，并将数据类型定义为 FLOAT。
*   TFGNN 模型可以在“定向”边的两个方向上传播消息，默认情况下，TFGNN 在消息传递阶段采用定向边。因此在数据准备步骤中不需要指定“无方向”的边。
*   有各种各样的图任务类型:单图分类/回归(给出一堆小图进行分类)，单节点分类/回归(从大图中抽取一个子图)等。

**第二步:数据样本准备&采样:**[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/Data _ prep . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/data_prep.md)

python 内编码的步骤:

*   使用 GraphTensor.from_pieces()函数为 GraphTensor 类型构造一个 eager 实例。
*   使用 tfgnn.write_example(图形)和 writer.write(示例。SerializeToString())来序列化内存实例，并将其写入磁盘上的文件中。

**第三步:将文件读入数据集:**[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/input _ pipeline . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/input_pipeline.md)

要读取一个 GraphTensor(复合张量)，我们需要根据定义的 Spec:tfg nn . parse _ single _ example/parse _ example()解析*serializable*；

## 建模管道**(使用 Keras API):**

通常，我们为预处理和实际建模构建 2 个 Keras 模型:1 个预处理模型+ 1 个主训练模型；请注意，这两个模型都是在标量采样模式下定义的…

管道摘要:读取数据集->解析为张量->构建预处理模型->构建训练模型->拟合训练模型->导出用于服务

**预处理模型定义:**[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/input _ pipeline . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/input_pipeline.md)

预处理模型的动机是将原始特征从离散特征处理成输入向量，例如将 INT 特征映射成 8 长度向量；或者*将*几个功能串联在一起。预处理函数的返回值应该是节点/边的起始隐藏状态。

步骤:

*   对于预处理函数，最好使用 tf.keras.layers 的内置函数。
*   TFGNN 将在一个具有独立组件的大图(没有连接的子图)中合并一批图样本，然后应用更新功能，将该批作为标量样本。这样做的原因是为了使基于批处理的计算可行，因为每个样本可能具有不同的大小，这与传统的表格数据集显著不同(用图张量'*size*' =[*total _ size*，]，其中*total _ size*=*batch _ size**(*set _ size*)**channel _ size*)。

**主模型定义:**

[https://github . com/tensor flow/gnn/blob/main/tensor flow _ gnn/docs/guide/gnn _ modeling . MD](https://github.com/tensorflow/gnn/blob/main/tensorflow_gnn/docs/guide/gnn_modeling.md)

在 TFGNN 中有三种方法可以构建 GNN 图层:

1.  使用本机支持的层的 XXXGraphUpdate 函数，如 GATv2、GCN 等。

2.从 TensorFlow-Keras' scratch 自定义 *tf.keras.layers.Layer* 。

3.利用*tfgnn . keras . layers . xxxgraphupdate**在 tfg nn 的帮助下建立图层。*

## *祝你好运！*

*[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)*