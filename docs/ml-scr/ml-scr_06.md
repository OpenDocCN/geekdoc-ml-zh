# 构建过程

> 原文：[`dafriedman97.github.io/mlbook/content/c1/construction.html`](https://dafriedman97.github.io/mlbook/content/c1/construction.html)

$$ \newcommand{\sumN}{\sum_{n = 1}^N} \newcommand{\sumn}{\sum_n} \newcommand{\bx}{\mathbf{x}} \newcommand{\bbeta}{\boldsymbol{\beta}} \newcommand{\btheta}{\boldsymbol{\theta}} \newcommand{\bbetahat}{\boldsymbol{\hat{\beta}}} \newcommand{\bthetahat}{\boldsymbol{\hat{\theta}}} \newcommand{\dadb}[2]{\frac{\partial #1}{\partial #2}} \newcommand{\by}{\mathbf{y}} \newcommand{\bX}{\mathbf{X}} $$

本节演示了如何仅使用`numpy`构建线性回归模型。为此，我们创建了一个名为`LinearRegression`的类。我们使用这个类来训练模型和进行未来的预测。

`LinearRegression`类中的第一个方法是`fit()`，它负责估计$\bbeta$参数。这仅仅包括计算

$$ \bbetahat = \left(\bX^\top \bX\right)^{-1}\bX^\top \by $$

`fit`方法也在样本内进行预测，$\hat{\by} = \bX \bbetahat$，并计算训练损失

$$ \mathcal{L}(\bbetahat) = \frac{1}{2}\sumN \left(y_n - \hat{y}_n \right)². $$

第二种方法是`predict()`，它形成样本外预测。给定一个预测变量测试集$\bX'$，我们可以用$\hat{\by}' = \bX' \bbetahat$来形成拟合值。

```py
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns 
```

```py
class LinearRegression:

    def fit(self, X, y, intercept = False):

        # record data and dimensions
        if intercept == False: # add intercept (if not already included)
            ones = np.ones(len(X)).reshape(len(X), 1) # column of ones 
            X = np.concatenate((ones, X), axis = 1)
        self.X = np.array(X)
        self.y = np.array(y)
        self.N, self.D = self.X.shape

        # estimate parameters
        XtX = np.dot(self.X.T, self.X)
        XtX_inverse = np.linalg.inv(XtX)
        Xty = np.dot(self.X.T, self.y)
        self.beta_hats = np.dot(XtX_inverse, Xty)

        # make in-sample predictions
        self.y_hat = np.dot(self.X, self.beta_hats)

        # calculate loss
        self.L = .5*np.sum((self.y - self.y_hat)**2)

    def predict(self, X_test, intercept = True):

        # form predictions
        self.y_test_hat = np.dot(X_test, self.beta_hats) 
```

让我们用一些数据来测试我们的`LinearRegression`类。这里我们使用来自`sklearn.datasets`的 Boston housing 数据集。该数据集的目标变量是中位数社区房价。预测变量都是连续的，代表可能与中位数房价相关的因素，例如每户平均房间数。点击“显示代码”来查看加载此数据的代码。

```py
from sklearn import datasets
boston = datasets.load_boston()
X = boston['data']
y = boston['target'] 
```

在构建了类并加载了数据后，我们就可以运行我们的回归模型了。这就像实例化模型并应用`fit()`一样简单，如下所示。

```py
model = LinearRegression() # instantiate model
model.fit(X, y, intercept = False) # fit model 
```

然后让我们看看我们的拟合值模型如何模拟真实的目标值。点越接近 45 度线，拟合越准确。模型似乎做得相当不错；我们的预测确实很好地遵循了真实值，尽管我们希望拟合更紧密一些。

注意

注意那些$y = 50$的观测值。这是由于数据收集过程中的审查所致。看起来平均房价超过 50,000 美元的社区甚至被分配了一个 50 的值。

```py
fig, ax = plt.subplots()
sns.scatterplot(model.y, model.y_hat)
ax.set_xlabel(r'$y$', size = 16)
ax.set_ylabel(r'$\hat{y}$', rotation = 0, size = 16, labelpad = 15)
ax.set_title(r'$y$ vs. $\hat{y}$', size = 20, pad = 10)
sns.despine() 
```

![../../_images/construction_10_0.png](img/da7ad87ad14998b49f5c2665f1e949d7.png)
