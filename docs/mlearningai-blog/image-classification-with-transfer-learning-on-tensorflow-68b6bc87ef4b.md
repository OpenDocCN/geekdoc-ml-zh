# 基于张量流的迁移学习图像分类

> 原文：<https://medium.com/mlearning-ai/image-classification-with-transfer-learning-on-tensorflow-68b6bc87ef4b?source=collection_archive---------0----------------------->

在我之前的[迁移学习帖子](/mlearning-ai/transfer-learning-with-transformers-trainer-and-pipeline-for-nlp-8b1d2c1a8c3d)中，我们回顾了 NLP 的迁移学习。 [Huggingface](https://huggingface.co/) 让 NLP 迁移学习变得非常容易。然而，到目前为止，我还没有找到针对各种计算机视觉任务的类似框架。让我们回顾一下图像分类任务，看看是什么样的模式。本帖我们将重点关注 tensorflow。

我们将使用著名的猫狗图像分类任务(辨别图像是猫图像还是狗图像)。Tensorflow 为初学者提供了一个很好的教程(带有 colab notebook ),我们将用进一步的解释来补充它。

[](https://www.tensorflow.org/tutorials/images/transfer_learning) [## 迁移学习和微调| TensorFlow 核心

### 此外，您应该尝试微调一小部分顶层，而不是整个 MobileNet 模型。在大多数情况下…

www.tensorflow.org](https://www.tensorflow.org/tutorials/images/transfer_learning) 

通过上面的例子，你会看到两种定制预训练模型的方法:

1.  特征提取:使用前一个网络学习的表示从新样本中提取有意义的特征。您只需在预训练模型的顶部添加一个新的分类器，该分类器将从头开始训练，以便您可以重新调整之前为数据集学习的特征映射。
    你不需要(重新)训练整个模型。基本的卷积网络已经包含了通常用于分类图片的特征。然而，预训练模型的最终分类部分特定于原始分类任务，并且随后特定于模型被训练的类别集。
2.  微调:解冻冻结模型库的几个顶层，并联合训练新添加的分类器层和基础模型的最后几层。这允许我们“微调”基础模型中的高阶特征表示，以便使它们与特定任务更相关。

目前，计算机视觉的主要模型架构是卷积神经网络/CNN 架构。有关 CNN 的更多信息，请参考附录，但现在只需了解该模型被训练为自动捕捉边缘、轮廓、方向、纹理等特征，这些特征可用于上层任务。

# 图像分类迁移学习的一般步骤

1.  数据加载器
2.  预处理
3.  加载预训练模型，根据需要冻结模型层
4.  根据需要添加额外的层，以形成最终的模型
5.  编译模型，设置优化器和损失函数
6.  使用 model.fit 训练模型

# 特征抽出

在主模型训练之前，一些代码加载数据集，设置预处理。您可以直接跳到“创建基础模型”部分。

## 导入图库并下载图像

这个步骤只是导入库和下载训练图像到“训练”和“验证”文件夹

```
import matplotlib.pyplot as plt
import numpy as np
import os
import tensorflow as tf_URL = '[https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip'](https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip')
path_to_zip = tf.keras.utils.get_file('cats_and_dogs.zip', origin=_URL, extract=True)
PATH = os.path.join(os.path.dirname(path_to_zip), 'cats_and_dogs_filtered')train_dir = os.path.join(PATH, 'train')
validation_dir = os.path.join(PATH, 'validation')
```

您可以在 keras 下载文件夹下看到以下文件夹，/root/。keras/datasets/cats _ and _ dogs _ filtered

火车

—猫

—狗

确认

—猫

—狗

您可以使用 linux 工具来检查原始图像大小

```
!apt install file
!apt install -y imagemagick!file /root/.keras/datasets/cats_and_dogs_filtered/train/cats/cat.199.jpg!identify /root/.keras/datasets/cats_and_dogs_filtered/train/cats/cat.199.jpg
```

原始图像大小为 270x319。

## 加载训练和验证数据集

我们将从训练文件夹加载训练数据集，从验证文件夹加载验证数据集。注意两个[参数](https://www.tensorflow.org/api_docs/python/tf/keras/utils/image_dataset_from_directory) : shuffle，是否对数据进行洗牌。默认值:True。如果设置为 False，则按字母数字顺序对数据进行排序。image_size，从磁盘中读取图像后将图像调整到的大小。由于管道处理的成批图像必须具有相同的大小，因此必须提供这一点。

```
BATCH_SIZE = 32
IMG_SIZE = (160, 160)train_dataset = tf.keras.utils.image_dataset_from_directory(train_dir,
                                                            shuffle=True,
                                                            batch_size=BATCH_SIZE,
                                                            image_size=IMG_SIZE) validation_dataset = tf.keras.utils.image_dataset_from_directory(validation_dir,
                                                                 shuffle=True,
                                                                 batch_size=BATCH_SIZE,
                                                                 image_size=IMG_SIZE)
```

由于原始数据集不包含测试集，您将创建一个测试集。为此，使用 TF . data . experimental . cardinality 确定验证集中有多少批数据可用，然后将其中的 20%移到测试集中。

```
val_batches = tf.data.experimental.cardinality(validation_dataset)
test_dataset = validation_dataset.take(val_batches // 5)
validation_dataset = validation_dataset.skip(val_batches // 5)
```

## 数据扩充和预处理

您可以向图像添加一些数据扩充以增加数据集大小，从而防止过拟合，例如水平或垂直翻转、旋转以增加训练图像的多样性

```
data_augmentation = tf.keras.Sequential([
  # A preprocessing layer which randomly flips images during training.
  tf.keras.layers.RandomFlip('horizontal_and_vertical'),
  # A preprocessing layer which randomly rotates images during training.
  tf.keras.layers.RandomRotation(0.2),
])
```

重新缩放像素值
TF . keras . applications . mobilenetv2 模型期望像素值在[-1，1]内，但此时，图像中的像素值在[0，255]内。要重新缩放它们，请使用模型中包含的预处理方法。

```
preprocess_input = tf.keras.applications.mobilenet_v2.preprocess_input
```

## 创建基础模型

我们将使用 MobileNetV2，它在移动设备上表现很好。我们将把训练图像传递给基本模型，并由基本模型输出特征。

```
# Create the base model from the pre-trained model MobileNet V2
IMG_SHAPE = IMG_SIZE + (3,)
base_model = tf.keras.applications.MobileNetV2(input_shape=IMG_SHAPE,
                                               include_top=False,
                                               weights='imagenet')image_batch, label_batch = next(iter(train_dataset))
feature_batch = base_model(image_batch)
# 32 images, since our batch_size is 32
print(feature_batch.shape)
```

## 冻结基础模型

当使用基模型作为特征提取层时，在编译和训练模型之前冻结卷积基是很重要的。冻结(通过设置 layer.trainable = False)防止给定层中的权重在训练期间被更新。MobileNet V2 有许多层，因此将整个模型的可训练标志设置为 False 将冻结所有层。

```
base_model.trainable = False
```

## 设置模型架构

当您定义模型时，您需要告诉 Keras 如何将输入映射到输出，在我们的场景中，输入->数据扩充层->预处理(重新缩放)层-> mobile v2 net-> globaveragepool2d 层-> Dropout 层-> Dense 层。

要从要素块中生成预测，请在空间 5x5 空间位置上进行平均，使用 TF . keras . layers . globalaveragepooling2d 图层将要素转换为每个影像的单个 1280 元素矢量。这就像 GlobalAveragePooling2D 在空间维度上应用平均池，直到每个空间维度都是一个。

```
global_average_layer = tf.keras.layers.GlobalAveragePooling2D()
feature_batch_average = global_average_layer(feature_batch)
# (32, 5, 5, 1280) -> (32, 1280)
print(feature_batch_average.shape)
```

应用 tf.keras.layers.Dense 图层将这些要素转换为每个影像的单个预测。

```
prediction_layer = tf.keras.layers.Dense(1)
prediction_batch = prediction_layer(feature_batch_average)
print(prediction_batch.shape)
```

链接各层

```
inputs = tf.keras.Input(shape=(160, 160, 3))
x = data_augmentation(inputs)
x = preprocess_input(x)
x = base_model(x, training=False)
x = global_average_layer(x)
x = tf.keras.layers.Dropout(0.2)(x)
outputs = prediction_layer(x)
model = tf.keras.Model(inputs, outputs)
```

## 编译模型

指定[优化器](https://data-flair.training/blogs/compile-evaluate-predict-model-in-keras/)和[损失函数](https://data-flair.training/blogs/compile-evaluate-predict-model-in-keras/)

```
base_learning_rate = 0.0001
model.compile(optimizer=tf.keras.optimizers.Adam(learning_rate=base_learning_rate),
              loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
              metrics=['accuracy'])
```

## 训练模型

我们记录了训练的历史，所以以后我们可以继续训练

```
initial_epochs = 10
history = model.fit(train_dataset,
                    epochs=initial_epochs,
                    validation_data=validation_dataset)
```

# 微调

微调的主要思想是希望调整预训练模型中的某些权重，尤其是最后几个图层中的权重，以便将一般要素地图的权重调整为与数据集特定关联的要素。您应该尝试微调一小部分顶层，而不是整个 MobileNet 模型。在大多数卷积网络中，层越高，它就越专门化。前几层学习非常简单和通用的功能，可以推广到几乎所有类型的图像。随着越往上，要素越来越特定于模型所基于的数据集。微调的目标是调整这些专门的功能以适应新的数据集，而不是覆盖一般的学习。

## 取消冻结模型的顶层

现在我们想调整预训练模型的权重，但只是在某一层之后。以下代码首先将基本模型设置为可训练的，然后将第 100 层之前的所有层设置为不可训练的(冻结包含简单和通用功能的早期层)。

```
base_model.trainable = True# Fine-tune from this layer onwards
fine_tune_at = 100# Freeze all the layers before the `fine_tune_at` layer
for layer in base_model.layers[:fine_tune_at]:
  layer.trainable = False
```

## 设置模型架构

我们将使用相同的模型架构，如特征提取案例。

## 编译模型

由于您正在训练一个大得多的模型，并且希望重新调整预训练的权重，因此在此阶段使用较低的学习速率非常重要。否则，您的模型可能会很快过度拟合。

```
model.compile(loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
              optimizer = tf.keras.optimizers.RMSprop(learning_rate=base_learning_rate/10),
              metrics=['accuracy'])
```

## 训练模型

这里，我们从先前特征提取模型停止的地方继续训练

```
fine_tune_epochs = 10
total_epochs =  initial_epochs + fine_tune_epochshistory_fine = model.fit(train_dataset,
                         epochs=total_epochs,
                         initial_epoch=history.epoch[-1],
                         validation_data=validation_dataset)
```

# 预言；预测；预告

```
# Retrieve a batch of images from the test set
image_batch, label_batch = test_dataset.as_numpy_iterator().next()
predictions = model.predict_on_batch(image_batch).flatten()# Apply a sigmoid since our model returns logits
predictions = tf.nn.sigmoid(predictions)
predictions = tf.where(predictions < 0.5, 0, 1)print('Predictions:\n', predictions.numpy())
print('Labels:\n', label_batch)plt.figure(figsize=(10, 10))
for i in range(9):
  ax = plt.subplot(3, 3, i + 1)
  plt.imshow(image_batch[i].astype("uint8"))
  plt.title(class_names[predictions[i]])
  plt.axis("off")
```

# 附录

[](https://towardsdatascience.com/the-most-intuitive-and-easiest-guide-for-convolutional-neural-network-3607be47480) [## CNN 最直观、最简单的指南

### 揭开卷积神经网络的神秘面纱

towardsdatascience.com](https://towardsdatascience.com/the-most-intuitive-and-easiest-guide-for-convolutional-neural-network-3607be47480) [](https://www.analyticsvidhya.com/blog/2020/10/what-is-the-convolutional-neural-network-architecture/) [## 卷积神经网络架构| CNN 架构

### 从事图像识别或物体检测项目，但不具备构建架构的基础？在…

www.analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2020/10/what-is-the-convolutional-neural-network-architecture/) [](https://cs231n.github.io/convolutional-networks/) [## 用于视觉识别的 CS231n 卷积神经网络

### 目录:卷积神经网络非常类似于以前的普通神经网络…

cs231n.github.io](https://cs231n.github.io/convolutional-networks/) [](https://sagarsonwane230797.medium.com/transfer-learning-from-pre-trained-model-for-image-facial-recognition-8b0c2038d5f0) [## 用于图像(面部)识别的预训练模型的迁移学习

### 本文的目的是用迁移学习的方法快速简单地解决图像识别问题。对于…

sagarsonwane230797.medium.com](https://sagarsonwane230797.medium.com/transfer-learning-from-pre-trained-model-for-image-facial-recognition-8b0c2038d5f0) [](https://puneet166.medium.com/how-to-implement-transfer-learning-using-keras-696775a907d5) [## 如何利用 keras 实现迁移学习？

### 什么是迁移学习？为什么有用。

puneet166.medium.com](https://puneet166.medium.com/how-to-implement-transfer-learning-using-keras-696775a907d5) [](https://towardsdatascience.com/4-pre-trained-cnn-models-to-use-for-computer-vision-with-transfer-learning-885cb1b2dfc) [## 4 个预训练的 CNN 模型，用于具有迁移学习的计算机视觉

### 使用最先进的预训练神经网络模型，通过迁移学习解决计算机视觉问题

towardsdatascience.com](https://towardsdatascience.com/4-pre-trained-cnn-models-to-use-for-computer-vision-with-transfer-learning-885cb1b2dfc) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)