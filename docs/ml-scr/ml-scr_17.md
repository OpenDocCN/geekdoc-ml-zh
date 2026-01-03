# 概念

> 原文：[`dafriedman97.github.io/mlbook/content/c4/concept.html`](https://dafriedman97.github.io/mlbook/content/c4/concept.html)

\[ \newcommand{\sumN}{\sum_{n = 1}^N} \newcommand{\sumn}{\sum_n} \newcommand{\sumK}{\sum_{k = 1}^K} \newcommand{\sumk}{\sum_k} \newcommand{\prodN}{\prod_{n = 1}^N} \newcommand{\prodK}{\prod_{k = 1}^K} \newcommand{\by}{\mathbf{y}} \newcommand{\bX}{\mathbf{X}} \newcommand{\bx}{\mathbf{x}} \newcommand{\bp}{\mathbf{p}} \newcommand{\bbeta}{\boldsymbol{\beta}} \newcommand{\btheta}{\boldsymbol{\theta}} \newcommand{\bmu}{\boldsymbol{\mu}} \newcommand{\bpi}{\boldsymbol{\pi}} \newcommand{\bbetahat}{\boldsymbol{\hat{\beta}}} \newcommand{\bthetahat}{\boldsymbol{\hat{\theta}}} \newcommand{\bSigma}{\boldsymbol{\Sigma}} \newcommand{\bT}{\mathbf{T}} \newcommand{\dadb}[2]{\frac{\partial #1}{\partial #2}} \newcommand{\iid}{\overset{\small{\text{i.i.d.}}}{\sim}} \newcommand{\collection}{\{\bx_n, y_n\}_{n = 1}^N} \newcommand{\l}{\left(} \newcommand{\r}{\right)} \]

如前一章所述，判别分类器将目标变量建模为一个或多个预测器的直接函数。本章的主题生成分类器，则认为预测器是根据其类别生成的——即，它们将预测器视为目标函数，而不是相反。然后，它们使用贝叶斯定理将 \(P(\bx_n|Y_n = k)\) 转换为 \(P(Y_n = k|\bx_n)\)。

在生成分类器中，我们将目标和预测器视为随机变量。因此，我们将目标变量称为 \(Y_n\)，但为了避免与矩阵混淆，我们将预测器向量称为 \(\bx_n\)。

生成模型可以分解为以下三个步骤。假设我们有一个具有 \(K\) 个无序类别的分类任务，这些类别由 \(k = 1, \dots, K\) 表示。

1.  在目标属于每个类别的条件下估计预测器的密度。即，估计 \(p(\bx_n|Y_n = k)\) 对于 \(k = 1, \dots, K\)。

1.  估计目标属于任何给定类别的先验概率。即，估计 \(P(Y_n = k)\) 对于 \(k = 1, \dots, K\)。这也可以写成 \(p(Y_n)\)。

1.  使用贝叶斯定理计算目标属于任何给定类别的后验概率。即，计算 \(p(Y_n = k|\bx_n) \propto p(\bx_n|Y_n = k)p(Y_n = k)\) 对于 \(k = 1, \dots, K\)。

然后，我们将观察 \(n\) 分类为 \(P(Y_n = k|\bx_n)\) 最大的类别。在数学上，

\[ \hat{Y}_n = \underset{k}{\text{arg max }} p(Y_n = k|\bx_n). \]

注意，我们不需要 \(p(\bx_n)\)，这是贝叶斯定理公式中的分母，因为它在各个类别中是相等的。

注意

本章的侧重点与其他章节不同。讨论的主要方法——线性判别分析、二次判别分析和朴素贝叶斯——具有许多相同的结构。我们不是分别介绍它们，而是将它们一起描述，并在第 2.2 节中说明它们之间的区别。

## 1. 模型结构

生成式分类器模型了两种随机性来源。首先，我们假设在 \(K\) 个可能的类别中，每个观察值独立地属于类别 \(k\)，概率为 \(\pi_k\)。换句话说，让 \(\bpi =\begin{bmatrix} \pi_1 & \dots & \pi_K\end{bmatrix}^\top \in \mathbb{R}^{K}\)，我们假设先验

\[ y_n \iid \text{Cat}(\bpi). \]

有关分类分布的数学注释请见下文。

数学注释

一个随机变量，取 \(K\) 个离散且无序的结果之一，其概率为 \(\pi_1, \dots, \pi_K\)，遵循参数为 \(\bpi = \begin{bmatrix} \pi_1 & \dots & \pi_K \end{bmatrix}^\top\) 的分类分布，记作 \(\text{Cat}(\bpi)\)。例如，单次掷骰子的分布为 \(\text{Cat}(\bpi)\)，其中 \(\bpi = \begin{bmatrix} 1/6 \dots 1/6 \end{bmatrix}^\top\)。

\(Y \sim \text{Cat}(\bp)\) 的密度定义为

\[\begin{split} \begin{align*} P(Y = 1) &= p_1 \\ &... \\ P(Y = K) &= p_K. \end{align*} \end{split}\]

这可以更简洁地写成

\[ p(y) = \prod_{k = 1}^K p_k ^{I_k} \]

其中 \(I_k\) 是一个指示变量，当 \(y = k\) 时等于 1，否则为 0。

然后，我们假设在观察 \(n\) 的类别 \(Y_n\) 的条件下，\(\mathbf{x}_n\) 服从某种分布。我们通常假设所有 \(\bx_n\) 都来自同一 *家族* 的分布，尽管参数依赖于它们的类别。例如，我们可能有

\[\begin{split} \begin{align*} \bx_n|(Y_n = 1) &\sim \text{MVN}(\bmu_1, \bSigma_1), \\ &... \\ \bx_{n}|(Y_n = K) &\sim \text{MVN}(\bmu_K, \bSigma_K), \end{align*} \end{split}\]

尽管我们不会让一个条件分布是多元正态分布，而另一个是多元 \(t\) 分布。注意，然而，随机向量 \(\bx_n\) 中的单个变量可以遵循不同的分布。例如，如果 \(\bx_n = \begin{bmatrix} x_{n1} & x_{n2} \end{bmatrix}^\top\)，我们可能有

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \text{Bin}(n, p_k) \\ x_{n2}|(Y_n = k) &\sim \mathcal{N}(\bmu_k, \bSigma_k) \end{align*} \end{split}\]

机器学习任务是对这些模型的参数进行估计——\(Y_n\) 的 \(\bpi\) 以及可能索引 \(\bx_n|Y_n\) 的可能分布的参数，在这种情况下是 \(\bmu_k\) 和 \(\bSigma_k\) 对于 \(k = 1, \dots, K\)。一旦完成，我们就可以估计每个类别的 \(p(Y_n = k)\) 和 \(p(\bx_n|Y_n = k)\)，并通过贝叶斯定理选择最大化 \(p(Y_n = k|\bx_n)\) 的类别。

## 2. 参数估计

### 2.1 类别先验

让我们先推导 \(\bpi\)，即类别先验的估计。令 \(I_{nk}\) 为一个指示变量，当 \(Y_n = k\) 时等于 1，否则为 0。那么联合似然和对数似然由以下给出

\[\begin{split} \begin{align*} L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \prod_{n = 1}^N \prod_{k = 1}^K \pi_k^{I_{nk}} \\ \log L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \sum_{n = 1}^N \sum_{k = 1}^K I_{nk} \log(\pi_k) \\ &= \sum_{k = 1}^K N_k\log(\pi_k), \end{align*} \end{split}\]

其中 \(N_k = \sum_{n = 1}^N I_{nk}\) 给出了 \(k = 1, \dots, K\) 时类别 \(k\) 的观察数量。

数学注释

*拉格朗日函数* 提供了一种在约束 \(g(\bx) = 0\) 下优化函数 \(f(\bx)\) 的方法。拉格朗日函数由以下给出

\[ \mathcal{L}(\lambda, \bx) = f(\bx) - \lambda g(\bx). \]

\(\lambda\) 被称为 *拉格朗日乘子*。通过将 \(\mathcal{L}(\lambda, \bx)\) 关于 \(\lambda\) 和 \(\bx\) 的梯度设为 0，可以找到 \(f(\bx)\)（在等式约束下）的临界点。

注意到约束 \(\sum_{k = 1}^K \pi_k = 1\)（或者等价地 \(\sum_{k = 1}^K\pi_k - 1 = 0\)），我们可以通过以下拉格朗日乘子最大化对数似然。

\[\begin{split} \begin{align*} \mathcal{L}(\bpi) &= \sum_{k = 1}^K N_k \log(\pi_k) - \lambda(\sum_{k = 1}^K \pi_k - 1). \\ \frac{\partial \mathcal{L}(\bpi)}{\partial \pi_k} &= \frac{N_k}{\pi_k} - \lambda, \hspace{3mm}\forall \hspace{1mm}k \in \{1, \dots, K\} \\ \frac{\partial \mathcal{L}(\bpi)}{\partial \lambda} &= 1 - \sum_{k = 1}^K \pi_k. \\ \end{align*} \end{split}\]

这个方程组给出了一种直观的解法：

\[ \hat{\pi}_k = \frac{N_k}{N}, \hspace{1mm} \lambda = N, \]

这意味着我们估计 \(p(Y_n = k)\) 的方法就是来自类别 \(k\) 的观察样本的样本比例。

### 2.2 数据似然

下一步是建模给定 \(Y_n\) 的 \(\bx_n\) 的条件分布，以便我们可以估计这个分布的参数。这当然取决于我们选择用于建模 \(\bx_n\) 的分布族。以下详细介绍了三种常见的方法。

#### 2.2.1 线性判别分析 (LDA)

在 LDA 中，我们假设

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma), \]

对于 \(k = 1, \dots, K\)。注意，每个类别具有相同的协方差矩阵，但具有唯一的均值向量。

让我们推导出这种情况下的参数。首先，让我们找到似然和对数似然。注意，我们可以将联合似然写成以下形式，

\[ L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = \prod_{n = 1}^N \prod_{k = 1}^K \Big(p\l\bx_n|\bmu_k, \bSigma\r\Big)^{I_{nk}}, \]

由于 \(\left(p(\bx_{n}|\bmu_k, \bSigma)\right)^{I_{nk}}\) 在 \(y_n \neq k\) 时等于 1，而在 \(p(\bx_n|\bmu_k, \bSigma)\) 时则不等于 1。然后我们插入多元正态概率密度函数（省略乘性常数）并取对数，如下所示。

\[\begin{split} \begin{align*} L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \prod_{n = 1}^N \prod_{k = 1}^K \Big(\frac{1}{\sqrt{|\bSigma|}}\exp\left\{-\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k)\right\}\Big)^{I_{nk}} \\ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \sum_{n = 1}^N \sum_{k = 1}^K I_{nk}\l-\frac{1}{2} \log|\bSigma| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k) \r \end{align*} \end{split}\]

数学注释

以下矩阵导数将用于最大化上述对数似然。

对于任何可逆矩阵 \(\mathbf{W}\)，

\[ \dadb{|\mathbf{W}|}{\mathbf{W}} = |\mathbf{W}|\mathbf{W}^{-\top}, \tag{1} \]

其中 \(\mathbf{W}^{-\top} = (\mathbf{W}^{-1})^\top\). 因此，

\[ \dadb{\log |\mathbf{W}|}{\mathbf{W}} = \mathbf{W}^{-\top}. \tag{2} \]

我们还有

\[ \left[ \dadb{\bx^\top \mathbf{W}^{-1} \bx}{\mathbf{W}} \right] = -\mathbf{W}^{-\top} \bx \bx^\top \mathbf{W}^{-\top}. \tag{3} \]

对于任何对称矩阵 \(\mathbf{A}\)，

\[ \dadb{(\bx - \mathbf{s})^\top \mathbf{A} (\bx - \mathbf{s})}{\mathbf{s}} = -2\mathbf{A}(\bx - \mathbf{s}). \tag{4} \]

这些结果来自 [矩阵手册](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)。

让我们先估计 \(\bSigma\)。首先，简化对数似然，以便更明显地看到关于 \(\bSigma\) 的梯度。

\[ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = - \frac{N}{2}\log |\bSigma| -\frac{1}{2} \sumN\sumK I_{nk}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k). \]

然后，使用 *数学笔记* 中的方程 (2) 和 (3)，我们得到

\[ \begin{align*} \dadb{\log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r }{\bSigma} &= -\frac{N}{2}\bSigma^{-\top} + \frac{1}{2}\sumN\sumK I_{nk}\bSigma^{-\top} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\bSigma^{-\top}. \end{align*} \]

最后，我们将此等于 0 并将 \(\bSigma^{-1}\) 乘以左侧以求解 \(\hat{\bSigma}\)：

\[\begin{split} \begin{align*} 0 &= -\frac{N}{2} + \frac{1}{2}\l\sumN\sumK I_{nk} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\r\bSigma^{-\top} \\ \bSigma^\top &= \frac{1}{N}\sumN \sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma} &= \frac{1}{N}\mathbf{S}_T, \end{align*} \end{split}\]

其中 \(\mathbf{S}_T = \sumN\sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

现在，为了估计 \(\bmu_k\)，让我们逐个查看每个类别。设 \(N_k\) 为类别 \(k\) 中的观测数，\(C_k\) 为类别 \(k\) 中的观测集合。仅考虑涉及 \(\bmu_k\) 的项，我们得到

\[ \begin{align*} \log L(\bmu_k, \bSigma) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma| + (\bx_n - \bmu_k)^\top \bSigma^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

使用 *数学笔记* 中的方程 (4)，我们计算梯度如下

\[ \dadb{\log L(\bmu_k, \bSigma)}{\bmu_k} =\sum_{n \in C_k} \bSigma^{-1}(\bx_n - \bmu_k). \]

将此梯度设为 0 并求解，我们得到 \(\bmu_k\) 的估计：

\[ \hat{\bmu}_k = \frac{1}{N_k} \sum_{n \in C_k} \bx_n = \bar{\bx}_k, \]

其中 \(\bar{\bx}_k\) 是类别 \(k\) 中所有 \(\bx_n\) 的逐元素样本均值。

#### 2.2.2 二次判别分析 (QDA)

QDA 看起来与 LDA 非常相似，但假设每个类别都有自己的协方差矩阵：

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma_k) \]

对于 \(k = 1, \dots, K\)。对数似然与 LDA 相同，只是将 \(\bSigma\) 替换为 \(\bSigma_k\)：

\[ \begin{align*} \log L\l\{\bmu_k, \bSigma_k\}_{k = 1}^K\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma_k| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma_k^{-1}(\bx_n - \bmu_k) \r. \end{align*} \]

再次，让我们单独查看每个类别的参数。类别 \(k\) 的对数似然函数由以下给出

\[ \begin{align*} \log L(\bmu_k, \bSigma_k) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma_k| + (\bx_n - \bmu_k)^\top \bSigma_k^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

我们可以取这个对数似然函数关于 \(\bmu_k\) 的梯度，并将其设为 0 来求解 \(\hat{\bmu}_k\)。然而，我们也可以注意到，从 LDA 方法得到的 \(\hat{\bmu}_k\) 估计将保持不变，因为此表达式不依赖于协方差项（这是我们唯一改变的东西）。因此，我们再次得到

\[ \hat{\bmu}_k = \bar{\bx}_k. \]

为了估计 \(\bSigma_k\)，我们取类别 \(k\) 的对数似然函数的梯度。

\[ \begin{align*} \dadb{\log L(\bmu_k, \bSigma_k) }{\bSigma_k} &= -\frac{1}{2}\sum_{n \in C_k} \left( \bSigma_k^{-\top} - \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \right). \end{align*} \]

然后我们将这个梯度设为 0 来求解 \(\hat{\bSigma}_k\)：

\[\begin{split} \begin{align*} \sum_{n \in C_k} \bSigma_k^{-\top} &= \sum_{n \in C_k} \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ N_k I &= \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ \bSigma_k^\top &= \frac{1}{N_k} \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma}_k &= \frac{1}{N_k} \mathbf{S}_k, \end{align*} \end{split}\]

其中 \(\mathbf{S}_k = \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

#### 2.2.3 朴素贝叶斯

基于朴素贝叶斯，假设在观察类别 \(n\) 的条件下，\(\bx_n\) 中的随机变量是独立的。即如果 \(\bx_n \in \mathbb{R}^D\)，朴素贝叶斯假设

\[ p(\bx_n|Y_n) = p(x_{n1}|Y_n)\cdot p(x_{n2}|Y_n) \cdot ... \cdot p(x_{nD}|Y_n). \]

这使得估计 \(p(\bx_n|Y_n)\) 非常简单——为了估计 \(p(x_{nd}|Y_n)\) 的参数，我们可以忽略 \(\bx_{n}\) 中除了 \(x_{nd}\) 以外的所有变量！

例如，假设 \(\bx_n \in \mathbb{R}²\) 并且我们使用以下模型（为了简单起见，\(n\) 和 \(\sigma²_k\) 是已知的）。

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \mathcal{N}(\mu_k, \sigma²_k) \\ x_{n2}|(Y_n = k) &\sim \text{Bin}(n, p_k). \end{align*} \end{split}\]

令 \(\btheta_k = (\mu_k, \sigma_k², p_k)\) 包含类别 \(k\) 的所有参数。联合似然函数将变为

\[\begin{split} \begin{align*} L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(\bx|Y_n, \btheta_k)\r^{I_{nk}} \\ L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(x_{n1}|\mu_k, \sigma²_k) \cdot p(x_{n2}|p_k) \r^{I_{nk}}, \end{align*} \end{split}\]

由于朴素贝叶斯条件独立性假设，这两个是相等的。这使我们能够轻松找到最大似然估计。本小节的其余部分展示了如何找到这些估计，尽管这并不超出普通最大似然估计的范围。

对数似然由以下给出

\[ \log L(\{\btheta_k\}_{k = 1}^K) = \sumN\sumK I_{nk}\l\log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \r. \]

与之前一样，我们通过只观察该类中的项来估计每个类别的参数。让我们看看类别 \(k\) 的对数似然：

\[\begin{split} \begin{align*} \log L(\btheta_k) &= \sum_{n \in C_k} \log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \\ &= \sum_{n \in C_k} -\frac{(x_{n1} - \mu_k)²}{2\sigma²_k} + x_{n2}\log(p_k) + (1-x_{n2})\log(1-p_k). \end{align*} \end{split}\]

对 \(p_k\) 求导，我们得到

\[ \dadb{\log L(\btheta_k)}{p_k} = \sum_{n \in C_k}\frac{x_{n2}}{p_k} - \frac{1-x_{n2}}{1-p_k}, \]

这将给出 \(\hat{p}_k = \frac{1}{N_k}\sum_{n \in C_k} x_{n2}\) 如常。同样的过程还会给出 \(\mu_k\) 和 \(\sigma²_k\) 的典型结果。

## 3. 进行分类

无论我们对 \(p(\bx_n|Y_n)\) 的建模选择如何，对新观测值的分类都是容易的。考虑一个测试观测值 \(\bx_0\)。对于 \(k = 1, \dots, K\)，我们使用贝叶斯定理来计算

\[\begin{split} \begin{align*} p(Y_0 = k|\bx_0) &\propto p(\bx_0|Y_0 = k)p(Y_0 = k) \\ &= \hat{p}(\bx_0|Y_0 = k)\hat{\pi}_k, \end{align*} \end{split}\]

其中 \(\hat{p}\) 给出在 \(Y_0\) 条件下 \(\bx_0\) 的估计密度。然后我们预测 \(Y_0 = k\) 对于使上述表达式最大化的任何 \(k\) 值。

## 1. 模型结构

一个生成分类器模型了两种随机性来源。首先，我们假设在 \(K\) 个可能的类别中，每个观测值独立地属于类别 \(k\) 的概率为 \(\pi_k\)。换句话说，让 \(\bpi =\begin{bmatrix} \pi_1 & \dots & \pi_K\end{bmatrix}^\top \in \mathbb{R}^{K}\)，我们假设先验

\[ y_n \iid \text{Cat}(\bpi). \]

有关分类分布的数学注释见下文。

数学注释

一个取值为 \(K\) 个离散且无序结果的随机变量，其概率为 \(\pi_1, \dots, \pi_K\)，遵循参数为 \(\bpi = \begin{bmatrix} \pi_1 & \dots & \pi_K \end{bmatrix}^\top\) 的分类分布，写作 \(\text{Cat}(\bpi)\)。例如，单次掷骰子的分布为 \(\text{Cat}(\bpi)\)，其中 \(\bpi = \begin{bmatrix} 1/6 \dots 1/6 \end{bmatrix}^\top\)。

对于 \(Y \sim \text{Cat}(\bp)\) 的密度定义为

\[\begin{split} \begin{align*} P(Y = 1) &= p_1 \\ &... \\ P(Y = K) &= p_K. \end{align*} \end{split}\]

这可以更紧凑地写成

\[ p(y) = \prod_{k = 1}^K p_k ^{I_k} \]

其中 \(I_k\) 是一个指示符，当 \(y = k\) 时等于 1，否则为 0。

然后，我们假设在观察 \(n\) 的类别 \(Y_n\) 条件下，\(\mathbf{x}_n\) 服从某种分布。我们通常假设所有 \(\bx_n\) 都来自同一 *族* 的分布，尽管参数取决于它们的类别。例如，我们可能会有

\[\begin{split} \begin{align*} \bx_n|(Y_n = 1) &\sim \text{MVN}(\bmu_1, \bSigma_1), \\ &... \\ \bx_{n}|(Y_n = K) &\sim \text{MVN}(\bmu_K, \bSigma_K), \end{align*} \end{split}\]

尽管我们不会让一个条件分布是多元正态分布，而另一个是多元 \(t\) 分布。然而，随机向量 \(\bx_n\) 中的各个变量可以遵循不同的分布。例如，如果 \(\bx_n = \begin{bmatrix} x_{n1} & x_{n2} \end{bmatrix}^\top\)，我们可能会有

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \text{Bin}(n, p_k) \\ x_{n2}|(Y_n = k) &\sim \mathcal{N}(\bmu_k, \bSigma_k) \end{align*} \end{split}\]

机器学习任务是要估计这些模型的参数——\(Y_n\) 的 \(\bpi\) 以及可能索引 \(\bx_n|Y_n\) 的可能分布的任何参数，在这种情况下是 \(\bmu_k\) 和 \(\bSigma_k\) 对于 \(k = 1, \dots, K\)。一旦完成，我们就可以估计每个类别的 \(p(Y_n = k)\) 和 \(p(\bx_n|Y_n = k)\)，并通过贝叶斯定理选择最大化 \(p(Y_n = k|\bx_n)\) 的类别。

## 2\. 参数估计

### 2.1 类别先验

让我们先推导 \(\bpi\)，即类别先验的估计。令 \(I_{nk}\) 为一个指示变量，当 \(Y_n = k\) 时等于 1，否则为 0。然后，联合似然和对数似然由以下给出

\[\begin{split} \begin{align*} L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \prodN \prod_{k = 1}^K \pi_k^{I_{nk}} \\ \log L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \sumN \sum_{k = 1}^K I_{nk} \log(\pi_k) \\ &= \sum_{k = 1}^K N_k\log(\pi_k), \end{align*} \end{split}\]

其中 \(N_k = \sumN I_{nk}\) 给出了类别 \(k\) 中观察到的数量，对于 \(k = 1, \dots, K\)。

数学注释

*拉格朗日函数* 提供了一种在约束 \(g(\bx) = 0\) 下优化函数 \(f(\bx)\) 的方法。拉格朗日函数由以下给出

\[ \mathcal{L}(\lambda, \bx) = f(\bx) - \lambda g(\bx). \]

\(\lambda\) 被称为 *拉格朗日乘子*。通过将 \(\mathcal{L}(\lambda, \bx)\) 关于 \(\lambda\) 和 \(\bx\) 的梯度设为 0，可以找到 \(f(\bx)\)（在等式约束下）的临界点。

注意到约束 \(\sum_{k = 1}^K \pi_k = 1\)（或等价地 \(\sum_{k = 1}^K\pi_k - 1 = 0\)），我们可以通过以下拉格朗日乘数最大化对数似然。

\[\begin{split} \begin{align*} \mathcal{L}(\bpi) &= \sum_{k = 1}^K N_k \log(\pi_k) - \lambda(\sum_{k = 1}^K \pi_k - 1). \\ \dadb{\mathcal{L}(\bpi)}{\pi_k} &= \frac{N_k}{\pi_k} - \lambda, \hspace{3mm}\forall \hspace{1mm}k \in \{1, \dots, K\} \\ \dadb{\mathcal{L}(\bpi)}{\lambda} &= 1 - \sum_{k = 1}^K \pi_k. \\ \end{align*} \end{split}\]

这个方程组给出了一种直观的解法：

\[ \hat{\pi}_k = \frac{N_k}{N}, \hspace{1mm} \lambda = N, \]

这意味着我们估计 \(p(Y_n = k)\) 的方法就是从类 \(k\) 中观察到的样本比例。

### 2.2 数据似然

下一步是建模给定 \(Y_n\) 的 \(\bx_n\) 的条件分布，以便我们可以估计该分布的参数。这当然取决于我们选择用于建模 \(\bx_n\) 的分布族。以下详细介绍了三种常见方法。

#### 2.2.1 线性判别分析（LDA）

在线性判别分析（LDA）中，我们假设

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma), \]

对于 \(k = 1, \dots, K\)。注意，每个类具有相同的协方差矩阵，但具有唯一的均值向量。

让我们推导这个情况下的参数。首先，让我们找到似然和对数似然。注意，我们可以将联合似然写成以下形式，

\[ L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = \prodN \prodK \Big(p\l\bx_n|\bmu_k, \bSigma\r\Big)^{I_{nk}}, \]

因为 \(\left(p(\bx_{n}|\bmu_k, \bSigma)\right)^{I_{nk}}\) 在 \(y_n \neq k\) 时等于 1，而在 \(p(\bx_n|\bmu_k, \bSigma)\) 时则不等于 1。然后我们插入多元正态概率密度函数（省略乘法常数）并取对数，如下所示。

\[\begin{split} \begin{align*} L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \prodN\prodK \Big(\frac{1}{\sqrt{|\bSigma|}}\exp\left\{-\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k)\right\}\Big)^{I_{nk}} \\ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k) \r \end{align*} \end{split}\]

数学笔记

以下矩阵导数将有助于最大化上述对数似然。

对于任何可逆矩阵 \(\mathbf{W}\)，

\[ \dadb{|\mathbf{W}|}{\mathbf{W}} = |\mathbf{W}|\mathbf{W}^{-\top}, \tag{1} \]

其中 \(\mathbf{W}^{-\top} = (\mathbf{W}^{-1})^\top\). 因此，

\[ \dadb{\log |\mathbf{W}|}{\mathbf{W}} = \mathbf{W}^{-\top}. \tag{2} \]

我们还有

\[ \dadb{\bx^\top \mathbf{W}^{-1} \bx}{\mathbf{W}} = -\mathbf{W}^{-\top} \bx \bx^\top \mathbf{W}^{-\top}. \tag{3} \]

对于任何对称矩阵 \(\mathbf{A}\)，

\[ \dadb{(\bx - \mathbf{s})^\top \mathbf{A} (\bx - \mathbf{s})}{\mathbf{s}} = -2\mathbf{A}(\bx - \mathbf{s}). \tag{4} \]

这些结果来自[矩阵手册](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)。

让我们从估计 \(\bSigma\) 开始。首先，简化对数似然，以便更明显地看到相对于 \(\bSigma\) 的梯度。

\[ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = - \frac{N}{2}\log |\bSigma| -\frac{1}{2} \sumN\sumK I_{nk}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k). \]

然后，使用数学笔记中的方程（2）和（3），我们得到

\[ \begin{align*} \dadb{\log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r }{\bSigma} &= -\frac{N}{2}\bSigma^{-\top} + \frac{1}{2}\sumN\sumK I_{nk}\bSigma^{-\top} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\bSigma^{-\top}. \end{align*} \]

最后，我们将此设为 0 并在左侧乘以 \(\bSigma^{-1}\) 以求解 \(\hat{\bSigma}\):

\[\begin{split} \begin{align*} 0 &= -\frac{N}{2} + \frac{1}{2}\l\sumN\sumK I_{nk} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\r\bSigma^{-\top} \\ \bSigma^\top &= \frac{1}{N}\sumN \sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma} &= \frac{1}{N}\mathbf{S}_T, \end{align*} \end{split}\]

其中 \(\mathbf{S}_T = \sumN\sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

现在，为了估计 \(\bmu_k\)，让我们逐个查看每个类别。设 \(N_k\) 为类别 \(k\) 中的观测数，\(C_k\) 为类别 \(k\) 中的观测集合。仅查看涉及 \(\bmu_k\) 的项，我们得到

\[ \begin{align*} \log L(\bmu_k, \bSigma) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma| + (\bx_n - \bmu_k)^\top \bSigma^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

使用 *数学笔记* 中的方程 (4)，我们计算梯度如下

\[ \dadb{\log L(\bmu_k, \bSigma)}{\bmu_k} =\sum_{n \in C_k} \bSigma^{-1}(\bx_n - \bmu_k). \]

将此梯度设为 0 并求解，我们得到我们的 \(\bmu_k\) 估计值：

\[ \hat{\bmu}_k = \frac{1}{N_k} \sum_{n \in C_k} \bx_n = \bar{\bx}_k, \]

其中 \(\bar{\bx}_k\) 是类 \(k\) 中所有 \(\bx_n\) 的元素样本均值。

#### 2.2.2 二次判别分析（QDA）

QDA 与 LDA 非常相似，但假设每个类别都有自己的协方差矩阵：

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma_k) \]

对于 \(k = 1, \dots, K\)。对数似然与 LDA 相同，只是我们将 \(\bSigma\) 替换为 \(\bSigma_k\):

\[ \begin{align*} \log L\l\{\bmu_k, \bSigma_k\}_{k = 1}^K\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma_k| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma_k^{-1}(\bx_n - \bmu_k) \r. \end{align*} \]

再次，让我们逐个查看每个类别的参数。类别 \(k\) 的对数似然由以下给出

\[ \begin{align*} \log L(\bmu_k, \bSigma_k) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma_k| + (\bx_n - \bmu_k)^\top \bSigma_k^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

我们可以对此对数似然相对于 \(\bmu_k\) 求梯度，并将其设为 0 以求解 \(\hat{\bmu}_k\)。然而，我们也可以注意到，从 LDA 方法得到的 \(\hat{\bmu}_k\) 估计将保持不变，因为此表达式不依赖于协方差项（这是我们唯一改变的东西）。因此，我们再次得到

\[ \hat{\bmu}_k = \bar{\bx}_k. \]

为了估计 \(\bSigma_k\)，我们取类别 \(k\) 的对数似然的梯度。

\[ \begin{align*} \dadb{\log L(\bmu_k, \bSigma_k) }{\bSigma_k} &= -\frac{1}{2}\sum_{n \in C_k} \left( \bSigma_k^{-\top} - \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \right). \end{align*} \]

然后我们将此设为 0 以求解 \(\hat{\bSigma}_k\):

\[\begin{split} \begin{align*} \sum_{n \in C_k} \bSigma_k^{-\top} &= \sum_{n \in C_k} \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ N_k I &= \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ \bSigma_k^\top &= \frac{1}{N_k} \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma}_k &= \frac{1}{N_k} \mathbf{S}_k, \end{align*} \end{split}\]

其中 \(\mathbf{S}_k = \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

#### 2.2.3 简单贝叶斯

朴素贝叶斯假设 \(\bx_n\) 中的随机变量在观察类别 \(n\) 的条件下是独立的。即如果 \(\bx_n \in \mathbb{R}^D\)，朴素贝叶斯假设

\[ p(\bx_n|Y_n) = p(x_{n1}|Y_n)\cdot p(x_{n2}|Y_n) \cdot ... \cdot p(x_{nD}|Y_n). \]

这使得估计 \(p(\bx_n|Y_n)\) 非常容易——为了估计 \(p(x_{nd}|Y_n)\) 的参数，我们可以忽略 \(\bx_{n}\) 中除了 \(x_{nd}\) 以外的所有变量！

例如，假设 \(\bx_n \in \mathbb{R}²\) 并且我们使用以下模型（其中为了简单起见 \(n\) 和 \(\sigma²_k\) 是已知的）。

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \mathcal{N}(\mu_k, \sigma²_k) \\ x_{n2}|(Y_n = k) &\sim \text{Bin}(n, p_k). \end{align*} \end{split}\]

让 \(\btheta_k = (\mu_k, \sigma_k², p_k)\) 包含类别 \(k\) 的所有参数。联合似然函数将变为

\[\begin{split} \begin{align*} L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(\bx|Y_n, \btheta_k)\r^{I_{nk}} \\ L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(x_{n1}|\mu_k, \sigma²_k) \cdot p(x_{n2}|p_k) \r^{I_{nk}}, \end{align*} \end{split}\]

由于朴素贝叶斯条件独立性假设，这两个是相等的。这使我们能够轻松找到最大似然估计。本小节其余部分将演示如何找到这些估计，尽管这并不超出普通最大似然估计的范围。

对数似然由以下给出

\[ \log L(\{\btheta_k\}_{k = 1}^K) = \sumN\sumK I_{nk}\l\log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \r. \]

如前所述，我们通过只查看该类别中的项来估计每个类别的参数。让我们看看类别 \(k\) 的对数似然：

\[\begin{split} \begin{align*} \log L(\btheta_k) &= \sum_{n \in C_k} \log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \\ &= \sum_{n \in C_k} -\frac{(x_{n1} - \mu_k)²}{2\sigma²_k} + x_{n2}\log(p_k) + (1-x_{n2})\log(1-p_k). \end{align*} \end{split}\]

对 \(p_k\) 求导，我们得到

\[ \dadb{\log L(\btheta_k)}{p_k} = \sum_{n \in C_k}\frac{x_{n2}}{p_k} - \frac{1-x_{n2}}{1-p_k}, \]

这将给我们带来 \(\hat{p}_k = \frac{1}{N_k}\sum_{n \in C_k} x_{n2}\) 如常。同样的过程会再次为 \(\mu_k\) 和 \(\sigma²_k\) 提供典型结果。

### 2.1 类别先验

让我们先推导 \(\bpi\)，即类先验的估计。令 \(I_{nk}\) 为一个指示变量，当 \(Y_n = k\) 时等于 1，否则为 0。然后联合似然和对数似然如下给出

\[\begin{split} \begin{align*} L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \prod_{n=1}^N \prod_{k = 1}^K \pi_k^{I_{nk}} \\ \log L\left(\bpi; \{\bx_n, Y_n\}_{n = 1}^N\right) &= \sum_{n=1}^N \sum_{k = 1}^K I_{nk} \log(\pi_k) \\ &= \sum_{k = 1}^K N_k\log(\pi_k), \end{align*} \end{split}\]

其中 \(N_k = \sum_{n=1}^N I_{nk}\) 给出了类 \(k\) 的观测数，对于 \(k = 1, \dots, K\)。

数学注释

*拉格朗日函数*提供了一种在约束 \(g(\bx) = 0\) 下优化函数 \(f(\bx)\) 的方法。拉格朗日函数如下给出

\[ \mathcal{L}(\lambda, \bx) = f(\bx) - \lambda g(\bx). \]

\(\lambda\) 被称为 *拉格朗日乘子*。通过将 \(\mathcal{L}(\lambda, \bx)\) 关于 \(\lambda\) 和 \(\bx\) 的梯度设置为 0，找到 \(f(\bx)\)（在等式约束下）的临界点。

注意到约束条件 \(\sum_{k = 1}^K \pi_k = 1\)（或等价地 \(\sum_{k = 1}^K\pi_k - 1 = 0\)），我们可以通过以下拉格朗日函数最大化对数似然。

\[\begin{split} \begin{align*} \mathcal{L}(\bpi) &= \sum_{k = 1}^K N_k \log(\pi_k) - \lambda(\sum_{k = 1}^K \pi_k - 1). \\ \dadb{\mathcal{L}(\bpi)}{\pi_k} &= \frac{N_k}{\pi_k} - \lambda, \hspace{3mm}\forall \hspace{1mm}k \in \{1, \dots, K\} \\ \dadb{\mathcal{L}(\bpi)}{\lambda} &= 1 - \sum_{k = 1}^K \pi_k. \\ \end{align*} \end{split}\]

这个方程组给出了一种直观的解法：

\[ \hat{\pi}_k = \frac{N_k}{N}, \hspace{1mm} \lambda = N, \]

这意味着我们 \(p(Y_n = k)\) 的估计只是来自类 \(k\) 的观测样本的样本比例。

### 2.2 数据似然

下一步是建模给定 \(Y_n\) 的 \(\bx_n\) 的条件分布，以便我们可以估计该分布的参数。这当然取决于我们选择用于建模 \(\bx_n\) 的分布族。以下详细介绍了三种常见方法。

#### 2.2.1 线性判别分析 (LDA)

在 LDA 中，我们假设

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma), \]

对于 \(k = 1, \dots, K\)。注意，每个类都有相同的协方差矩阵，但具有唯一的均值向量。

让我们推导这种情况下的参数。首先，让我们找到似然和对数似然。注意，我们可以将联合似然写成以下形式，

\[ L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = \prod_{n=1}^N \prod_{k = 1}^K \Big(p\l\bx_n|\bmu_k, \bSigma\r\Big)^{I_{nk}}, \]

由于 \(\left(p(\bx_{n}|\bmu_k, \bSigma)\right)^{I_{nk}}\) 当 \(y_n \neq k\) 时等于 1，而当 \(p(\bx_n|\bmu_k, \bSigma)\) 时为 1。然后我们插入多元正态概率密度函数（省略乘性常数）并取对数，如下所示。

\[\begin{split} \begin{align*} L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \prodN\prodK \Big(\frac{1}{\sqrt{|\bSigma|}}\exp\left\{-\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k)\right\}\Big)^{I_{nk}} \\ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k) \r \end{align*} \end{split}\]

数学注释

以下矩阵导数将有助于最大化上述对数似然。

对于任何可逆矩阵 \(\mathbf{W}\),

\[ \dadb{|\mathbf{W}|}{\mathbf{W}} = |\mathbf{W}|\mathbf{W}^{-\top}, \tag{1} \]

其中 \(\mathbf{W}^{-\top} = (\mathbf{W}^{-1})^\top\). 因此，

\[ \dadb{\log |\mathbf{W}|}{\mathbf{W}} = \mathbf{W}^{-\top}. \tag{2} \]

我们还有

\[ \dadb{\bx^\top \mathbf{W}^{-1} \bx}{\mathbf{W}} = -\mathbf{W}^{-\top} \bx \bx^\top \mathbf{W}^{-\top}. \tag{3} \]

对于任何对称矩阵 \(\mathbf{A}\),

\[ \dadb{(\bx - \mathbf{s})^\top \mathbf{A} (\bx - \mathbf{s})}{\mathbf{s}} = -2\mathbf{A}(\bx - \mathbf{s}). \tag{4} \]

这些结果来自 [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf).

让我们先估计 \(\bSigma\)。首先，简化对数似然函数，以便更明显地看到关于 \(\bSigma\) 的梯度。

\[ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = - \frac{N}{2}\log |\bSigma| -\frac{1}{2} \sumN\sumK I_{nk}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k). \]

然后，使用 *Math Note* 中的方程 (2) 和 (3)，我们得到

\[ \begin{align*} \dadb{\log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r }{\bSigma} &= -\frac{N}{2}\bSigma^{-\top} + \frac{1}{2}\sumN\sumK I_{nk}\bSigma^{-\top} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\bSigma^{-\top}. \end{align*} \]

最后，我们将这个等式设为 0，并在左侧乘以 \(\bSigma^{-1}\) 来求解 \(\hat{\bSigma}\):

\[\begin{split} \begin{align*} 0 &= -\frac{N}{2} + \frac{1}{2}\l\sumN\sumK I_{nk} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\r\bSigma^{-\top} \\ \bSigma^\top &= \frac{1}{N}\sumN \sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma} &= \frac{1}{N}\mathbf{S}_T, \end{align*} \end{split}\]

其中 \(\mathbf{S}_T = \sumN\sumK I_{nk}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

现在，为了估计 \(\bmu_k\)，让我们逐个查看每个类别。设 \(N_k\) 为类别 \(k\) 中的观测数，\(C_k\) 为类别 \(k\) 中的观测集合。仅查看涉及 \(\bmu_k\) 的项，我们得到

\[ \begin{align*} \log L(\bmu_k, \bSigma) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma| + (\bx_n - \bmu_k)^\top \bSigma^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

使用 *Math Note* 中的方程 (4)，我们计算梯度如下

\[ \left[ \dadb{\log L(\bmu_k, \bSigma)}{\bmu_k} \right] = \sum_{n \in C_k} \bSigma^{-1}(\bx_n - \bmu_k). \]

将这个梯度设为 0 并求解，我们得到我们的 \(\bmu_k\) 估计值：

\[ \hat{\bmu}_k = \frac{1}{N_k} \sum_{n \in C_k} \bx_n = \bar{\bx}_k, \]

其中 \(\bar{\bx}_k\) 是类别 \(k\) 中所有 \(\bx_n\) 的元素平均值。

#### 2.2.2 二次判别分析 (QDA)

QDA 与 LDA 非常相似，但假设每个类别都有自己的协方差矩阵：

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma_k) \]

对于 \(k = 1, \dots, K\)。对数似然与 LDA 相同，只是将 \(\bSigma\) 替换为 \(\bSigma_k\)：

\[ \begin{align*} \log L\l\{\bmu_k, \bSigma_k\}_{k = 1}^K\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma_k| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma_k^{-1}(\bx_n - \bmu_k) \r. \end{align*} \]

再次，让我们分别查看每个类别的参数。类别 \(k\) 的对数似然由以下给出

\[ \begin{align*} \log L(\bmu_k, \bSigma_k) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma_k| + (\bx_n - \bmu_k)^\top \bSigma_k^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

我们可以对这个对数似然相对于 \(\bmu_k\) 求导，并将其设为 0 来求解 \(\hat{\bmu}_k\)。然而，我们也可以注意到，由于这个表达式没有依赖于协方差项（这是我们唯一改变的东西），因此从 LDA 方法得到的 \(\hat{\bmu}_k\) 估计将保持不变。因此，我们再次得到

\[ \hat{\bmu}_k = \bar{\bx}_k. \]

为了估计 \(\bSigma_k\)，我们取类别 \(k\) 的对数似然函数的梯度。

\[ \begin{align*} \dadb{\log L(\bmu_k, \bSigma_k) }{\bSigma_k} &= -\frac{1}{2}\sum_{n \in C_k} \left( \bSigma_k^{-\top} - \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \right). \end{align*} \]

然后我们将这个等式设为 0 来求解 \(\hat{\bSigma}_k\):

\[\begin{split} \begin{align*} \sum_{n \in C_k} \bSigma_k^{-\top} &= \sum_{n \in C_k} \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ N_k I &= \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ \bSigma_k^\top &= \frac{1}{N_k} \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma}_k &= \frac{1}{N_k} \mathbf{S}_k, \end{align*} \end{split}\]

其中 \(\mathbf{S}_k = \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

#### 2.2.3 简单贝叶斯

简单贝叶斯假设在观察 \(n\) 的类别条件下，\(\bx_n\) 中的随机变量是独立的。即如果 \(\bx_n \in \mathbb{R}^D\)，则简单贝叶斯假设

\[ p(\bx_n|Y_n) = p(x_{n1}|Y_n)\cdot p(x_{n2}|Y_n) \cdot ... \cdot p(x_{nD}|Y_n). \]

这使得估计 \(p(\bx_n|Y_n)\) 非常简单——为了估计 \(p(x_{nd}|Y_n)\) 的参数，我们可以忽略 \(\bx_{n}\) 中除了 \(x_{nd}\) 以外的所有变量！

例如，假设 \(\bx_n \in \mathbb{R}²\)，并使用以下模型（为了简单起见，\(n\) 和 \(\sigma²_k\) 是已知的）。

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \mathcal{N}(\mu_k, \sigma²_k) \\ x_{n2}|(Y_n = k) &\sim \text{Bin}(n, p_k). \end{align*} \end{split}\]

让 \(\btheta_k = (\mu_k, \sigma_k², p_k)\) 包含类 \(k\) 的所有参数。联合似然函数将变为

\[\begin{split} \begin{align*} L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(\bx|Y_n, \btheta_k)\r^{I_{nk}} \\ L(\{\btheta_k\}_{k = 1}^K) &= \prodN \prodK \l p(x_{n1}|\mu_k, \sigma²_k) \cdot p(x_{n2}|p_k) \r^{I_{nk}}, \end{align*} \end{split}\]

由于朴素贝叶斯条件独立性假设，这两个是相等的。这使我们能够轻松地找到最大似然估计。本小节的其余部分将演示如何找到这些估计，尽管这并不超出普通最大似然估计的范围。

对数似然由以下给出

\[ \log L(\{\btheta_k\}_{k = 1}^K) = \sumN\sumK I_{nk}\l\log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \r. \]

如前所述，我们通过只查看该类中的项来估计每个类的参数。让我们看看类 \(k\) 的对数似然：

\[\begin{split} \begin{align*} \log L(\btheta_k) &= \sum_{n \in C_k} \log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \\ &= \sum_{n \in C_k} -\frac{(x_{n1} - \mu_k)²}{2\sigma²_k} + x_{n2}\log(p_k) + (1-x_{n2})\log(1-p_k). \end{align*} \end{split}\]

对 \(p_k\) 求导，我们得到

\[ \dadb{\log L(\btheta_k)}{p_k} = \sum_{n \in C_k}\frac{x_{n2}}{p_k} - \frac{1-x_{n2}}{1-p_k}, \]

这将给出 \(\hat{p}_k = \frac{1}{N_k}\sum_{n \in C_k} x_{n2}\) 的结果。同样的过程会再次给出 \(\mu_k\) 和 \(\sigma²_k\) 的典型结果。

#### 2.2.1 线性判别分析 (LDA)

在 LDA 中，我们假设

\[ \bx_n|(Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma), \]

对于 \(k = 1, \dots, K\)。注意，每个类都有相同的协方差矩阵，但具有唯一的均值向量。

让我们推导这种情况下的参数。首先，让我们找到似然和对数似然。注意，我们可以将联合似然写成以下形式，

\[ L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = \prodN \prodK \Big(p\l\bx_n|\bmu_k, \bSigma\r\Big)^{I_{nk}}, \]

因为 \(\left(p(\bx_{n}|\bmu_k, \bSigma)\right)^{I_{nk}}\) 在 \(y_n \neq k\) 时等于 1，而在 \(p(\bx_n|\bmu_k, \bSigma)\) 时则不等于 1。然后我们插入多元正态概率密度函数（省略乘法常数）并取对数，如下所示。

\[\begin{split} \begin{align*} L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \prodN\prodK \Big(\frac{1}{\sqrt{|\bSigma|}}\exp\left\{-\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k)\right\}\Big)^{I_{nk}} \\ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r &= \sumN\sumK I_{nk}\l-\frac{1}{2} \log|\bSigma| -\frac{1}{2}(\bx_n - \bmu_k)^\top\bSigma^{-1}(\bx_n - \bmu_k) \r \end{align*} \end{split}\]

数学笔记

以下矩阵导数对于最大化上述对数似然是有用的。

对于任何可逆矩阵 \(\mathbf{W}\)，

\[ \dadb{|\mathbf{W}|}{\mathbf{W}} = |\mathbf{W}|\mathbf{W}^{-\top}, \tag{1} \]

其中 \(\mathbf{W}^{-\top} = (\mathbf{W}^{-1})^\top\)。由此可得

\[ \frac{\partial \log |\mathbf{W}|}{\partial \mathbf{W}} = \mathbf{W}^{-\top}. \tag{2} \]

我们还有

\[ \frac{\partial \left( \bx^\top \mathbf{W}^{-1} \bx \right)}{\partial \mathbf{W}} = -\mathbf{W}^{-\top} \bx \bx^\top \mathbf{W}^{-\top}. \tag{3} \]

对于任何对称矩阵 \(\mathbf{A}\)，

\[ \frac{\left[ \left( \bx - \mathbf{s} \right)^\top \mathbf{A} \left( \bx - \mathbf{s} \right) \right]}{\mathbf{s}} = -2\mathbf{A} \left( \bx - \mathbf{s} \right). \tag{4} \]

这些结果来自 [Matrix Cookbook](https://www.math.uwaterloo.ca/~hwolkowi/matrixcookbook.pdf)。

让我们先估计 \(\bSigma\)。首先，简化对数似然，以便更明显地看到关于 \(\bSigma\) 的梯度。

\[ \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r = - \frac{N}{2}\log |\bSigma| -\frac{1}{2} \sum_{n \in C_k} I_{nk} \left( \bx_n - \bmu_k \right)^\top \bSigma^{-1} \left( \bx_n - \bmu_k \right). \]

然后，使用来自 *Math Note* 的方程 (2) 和 (3)，我们得到

\[ \begin{align*} \frac{\partial \log L\l\{\bmu_k\}_{k = 1}^K, \bSigma\r}{\bSigma} &= -\frac{N}{2}\bSigma^{-\top} + \frac{1}{2}\sum_{n \in C_k} I_{nk}\bSigma^{-\top} \left( \bx_n - \bmu_k \right) \left( \bx_n - \bmu_k \right)^\top \bSigma^{-\top}. \end{align*} \]

最后，我们将此设为 0 并在左侧乘以 \(\bSigma^{-1}\) 来求解 \(\hat{\bSigma}\)：

\[\begin{split} \begin{align*} 0 &= -\frac{N}{2} + \frac{1}{2}\sum_{n \in C_k} I_{nk} \left( \bx_n - \bmu_k \right) \left( \bx_n - \bmu_k \right)^\top \bSigma^{-\top} \\ \bSigma^\top &= \frac{1}{N} \sum_{n \in C_k} I_{nk} \left( \bx_n - \bmu_k \right) \left( \bx_n - \bmu_k \right)^\top \\ \hat{\bSigma} &= \frac{1}{N} \mathbf{S}_T, \end{align*} \end{split}\]

其中 \(\mathbf{S}_T = \sum_{n \in C_k} I_{nk} \left( \bx_n - \bmu_k \right) \left( \bx_n - \bmu_k \right)^\top\).

现在，为了估计 \(\bmu_k\)，让我们逐个查看每个类别。设 \(N_k\) 为类别 \(k\) 中的观测数，\(C_k\) 为类别 \(k\) 中的观测集合。仅查看涉及 \(\bmu_k\) 的项，我们得到

\[ \begin{align*} \log L(\bmu_k, \bSigma) &= -\frac{1}{2} \sum_{n \in C_k} \left( \log |\bSigma| + \left( \bx_n - \bmu_k \right)^\top \bSigma^{-1} \left( \bx_n - \bmu_k \right) \right). \end{align*} \]

使用来自 *Math Note* 的方程 (4)，我们计算梯度如下

\[ \frac{\partial \log L(\bmu_k, \bSigma)}{\partial \bmu_k} = \sum_{n \in C_k} \bSigma^{-1} \left( \bx_n - \bmu_k \right). \]

将此梯度设为 0 并求解，我们得到 \(\bmu_k\) 的估计值：

\[ \hat{\bmu}_k = \frac{1}{N_k} \sum_{n \in C_k} \bx_n = \bar{\bx}_k, \]

其中 \(\bar{\bx}_k\) 是类别 \(k\) 中所有 \(\bx_n\) 的逐元素样本均值。

#### 2.2.2 二次判别分析 (QDA)

QDA 与 LDA 非常相似，但假设每个类别都有自己的协方差矩阵：

\[ \bx_n \mid (Y_n = k) \sim \text{MVN}(\bmu_k, \bSigma_k) \]

对于 \(k = 1, \dots, K\)。对数似然与 LDA 相同，只是我们将 \(\bSigma\) 替换为 \(\bSigma_k\)：

\[ \begin{align*} \log L\l\{\bmu_k, \bSigma_k\}_{k = 1}^K\r &= \sum_{n \in C_k} I_{nk} \left( -\frac{1}{2} \log |\bSigma_k| -\frac{1}{2} \left( \bx_n - \bmu_k \right)^\top \bSigma_k^{-1} \left( \bx_n - \bmu_k \right) \right). \end{align*} \]

再次，让我们逐个查看每个类别的参数。类别 \(k\) 的对数似然由以下给出

\[ \begin{align*} \log L(\bmu_k, \bSigma_k) &= -\frac{1}{2} \sum_{n \in C_k} \Big( \log|\bSigma_k| + (\bx_n - \bmu_k)^\top \bSigma_k^{-1}(\bx_n - \bmu_k) \Big). \end{align*} \]

我们可以对这个对数似然关于 \(\bmu_k\) 求梯度，并将其设为 0 来求解 \(\hat{\bmu}_k\)。然而，我们也可以注意到，由于这个表达式不依赖于协方差项（这是我们唯一改变的东西），我们使用 LDA 方法得到的 \(\hat{\bmu}_k\) 估计将保持不变。因此，我们再次得到

\[ \hat{\bmu}_k = \bar{\bx}_k. \]

为了估计 \(\bSigma_k\)，我们取类 \(k\) 的对数似然的梯度。

\[ \begin{align*} \frac{\partial \log L(\bmu_k, \bSigma_k)}{\partial \bSigma_k} &= -\frac{1}{2}\sum_{n \in C_k} \left( \bSigma_k^{-\top} - \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \right). \end{align*} \]

然后我们将这个值设为 0 来求解 \(\hat{\bSigma}_k\)：

\[\begin{split} \begin{align*} \sum_{n \in C_k} \bSigma_k^{-\top} &= \sum_{n \in C_k} \bSigma_k^{-\top}(\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ N_k I &= \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \bSigma_k^{-\top} \\ \bSigma_k^\top &= \frac{1}{N_k} \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top \\ \hat{\bSigma}_k &= \frac{1}{N_k} \mathbf{S}_k, \end{align*} \end{split}\]

其中 \(\mathbf{S}_k = \sum_{n \in C_k} (\bx_n - \bmu_k)(\bx_n - \bmu_k)^\top\).

#### 2.2.3 简单贝叶斯

简单贝叶斯假设 \(\bx_n\) 中的随机变量在观察类 \(n\) 的条件下是独立的。即如果 \(\bx_n \in \mathbb{R}^D\)，简单贝叶斯假设

\[ p(\bx_n|Y_n) = p(x_{n1}|Y_n) \cdot p(x_{n2}|Y_n) \cdot ... \cdot p(x_{nD}|Y_n). \]

这使得估计 \(p(\bx_n|Y_n)\) 非常容易——为了估计 \(p(x_{nd}|Y_n)\) 的参数，我们可以忽略 \(\bx_n\) 中除了 \(x_{nd}\) 以外的所有变量！

例如，假设 \(\bx_n \in \mathbb{R}²\) 并且我们使用以下模型（为了简单起见，\(n\) 和 \(\sigma²_k\) 是已知的）。

\[\begin{split} \begin{align*} x_{n1}|(Y_n = k) &\sim \mathcal{N}(\mu_k, \sigma²_k) \\ x_{n2}|(Y_n = k) &\sim \text{Bin}(n, p_k). \end{align*} \end{split}\]

让 \(\btheta_k = (\mu_k, \sigma_k², p_k)\) 包含类 \(k\) 的所有参数。联合似然函数将变为

\[\begin{split} \begin{align*} L(\{\btheta_k\}_{k = 1}^K) &= \prod_{n=1}^{N} \prod_{k=1}^{K} \left( p(\bx|Y_n, \btheta_k) \right)^{I_{nk}} \\ L(\{\btheta_k\}_{k = 1}^K) &= \prod_{n=1}^{N} \prod_{k=1}^{K} \left( p(x_{n1}|\mu_k, \sigma²_k) \cdot p(x_{n2}|p_k) \right)^{I_{nk}}, \end{align*} \end{split}\]

由于简单贝叶斯条件独立性假设，这两个是相等的。这使我们能够轻松找到最大似然估计。本小节的其余部分将演示如何找到这些估计，尽管这并不超出普通最大似然估计的范围。

对数似然由以下给出

\[ \log L(\{\btheta_k\}_{k = 1}^K) = \sum_{n=1}^{N}\sum_{k=1}^{K} I_{nk}\log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \]

如前所述，我们通过只观察该类中的项来估计每个类别的参数。让我们看看类别 \(k\) 的对数似然：

\[\begin{split} \begin{align*} \log L(\btheta_k) &= \sum_{n \in C_k} \log p(x_{n1}|\mu_k, \sigma²_k) + \log p(x_{n2}|p_k) \\ &= \sum_{n \in C_k} -\frac{(x_{n1} - \mu_k)²}{2\sigma²_k} + x_{n2}\log(p_k) + (1-x_{n2})\log(1-p_k). \end{align*} \end{split}\]

对 \(p_k\) 求导，我们得到

\[ \dadb{\log L(\btheta_k)}{p_k} = \sum_{n \in C_k}\frac{x_{n2}}{p_k} - \frac{1-x_{n2}}{1-p_k}, \]

这将给出 \(\hat{p}_k = \frac{1}{N_k}\sum_{n \in C_k} x_{n2}\) 作为通常的结果。同样的过程会再次给出 \(\mu_k\) 和 \(\sigma²_k\) 的典型结果。

## 3. 进行分类

无论我们对 \(p(\bx_n|Y_n)\) 的建模选择如何，对新观测的分类都是容易的。考虑一个测试观测 \(\bx_0\)。对于 \(k = 1, \dots, K\)，我们使用贝叶斯定理来计算

\[\begin{split} \begin{align*} p(Y_0 = k|\bx_0) &\propto p(\bx_0|Y_0 = k)p(Y_0 = k) \\ &= \hat{p}(\bx_0|Y_0 = k)\hat{\pi}_k, \end{align*} \end{split}\]

其中 \(\hat{p}\) 给出了在 \(Y_0\) 条件下 \(\bx_0\) 的估计密度。然后我们预测 \(Y_0 = k\)，对于使上述表达式最大化的 \(k\) 值。
