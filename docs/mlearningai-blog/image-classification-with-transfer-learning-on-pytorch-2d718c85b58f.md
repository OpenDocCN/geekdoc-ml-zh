# PyTorch 上基于迁移学习的图像分类

> 原文：<https://medium.com/mlearning-ai/image-classification-with-transfer-learning-on-pytorch-2d718c85b58f?source=collection_archive---------2----------------------->

在[之前的文章](/mlearning-ai/image-classification-with-transfer-learning-on-tensorflow-68b6bc87ef4b)中，我们回顾了使用 Tensorflow/Keras 进行迁移学习的图像分类。PyTorch 是另一个流行的深度学习框架，同样的模式在这里也适用。

同样，图像分类迁移学习的一般步骤是:

1.  数据加载器
2.  预处理
3.  加载预训练模型，根据需要冻结模型层
4.  根据需要添加额外的层，以形成最终的模型
5.  编译模型，设置优化器和损失函数
6.  使用 model.fit 训练模型

让我们回顾一下 PyTorch 提供的示例教程

 [## 计算机视觉迁移学习教程- PyTorch 教程 1.11.0+cu102 文档

### 作者:Sasank Chilamkurthy 在本教程中，你将学习如何训练一个卷积神经网络的图像…

pytorch.org](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html) 

## 数据加载器和预处理

该示例用于训练模型对蚂蚁和蜜蜂进行分类。它假设你将[数据集](https://download.pytorch.org/tutorial/hymenoptera_data.zip)下载到“数据/膜翅目 _ 数据”文件夹。该示例还包括数据加载期间的预处理(使用转换功能，例如调整大小、翻转等。)

```
from __future__ import print_function, divisionimport torch
import torch.nn as nn
import torch.optim as optim
from torch.optim import lr_scheduler
import torch.backends.cudnn as cudnn
import numpy as np
import torchvision
from torchvision import datasets, models, transforms
import matplotlib.pyplot as plt
import time
import os
import copy# Data augmentation and normalization for training
# Just normalization for validation
data_transforms = {
    'train': transforms.Compose([
        transforms.RandomResizedCrop(224),
        transforms.RandomHorizontalFlip(),
        transforms.ToTensor(),
        transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
    ]),
    'val': transforms.Compose([
        transforms.Resize(256),
        transforms.CenterCrop(224),
        transforms.ToTensor(),
        transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225])
    ]),
}data_dir = 'data/hymenoptera_data'
image_datasets = {x: datasets.ImageFolder(os.path.join(data_dir, x),
                                          data_transforms[x])
                  for x in ['train', 'val']}
dataloaders = {x: torch.utils.data.DataLoader(image_datasets[x], batch_size=4,
                                             shuffle=True, num_workers=4)
              for x in ['train', 'val']}
dataset_sizes = {x: len(image_datasets[x]) for x in ['train', 'val']}
class_names = image_datasets['train'].classes
```

## 负载预训练模型

[RESNET18](https://arxiv.org/abs/1512.03385) 是微软研究院在 2015 年发布的一款热门机型。

```
model_ft = models.resnet18(pretrained=True)
```

## 添加附加层

它代替最后的[全连接层](https://towardsdatascience.com/convolutional-layers-vs-fully-connected-layers-364f05ab460b)进行二进制分类。

```
num_ftrs = model_ft.fc.in_features
# Here the size of each output sample is set to 2.
# Alternatively, it can be generalized to nn.Linear(num_ftrs, len(class_names)).
model_ft.fc = nn.Linear(num_ftrs, 2)
```

## 设置优化器，损失函数

[分类任务常用交叉熵损失](/swlh/cross-entropy-loss-in-pytorch-c010faf97bab)，使用[随机梯度下降/SGD](https://www.projectpro.io/recipes/optimize-function-sgd-pytorch) 优化器。学习率在机器学习训练中非常重要。想想下山，通常一开始坡度很陡，所以你会迈较大的步幅，但当你接近底部时，坡度就不那么陡了，为了不错过底部，你会迈越来越小的步幅。PyTorch 提供 lr_scheduler 来调整学习速率。更多信息，请看[这段视频](https://www.youtube.com/watch?v=81NJgoR5RfY)。

```
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
model_ft = model_ft.to(device)criterion = nn.CrossEntropyLoss()# Observe that all parameters are being optimized
optimizer_ft = optim.SGD(model_ft.parameters(), lr=0.001, momentum=0.9)# Decay LR by a factor of 0.1 every 7 epochs
exp_lr_scheduler = lr_scheduler.StepLR(optimizer_ft, step_size=7, gamma=0.1)
```

## 火车模型

```
model_ft = train_model(model_ft, criterion, optimizer_ft, exp_lr_scheduler, num_epochs=25)
```

## 等等。有东西不见了

我必须承认，为了保持模式简单，我故意省略了一些东西:您必须定义 train_model 函数。不像 Keras API，在 PyTorch 中你需要定义训练循环。接下来，对于每个训练历元，它分批迭代输入图像，对于每一批，它首先将输入变量(图像张量)通过一个 f [正向通道](https://d2l.ai/chapter_multilayer-perceptrons/backprop.html)来计算模型输出，然后使用[反向传播](https://www.analyticsvidhya.com/blog/2021/06/how-does-backward-propagation-work-in-neural-networks/)来计算神经网络参数的梯度，以最小化损失函数。

```
def train_model(model, criterion, optimizer, scheduler, num_epochs=25):
    since = time.time()best_model_wts = copy.deepcopy(model.state_dict())
    best_acc = 0.0for epoch in range(num_epochs):
        print(f'Epoch {epoch}/{num_epochs - 1}')
        print('-' * 10)# Each epoch has a training and validation phase
        for phase in ['train', 'val']:
            if phase == 'train':
                model.train()  # Set model to training mode
            else:
                model.eval()   # Set model to evaluate moderunning_loss = 0.0
            running_corrects = 0# Iterate over data.
            for inputs, labels in dataloaders[phase]:
                inputs = inputs.to(device)
                labels = labels.to(device)# zero the parameter gradients
                optimizer.zero_grad()# forward
                # track history if only in train
                with torch.set_grad_enabled(phase == 'train'):
                    outputs = model(inputs)
                    _, preds = torch.max(outputs, 1)
                    loss = criterion(outputs, labels)# backward + optimize only if in training phase
                    if phase == 'train':
                        loss.backward()
                        optimizer.step()# statistics
                running_loss += loss.item() * inputs.size(0)
                running_corrects += torch.sum(preds == labels.data)
            if phase == 'train':
                scheduler.step()epoch_loss = running_loss / dataset_sizes[phase]
            epoch_acc = running_corrects.double() / dataset_sizes[phase]print(f'{phase} Loss: {epoch_loss:.4f} Acc: {epoch_acc:.4f}')# deep copy the model
            if phase == 'val' and epoch_acc > best_acc:
                best_acc = epoch_acc
                best_model_wts = copy.deepcopy(model.state_dict())print()time_elapsed = time.time() - since
    print(f'Training complete in {time_elapsed // 60:.0f}m {time_elapsed % 60:.0f}s')
    print(f'Best val Acc: {best_acc:4f}')# load best model weights
    model.load_state_dict(best_model_wts)
    return model
```

如您所见，这个函数中有许多样板代码。PyTorch Lightning 就是为了简化这一点而创建的。下次再来探索吧。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)