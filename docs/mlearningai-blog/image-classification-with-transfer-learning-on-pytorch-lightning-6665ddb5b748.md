# PyTorch 闪电的迁移学习图像分类

> 原文：<https://medium.com/mlearning-ai/image-classification-with-transfer-learning-on-pytorch-lightning-6665ddb5b748?source=collection_archive---------1----------------------->

## 提高深度学习代码的可读性和健壮性

在之前的文章 [1](/mlearning-ai/image-classification-with-transfer-learning-on-tensorflow-68b6bc87ef4b) 、 [2](/mlearning-ai/image-classification-with-transfer-learning-on-pytorch-2d718c85b58f) 中，我们回顾了用 Tensorflow/Keras 和 PyTorch 进行迁移学习的图像分类。Keras API 很简单，因为 Keras 是高级 API(与[低级 Tensorflow](https://towardsdatascience.com/tensorflow-vs-keras-d51f2d68fdfc) 相比)，然而，PyTorch 在训练函数中有许多样板代码。PyTorch 有没有类似 Keras 的东西？进入 [PyTorch 闪电](https://www.pytorchlightning.ai)的世界。

关于使用 PyTorch lightning 的好处，你可以参考下面的文章。

[](https://devblog.pytorchlightning.ai/why-should-i-use-pytorch-lightning-488760847b8b) [## 我为什么要用 PyTorch Lightning？

devblog.pytorchlightning.ai](https://devblog.pytorchlightning.ai/why-should-i-use-pytorch-lightning-488760847b8b) [](https://www.sabrepc.com/blog/Deep-Learning-and-AI/why-use-pytorch-lightning) [## 为什么应该使用 PyTorch Lightning 以及如何开始使用

### 随着您继续研究 Python 编码语言并探索它提供的各种框架，您可能会…

www.sabrepc.com](https://www.sabrepc.com/blog/Deep-Learning-and-AI/why-use-pytorch-lightning) 

如[文章](https://neptune.ai/blog/pytorch-lightning-vs-ignite-differences)中所述，PyTorch Lightning 具有以下关键特性:

*   **在任何硬件**上训练模型:CPU、GPU 或 TPU，无需更改源代码
*   **可读性**:减少不想要的或样板代码，将重点放在代码的研究方面
*   **删除不需要的或样板代码**
*   **界面**:简洁、整洁、易于导航
*   **更容易复制**
*   **可扩展**:你可以使用多个数学函数(优化器、激活函数、损失函数等等)
*   **可重用性**
*   **与可视化框架**集成，如 Neptune.ai、Tensorboard、MLFlow、Comet.ml、Wandb

这张[图片](https://pytorch-lightning.readthedocs.io/en/0.7.1/_images/pt_to_pl.jpg)展示了 PyTorch 和 PyTorch Lightning 之间的代码差异，以快速感受其优势。

# 利用迁移学习进行图像分类的一般步骤

1.  数据加载器
2.  预处理
3.  加载预训练模型，根据需要冻结模型层
4.  根据需要添加额外的层，以形成最终的模型
5.  编译模型，设置优化器和损失函数
6.  训练模型
7.  验证模型

通常，前两步与数据加载和预处理相关，其他步骤与模型本身相关(加载预训练模型、修改/添加模型层、设置损失函数和优化器、训练/验证)。下面我们将使用 PyTorch lightning 下面的例子来回顾一下代码模式是什么样子的。

[](https://github.com/PyTorchLightning/pytorch-lightning/blob/master/pl_examples/domain_templates/computer_vision_fine_tuning.py) [## py torch-lightning/computer _ vision _ fine _ tuning . py at master pytorch lightning/py torch-lightning

### 此文件包含双向 Unicode 文本，其解释或编译可能与下面显示的不同…

github.com](https://github.com/PyTorchLightning/pytorch-lightning/blob/master/pl_examples/domain_templates/computer_vision_fine_tuning.py) 

在此之前，请安装 PyTorch lightning

```
pip install pandas torch pytorch-lightning
```

# 步骤 0:导入必要的库

```
import logging
from pathlib import Path
from typing import Unionimport torch
import torch.nn.functional as F
from torch import nn, optim
from torch.optim.lr_scheduler import MultiStepLR
from torch.optim.optimizer import Optimizer
from torch.utils.data import DataLoader
from torchmetrics import Accuracy
from torchvision import models, transforms
from torchvision.datasets import ImageFolder
from torchvision.datasets.utils import download_and_extract_archiveimport pytorch_lightning as pl
from pytorch_lightning import LightningDataModule
from pytorch_lightning.callbacks.finetuning import BaseFinetuning
from pytorch_lightning import Trainer
from pytorch_lightning.callbacks import ModelCheckpoint, EarlyStoppinglog = logging.getLogger(__name__)
DATA_URL = "[https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip](https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip)"
```

# 步骤 1:点亮数据模块

像所有其他机器学习过程一样，您需要加载训练/验证数据集，进行一些预处理，这些将在以后的训练和验证中使用。LightningDataModule 封装了这些并给你一个遵循的食谱。如 LightningDataModule [API](https://pytorch-lightning.readthedocs.io/en/stable/extensions/datamodules.html#lightningdatamodule-api) 中所述，主要的 API 有:

*准备 _ 数据:下载并保存数据的地方*

*train_dataloader:提供对列车数据集*的访问

*val_dataloader:提供对验证数据集的访问*

*设置:一些初始化操作，例如构建词汇、执行训练/val/测试分割*

*test_dataloader:提供对测试数据集的访问*

*predict_dataloader:提供对预测数据集的访问*

如您所见，它加强了一些常见的实践，例如模型的训练/验证/测试、数据集准备的中心位置、训练初始化。

## 准备 _ 数据

```
class CatDogImageDataModule(LightningDataModule):
    def __init__(self, dl_path: Union[str, Path] = "data", num_workers: int = 0, batch_size: int = 8):
        """CatDogImageDataModule.
        Args:
            dl_path: root directory where to download the data
            num_workers: number of CPU workers
            batch_size: number of sample in a batch
        """
        super().__init__()self._dl_path = dl_path
        self._num_workers = num_workers
        self._batch_size = batch_sizedef prepare_data(self):
        """Download images and prepare images datasets."""
        download_and_extract_archive(url=DATA_URL, download_root=self._dl_path, remove_finished=True)
```

## train_dataloader

在这里，您可以看到它只是从训练图像文件夹加载训练数据集，应用训练转换。data_path、normalize_transform、create_dataset、__dataloader 方法只是在训练和验证数据集加载器之间共享的方法。

```
[@property](http://twitter.com/property)
    def data_path(self):
        return Path(self._dl_path).joinpath("cats_and_dogs_filtered")
[@property](http://twitter.com/property)
    def normalize_transform(self):
        return transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
[@property](http://twitter.com/property)
    def train_transform(self):
        return transforms.Compose(
            [
                transforms.Resize((224, 224)),
                transforms.RandomHorizontalFlip(),
                transforms.ToTensor(),
                self.normalize_transform,
            ]
        )

def create_dataset(self, root, transform):
        return ImageFolder(root=root, transform=transform)
def __dataloader(self, train: bool):
        """Train/validation loaders."""
        if train:
            dataset = self.create_dataset(self.data_path.joinpath("train"), self.train_transform)
        else:
            dataset = self.create_dataset(self.data_path.joinpath("validation"), self.valid_transform)
        return DataLoader(dataset=dataset, batch_size=self._batch_size, num_workers=self._num_workers, shuffle=train)
def train_dataloader(self):
        log.info("Training data loaded.")
        return self.__dataloader(train=True) 
```

## val_dataloader

从验证图像文件夹加载验证数据集，应用验证转换。

```
[@property](http://twitter.com/property)
    def valid_transform(self):
        return transforms.Compose([transforms.Resize((224, 224)), transforms.ToTensor(), self.normalize_transform])def val_dataloader(self):
        log.info("Validation data loaded.")
        return self.__dataloader(train=False)
```

# 第二步:照明模块

在[高电平](https://www.analyticsvidhya.com/blog/2020/02/mathematics-behind-convolutional-neural-network/) l 上，深度神经网络是取输入，取正向计算计算输出，然后用输出计算损耗，再用损耗做反向传播调整网络权值，重复直到达到某个稳定的“底部”使损耗最小。因此，您的模型中需要一个前向函数、损失函数、优化函数(通常是梯度下降)。与 LightningDataModule 类似， [LightningModule](https://pytorch-lightning.readthedocs.io/en/latest/common/lightning_module.html) 也提供了相应的 train/val/test/predict 方法。

## 初始化

设置预训练模型信息、学习率调节器、建立迁移学习模型(在 __build_model()方法中，resnet50 +全连接分类器)、损失函数。

```
class TransferLearningModel(pl.LightningModule):
    def __init__(
        self,
        backbone: str = "resnet50",
        train_bn: bool = False,
        milestones: tuple = (2, 4),
        batch_size: int = 32,
        lr: float = 1e-3,
        lr_scheduler_gamma: float = 1e-1,
        num_workers: int = 6,
        **kwargs,
    ) -> None:
        """TransferLearningModel.
        Args:
            backbone: Name (as in ``torchvision.models``) of the feature extractor
            train_bn: Whether the BatchNorm layers should be trainable
            milestones: List of two epochs milestones
            lr: Initial learning rate
            lr_scheduler_gamma: Factor by which the learning rate is reduced at each milestone
        """
        super().__init__()
        self.backbone = backbone
        self.train_bn = train_bn
        self.milestones = milestones
        self.batch_size = batch_size
        self.lr = lr
        self.lr_scheduler_gamma = lr_scheduler_gamma
        self.num_workers = num_workersself.__build_model()self.train_acc = Accuracy()
        self.valid_acc = Accuracy()
        self.save_hyperparameters()def __build_model(self):
        """Define model layers & loss."""# 1\. Load pre-trained network:
        model_func = getattr(models, self.backbone)
        backbone = model_func(pretrained=True)_layers = list(backbone.children())[:-1]
        self.feature_extractor = nn.Sequential(*_layers)# 2\. Classifier:
        _fc_layers = [nn.Linear(2048, 256), nn.ReLU(), nn.Linear(256, 32), nn.Linear(32, 1)]
        self.fc = nn.Sequential(*_fc_layers)# 3\. Loss:
        self.loss_func = F.binary_cross_entropy_with_logits
```

## 前进传球

```
def forward(self, x):
        """Forward pass.
        Returns logits.
        """
# 1\. Feature extraction:
        x = self.feature_extractor(x)
        x = x.squeeze(-1).squeeze(-1)
# 2\. Classifier (returns logits):
        x = self.fc(x)
return x
def loss(self, logits, labels):
        return self.loss_func(input=logits, target=labels)
```

## 训练步骤

向前传递，然后用输出和实际标签计算损耗。PyTorch Lightning 将通过批次和时期进行迭代，从训练方法中获得损失，并使用该损失进行反向传播。

```
def training_step(self, batch, batch_idx):
        # 1\. Forward pass:
        x, y = batch
        y_logits = self.forward(x)
        y_scores = torch.sigmoid(y_logits)
        y_true = y.view((-1, 1)).type_as(x)
# 2\. Compute loss
        train_loss = self.loss(y_logits, y_true)
# 3\. Compute accuracy:
        self.log("train_acc", self.train_acc(y_scores, y_true.int()), prog_bar=True)
return train_loss
```

## 验证步骤

类似于训练循环，向前传递，然后用输出和实际标签计算损耗。

```
def validation_step(self, batch, batch_idx):
        # 1\. Forward pass:
        x, y = batch
        y_logits = self.forward(x)
        y_scores = torch.sigmoid(y_logits)
        y_true = y.view((-1, 1)).type_as(x)
# 2\. Compute loss
        self.log("val_loss", self.loss(y_logits, y_true), prog_bar=True)
# 3\. Compute accuracy:
        self.log("val_acc", self.valid_acc(y_scores, y_true.int()), prog_bar=True)
```

## 配置优化程序

设置 Adam 优化器和学习率调节器

```
def configure_optimizers(self):
        parameters = list(self.parameters())
        trainable_parameters = list(filter(lambda p: p.requires_grad, parameters))
        optimizer = optim.Adam(trainable_parameters, lr=self.lr)
        scheduler = MultiStepLR(optimizer, milestones=self.milestones, gamma=self.lr_scheduler_gamma)
        return [optimizer], [scheduler]
```

# 训练循环

现在你可以创建一个训练者，并开始你的训练，PyTorch Lightning 将使你从编写样板训练循环中解脱出来。

```
dm = CatDogImageDataModule()
model = TransferLearningModel()
trainer = Trainer(max_epochs=3, progress_bar_refresh_rate=20,)
trainer.fit(model=model, datamodule=dm)
```

# 预言；预测；预告

与所有模型一样，它们只有在能够预测时才有用。要进行预测，您需要获取输入、加载模型并向前传递。PyTorch Lightning 提供了 predict_dataloader，但是，在模型预测与训练分离的情况下，下面是加载预测数据并进行预测的示例。

```
import os
from PIL import Imagefrom torchvision import transforms
img_test_transforms = transforms.Compose([
    transforms.Resize((224,224)),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],std=[0.229, 0.224, 0.225] )
    ])# send model weights to GPU, [https://stackoverflow.com/questions/59013109/runtimeerror-input-type-torch-floattensor-and-weight-type-torch-cuda-floatte](https://stackoverflow.com/questions/59013109/runtimeerror-input-type-torch-floattensor-and-weight-type-torch-cuda-floatte)
if torch.cuda.is_available():
    model.cuda()def find_classes(dir):
    classes = os.listdir(dir)
    classes.sort()
    class_to_idx = {classes[i]: i for i in range(len(classes))}
    return classes, class_to_idxdef make_prediction(model, filename):
    labels, _ = find_classes('/content/data/cats_and_dogs_filtered/validation')
    img = Image.open(filename)
    # tensor size should [RGB, image dimension, image dimension,]: torch.Size([3, 224, 224])
    img = img_test_transforms(img)
    # make tensor first dimension as rows of images, torch.Size([1, 3, 224, 224])
    img = img.unsqueeze(0)
    model.eval()
    # align image bits and model weights on same GPU or CPU
    device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
    outputs = model(img.to(device))
    _, predicted = torch.max(outputs.data, 1)
    print(predicted)make_prediction(model, 'data/cats_and_dogs_filtered/validation/cats/cat.2000.jpg')# tensor([0], device='cuda:0')
```

# 附录

下面这篇文章给出了 PyTorch Lightning 与 PyTorch 在相同的机器学习问题上进行比较的很好的例子。

[](https://towardsdatascience.com/from-pytorch-to-pytorch-lightning-a-gentle-introduction-b371b7caaf09) [## 从 PyTorch 到 py torch Lightning——一个温和的介绍

### 这篇文章对使用 PyTorch 和 PyTorch Lightning 实现的 MNIST 进行了对比。

towardsdatascience.com](https://towardsdatascience.com/from-pytorch-to-pytorch-lightning-a-gentle-introduction-b371b7caaf09) [](https://www.assemblyai.com/blog/pytorch-lightning-for-dummies/) [## 假人用 PyTorch 闪电-教程和概述

### 在训练深度学习模型时，有许多标准的“样板”代码，它们独立于…

www.assemblyai.com](https://www.assemblyai.com/blog/pytorch-lightning-for-dummies/) [](/aimluae/tensorflow-vs-pytorch-which-will-be-the-top-deep-learning-framework-in-2022-a3488635029) [## tensor flow vs py torch:2022 年顶级深度学习框架会是哪个？

### PyTorch 和 Tensorflow 是目前最受欢迎的两个深度学习库。PyTorch 是由…

medium.com](/aimluae/tensorflow-vs-pytorch-which-will-be-the-top-deep-learning-framework-in-2022-a3488635029) [](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)