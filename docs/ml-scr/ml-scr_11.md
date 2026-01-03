# 实现

> 原文：[`dafriedman97.github.io/mlbook/content/c2/code.html`](https://dafriedman97.github.io/mlbook/content/c2/code.html)

本节展示了本章讨论的线性回归扩展在 Python 中的典型拟合方法。首先，让我们导入 Boston 住房数据集。

```py
import numpy as np 
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn import datasets
boston = datasets.load_boston()
X_train = boston['data']
y_train = boston['target'] 
```

## 正则化回归

Ridge 回归和 Lasso 回归都可以很容易地使用`scikit-learn`进行拟合。下面提供了一个基本的实现示例。请注意，正则化参数`alpha`（我们称之为\(\lambda\)）是任意选择的。

```py
from sklearn.linear_model import Ridge, Lasso
alpha = 1

# Ridge
ridge_model = Ridge(alpha = alpha)
ridge_model.fit(X_train, y_train)

# Lasso
lasso_model = Lasso(alpha = alpha)
lasso_model.fit(X_train, y_train); 
```

然而，在实践中，我们希望通过交叉验证来选择`alpha`。这通过指定一组要尝试的`alpha`值并在`RidgeCV`或`LassoCV`中拟合模型很容易地在`scikit-learn`中实现。

```py
from sklearn.linear_model import RidgeCV, LassoCV
alphas = [0.01, 1, 100]

# Ridge
ridgeCV_model = RidgeCV(alphas = alphas)
ridgeCV_model.fit(X_train, y_train)

# Lasso
lassoCV_model = LassoCV(alphas = alphas)
lassoCV_model.fit(X_train, y_train); 
```

我们可以看到以下哪些`alpha`值表现最好。

```py
print('Ridge alpha:', ridgeCV.alpha_)
print('Lasso alpha:', lassoCV.alpha_) 
```

```py
Ridge alpha: 0.01
Lasso alpha: 1.0 
```

## 贝叶斯回归

我们也可以使用`scikit-learn`（尽管另一个流行的包是`pymc3`）来拟合贝叶斯回归。下面提供了一个非常直接的实现示例。

```py
from sklearn.linear_model import BayesianRidge
bayes_model = BayesianRidge()
bayes_model.fit(X_train, y_train); 
```

然而，这并不等同于前一部分中的构造，因为它推断\(\sigma²\)和\(\tau\)参数，而不是将它们作为固定输入。更多信息可以在[这里](https://scikit-learn.org/stable/modules/linear_model.html#bayesian-regression)找到。下面的隐藏部分展示了在`scikit-learn`中使用已知的\(\sigma²\)和\(\tau\)值运行贝叶斯回归的笨拙解决方案，尽管很难想象有实际的理由这样做。

默认情况下，`scikit-learn`中的贝叶斯回归将\(\alpha = \frac{1}{\sigma²}\)和\(\lambda = \frac{1}{\tau}\)视为随机变量，并赋予它们以下先验分布

\[\begin{split} \begin{aligned} \alpha &\sim \text{Gamma}(\alpha_1, \alpha_2) \\ \lambda &\sim \text{Gamma}(\lambda_1, \lambda_2). \end{aligned} \end{split}\]

注意，\(E(\alpha) = \frac{\alpha_1}{\alpha_2}\)和\(E(\lambda) = \frac{\lambda_1}{\lambda_2}\)。为了*固定* \(\sigma²\)和\(\tau\)，我们可以为\(\alpha\)和\(\lambda\)提供一个非常强的先验，从而保证它们的估计值将大约等于它们的期望值。

假设我们想使用\(\sigma² = 11.8\)和\(\tau = 10\)，或者等价地\(\alpha = \frac{1}{11.8}\)，\(\lambda = \frac{1}{10}\)。那么，让我们

\[\begin{split} \begin{aligned} \alpha_1 &= 10000 \cdot \frac{1}{11.8}, \\ \alpha_2 &= 10000, \\ \lambda_1 &= 10000 \cdot \frac{1}{10}, \\ \lambda_2 &= 10000. \end{aligned} \end{split}\]

这保证了\(\sigma²\)和\(\tau\)将大约等于它们预定的值。这可以在`scikit-learn`中如下实现

```py
big_number = 10**5

# alpha
alpha = 1/11.8
alpha_1 = big_number*alpha
alpha_2 = big_number

# lambda 
lam = 1/10
lambda_1 = big_number*lam
lambda_2 = big_number

# fit 
bayes_model = BayesianRidge(alpha_1 = alpha_1, alpha_2 = alpha_2, alpha_init = alpha,
                     lambda_1 = lambda_1, lambda_2 = lambda_2, lambda_init = lam)
bayes_model.fit(X_train, y_train); 
```

## 泊松回归

GLMs 通常通过`statsmodels`中的`GLM`类在 Python 中进行拟合。下面提供了一个简单的泊松回归示例。

正如我们在 GLM 概念部分中看到的，GLM 由一个随机分布和一个链接函数组成。我们通过`GLM`函数的`family`参数来识别随机分布（例如，下面我们指定了`Poisson`分布）。默认链接函数取决于随机分布。默认情况下，泊松模型使用以下链接函数

\[ \eta_n = g(\mu_n) = \log(\lambda_n), \]

这就是我们下面使用的。有关可能的分布和链接函数的更多信息，请查看`statsmodels` GLM [文档](https://www.statsmodels.org/stable/glm.html)。

```py
import statsmodels.api as sm
X_train_with_constant = sm.add_constant(X_train)

poisson_model = sm.GLM(y_train, X_train, family=sm.families.Poisson())
poisson_model.fit(); 
```

## 正则化回归

Ridge 和 Lasso 回归都可以很容易地使用`scikit-learn`进行拟合。下面提供了一个基本的实现。请注意，正则化参数`alpha`（我们称之为\(\lambda\)）是任意选择的。

```py
from sklearn.linear_model import Ridge, Lasso
alpha = 1

# Ridge
ridge_model = Ridge(alpha = alpha)
ridge_model.fit(X_train, y_train)

# Lasso
lasso_model = Lasso(alpha = alpha)
lasso_model.fit(X_train, y_train); 
```

然而，在实践中，我们希望通过交叉验证来选择`alpha`。这通过在`scikit-learn`中指定一组要尝试的`alpha`值并通过`RidgeCV`或`LassoCV`拟合模型来实现，这是很容易的。

```py
from sklearn.linear_model import RidgeCV, LassoCV
alphas = [0.01, 1, 100]

# Ridge
ridgeCV_model = RidgeCV(alphas = alphas)
ridgeCV_model.fit(X_train, y_train)

# Lasso
lassoCV_model = LassoCV(alphas = alphas)
lassoCV_model.fit(X_train, y_train); 
```

然后，我们可以看到以下哪些`alpha`值表现最好。

```py
print('Ridge alpha:', ridgeCV.alpha_)
print('Lasso alpha:', lassoCV.alpha_) 
```

```py
Ridge alpha: 0.01
Lasso alpha: 1.0 
```

## 贝叶斯回归

我们也可以使用`scikit-learn`进行贝叶斯回归（尽管另一个流行的包是`pymc3`）。下面提供了一个非常直接的实现。

```py
from sklearn.linear_model import BayesianRidge
bayes_model = BayesianRidge()
bayes_model.fit(X_train, y_train); 
```

然而，这并不等同于上一节中的我们的构造，因为它推断出\(\sigma²\)和\(\tau\)参数，而不是将它们作为固定输入。更多信息可以在[这里](https://scikit-learn.org/stable/modules/linear_model.html#bayesian-regression)找到。下面的隐藏部分演示了一个在`scikit-learn`中使用已知的\(\sigma²\)和\(\tau\)值进行贝叶斯回归的笨拙解决方案，尽管很难想象这样做有实际的理由。

默认情况下，`scikit-learn`中的贝叶斯回归将\(\alpha = \frac{1}{\sigma²}\)和\(\lambda = \frac{1}{\tau}\)视为随机变量，并赋予它们以下先验分布

\[\begin{split} \begin{aligned} \alpha &\sim \text{Gamma}(\alpha_1, \alpha_2) \\ \lambda &\sim \text{Gamma}(\lambda_1, \lambda_2). \end{aligned} \end{split}\]

注意，\(E(\alpha) = \frac{\alpha_1}{\alpha_2}\)和\(E(\lambda) = \frac{\lambda_1}{\lambda_2}\)。为了*固定* \(\sigma²\)和\(\tau\)，我们可以为\(\alpha\)和\(\lambda\)提供一个非常强的先验，从而保证它们的估计值将大约等于它们的期望值。

假设我们想使用\(\sigma² = 11.8\)和\(\tau = 10\)，或者等价地\(\alpha = \frac{1}{11.8}\)，\(\lambda = \frac{1}{10}\)。那么让我们

\[\begin{split} \begin{aligned} \alpha_1 &= 10000 \cdot \frac{1}{11.8}, \\ \alpha_2 &= 10000, \\ \lambda_1 &= 10000 \cdot \frac{1}{10}, \\ \lambda_2 &= 10000. \end{aligned} \end{split}\]

这保证了\(\sigma²\)和\(\tau\)将大约等于它们预定的值。这可以在`scikit-learn`中如下实现

```py
big_number = 10**5

# alpha
alpha = 1/11.8
alpha_1 = big_number*alpha
alpha_2 = big_number

# lambda 
lam = 1/10
lambda_1 = big_number*lam
lambda_2 = big_number

# fit 
bayes_model = BayesianRidge(alpha_1 = alpha_1, alpha_2 = alpha_2, alpha_init = alpha,
                     lambda_1 = lambda_1, lambda_2 = lambda_2, lambda_init = lam)
bayes_model.fit(X_train, y_train); 
```

## 泊松回归

GLM 通常在 Python 中通过`statsmodels`的`GLM`类进行拟合。下面给出了一个简单的泊松回归示例。

正如我们在 GLM 概念部分所看到的，GLM 由一个随机分布和一个链接函数组成。我们通过`GLM`的`family`参数来识别随机分布（例如，下面我们指定了`Poisson`分布）。默认链接函数取决于随机分布。默认情况下，泊松模型使用链接函数

\[ \eta_n = g(\mu_n) = \log(\lambda_n), \]

这就是我们下面将要使用的。有关可能的分布和链接函数的更多信息，请查看`statsmodels` GLM [文档](https://www.statsmodels.org/stable/glm.html)。

```py
import statsmodels.api as sm
X_train_with_constant = sm.add_constant(X_train)

poisson_model = sm.GLM(y_train, X_train, family=sm.families.Poisson())
poisson_model.fit(); 
```
