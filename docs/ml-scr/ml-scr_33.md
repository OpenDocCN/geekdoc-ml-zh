# 数学

> 原文：[`dafriedman97.github.io/mlbook/content/appendix/math.html`](https://dafriedman97.github.io/mlbook/content/appendix/math.html)

$$ \newcommand{\sumN}{\sum_{n = 1}^N} \newcommand{\sumn}{\sum_n} \newcommand{\prodN}{\prod_{n = 1}^N} \newcommand{\by}{\mathbf{y}} \newcommand{\bX}{\mathbf{X}} \newcommand{\bx}{\mathbf{x}} \newcommand{\bu}{\mathbf{u}} \newcommand{\bv}{\mathbf{v}} \newcommand{\bbeta}{\boldsymbol{\beta}} \newcommand{\btheta}{\boldsymbol{\theta}} \newcommand{\bbetahat}{\boldsymbol{\hat{\beta}}} \newcommand{\bthetahat}{\boldsymbol{\hat{\theta}}} \newcommand{\bSigma}{\boldsymbol{\Sigma}} \newcommand{\bphi}{\boldsymbol{\phi}} \newcommand{\bPhi}{\boldsymbol{\Phi}} \newcommand{\bT}{\mathbf{T}} \newcommand{\dadb}[2]{\frac{\partial #1}{\partial #2}} \newcommand{\iid}{\overset{\small{\text{i.i.d.}}}{\sim}} $$

对于一本关于数学推导的书，本文假设对相对较少的数学方法有所了解。所需的数学背景主要总结在以下三个部分：微积分、矩阵和矩阵微积分。

## 微积分

本书最重要的数学先决条件是微积分。几乎所有的方法都涉及最小化损失函数或最大化似然函数，这通过将函数对一个或多个参数的导数设为 0 来实现。

让我们从回顾本书中常用的一些导数开始：

$$\begin{split} \begin{align*} f(x) &= x^a \rightarrow f'(x) = ax^{a-1} \\ f(x) &= \exp(x) \rightarrow f'(x) = \exp(x) \\ f(x) &= \log(x) \rightarrow f'(x) = \frac{1}{x} \\ f(x) &= |x| \rightarrow f'(x) = \begin{cases} 1, & x > 0 \\ -1, & x < 0,\end{cases} \\ \end{align*} \end{split}$$

我们还将经常使用求和、乘积和商的法则：

$$\begin{split} \begin{align*} f(x) &= g(x) + h(x) \rightarrow f'(x) = g'(x) + h'(x) \\ f(x) &= g(x)\cdot h(x) \rightarrow f'(x)= g'(x)h(x) + g(x)h'(x)\\ f(x) &= g(x)/h(x) \rightarrow f'(x) = \frac{h(x)g'(x) - g(x)h'(x)}{h(x)²}. \end{align*} \end{split}$$

最后，我们将主要依赖链式法则：

$$ f(x) = g(h(x)) \rightarrow f'(x) = g'(h(x))h'(x). $$

作为链式法则的一个例子，假设 $f(x) = \log(x²)$。设 $h(x) = x²$，意味着 $f(x) = \log(h(x))$。那么

$$ f'(x) = \frac{1}{h(x)}h'(x) = \frac{1}{x²}\cdot2x = \frac{2}{x}. $$

## 矩阵

虽然这本书中很少使用线性代数，但数据的矩阵和向量表示非常常见。以下简要回顾了最重要的矩阵和向量运算。

设 $\mathbf{u}$ 和 $\mathbf{v}$ 是长度为 $D$ 的两个列向量。$\mathbf{u}$ 和 $\mathbf{v}$ 的**点积**是一个由以下公式给出的标量值

$$ \mathbf{u} \cdot \mathbf{v} = \mathbf{u}^\top \mathbf{v} = \sum_{d = 1}^D u_d v_d = u_1v_1 + u_2v_2 + \dots + u_Dv_D. $$

如果 $\bv$ 是一个特征向量（为截距项添加一个前导 1），$\bu$ 是一个权重向量，这个点积也被称为 $\bv$ 中预测因子的*线性组合*。

**L1 范数**和**L2 范数**衡量向量的幅度。对于一个向量 $\bu$，这些分别由

$$\begin{split} \begin{align} ||\bu||_1 &= \sum_{d = 1}^D |u_d| \\ ||\bu||_2 &= \sqrt{\sum_{d = 1}^D u_d²}. \\ \end{align} \end{split}$$

设 $\mathbf{A}$ 为一个定义为 $(N \times D)$ 的矩阵

$$\begin{split} \mathbf{A} = \begin{pmatrix} A_{11} & A_{12} & ... & A_{1D} \\ A_{21} & A_{22} & ... & A_{2D} \\ & & ... & \\ A_{N1} & A_{N2} & ... & A_{ND} \end{pmatrix} \end{split}$$

$\mathbf{A}$ 的转置是一个 $(D \times N)$ 矩阵，表示为

$$\begin{split} \mathbf{A}^T = \begin{pmatrix} A_{11} & A_{21} & ... & A_{N1} \\ A_{12} & A_{22} & ... & A_{N2} \\ & & ... & \\ A_{1D} & A_{2D} & ... & A_{ND} \end{pmatrix} \end{split}$$

如果 $\mathbf{A}$ 是一个方阵 $(N \times N)$，其逆矩阵 $\mathbf{A}^{-1}$ 是一个矩阵，使得

$$ \mathbf{A}\mathbf{A}^{-1} = \mathbf{A}^{-1}\mathbf{A} = I_N. $$

## 矩阵微积分

处理多个参数、多个观测值以及有时多个损失函数，在这本书中我们经常需要同时求多个导数。这是通过矩阵微积分完成的。

在这本书中，我们将使用矩阵导数的[分子布局](https://en.wikipedia.org/wiki/Matrix_calculus#Numerator-layout_notation)约定。这可以通过例子最简单地展示。首先，设 $a$ 是一个标量，$\mathbf{u}$ 是一个长度为 $I$ 的向量。$a$ 对 $\mathbf{u}$ 的导数表示为

$$ \dadb{a}{\mathbf{u}} = \begin{pmatrix} \dadb{a}{u_1} & .. & \dadb{a}{u_I}\end{pmatrix} \in \R^{I}, $$

以及 $\bu$ 对 $a$ 的导数表示为

$$\begin{split} \dadb{\bu}{a} = \begin{pmatrix} \dadb{u_1}{a} \\ ... \\ \dadb{u_I}{a} \end{pmatrix} \R^I. \end{split}$$

注意，在两种情况下，导数的第一个维度由分子中的内容决定。同样，设 $\bv$ 是一个长度为 $J$ 的向量，$\bu$ 对 $\bv$ 的导数表示为

$$\begin{split} \dadb{\bu}{\bv} = \begin{pmatrix} \dadb{u_1}{v_1} & ... & \dadb{u_1}{v_J} \\ & ... & \\ \dadb{u_I}{v_1} & ... & \dadb{u_I}{v_J}\end{pmatrix} \in \R^{I \times J}. \end{split}$$

我们还必须对矩阵求导或相对于矩阵求导。设 $\bX$ 为一个 $(N \times D)$ 的矩阵。$\bX$ 对常数 $a$ 的导数表示为

$$\begin{split} \dadb{\bX}{a} = \begin{pmatrix} \dadb{X_{11}}{a} & ... & \dadb{X_{1D}}{a} \\ & ... & \\ \dadb{X_{N1}}{a} & ... & \dadb{X_{ND}}{a}\end{pmatrix} \in \R^{N \times D}, \end{split}$$

相反，$a$ 对 $\bX$ 的导数表示为

$$\begin{split} \dadb{a}{\bX} = \begin{pmatrix} \dadb{a}{X_{11}} & ... & \dadb{a}{X_{1D}} \\ & ... & \\ \dadb{a}{X_{N1}} & ... & \dadb{a}{X_{ND}} \end{pmatrix} \in \R^{N \times D}. \end{split}$$

最后，我们偶尔需要求向量相对于矩阵或反之的导数。这会导致一个 3 维或更高维的**张量**。下面给出了两个例子。首先，$\bu \in \R^I$相对于$\bX \in \R^{N \times D}$的导数由以下公式给出

$$\begin{split} \dadb{\bu}{\bX} = \begin{pmatrix} \begin{pmatrix} \dadb{u_1}{X_{11}} & ... & \dadb{u_1}{X_{1D}} \\ & ... & \\ \dadb{u_1}{X_{N1}} & ... & \dadb{u_1}{X_{ND}} \end{pmatrix} & ... & \begin{pmatrix} \dadb{u_I}{X_{11}} & ... & \dadb{u_I}{X_{1D}} \\ & ... & \\ \dadb{u_I}{X_{N1}} & ... & \dadb{u_I}{X_{ND}} \end{pmatrix} \end{pmatrix} \in \R^{I \times N \times D}, \end{split}$$

并且，$\bX$相对于$\bu$的导数由以下公式给出

$$\begin{split} \dadb{\bX}{\bu} = \begin{pmatrix} \begin{pmatrix} \dadb{X_{11}}{u_1} & ... & \dadb{X_{11}}{u_I} \end{pmatrix} & ... & \begin{pmatrix} \dadb{X_{1D}}{u_1} & ... & \dadb{X_{1D}}{u_I}\end{pmatrix} \\ & ... & \\ \begin{pmatrix}\dadb{X_{N1}}{u_1} & ... & \dadb{X_{N1}}{u_I} \end{pmatrix} & ... & \begin{pmatrix}\dadb{X_{ND}}{u_1} & ... & \dadb{X_{ND}}{u_I}\end{pmatrix} \\ \end{pmatrix} \in \R^{N \times D \times I}. \end{split}$$

再次注意，我们所求的导数的**被导数**决定了导数的第一维数，而我们求导数的**导数**决定了导数的最后一维数。

## 微积分

本书最重要的数学先决条件是微积分。几乎所有的方法都涉及到最小化损失函数或最大化似然函数，这通常是通过求函数对一个或多个参数的导数并将其设置为 0 来实现的。

让我们先回顾一下本书中常用的一些导数：

$$\begin{split} \begin{align*} f(x) &= x^a \rightarrow f'(x) = ax^{a-1} \\ f(x) &= \exp(x) \rightarrow f'(x) = \exp(x) \\ f(x) &= \log(x) \rightarrow f'(x) = \frac{1}{x} \\ f(x) &= |x| \rightarrow f'(x) = \begin{cases} 1, & x > 0 \\ -1, & x < 0,\end{cases} \\ \end{align*} \end{split}$$

我们还将经常使用求和、乘积和商的法则：

$$\begin{split} \begin{align*} f(x) &= g(x) + h(x) \rightarrow f'(x) = g'(x) + h'(x) \\ f(x) &= g(x)\cdot h(x) \rightarrow f'(x)= g'(x)h(x) + g(x)h'(x)\\ f(x) &= g(x)/h(x) \rightarrow f'(x) = \frac{h(x)g'(x) - g(x)h'(x)}{h(x)²}. \end{align*} \end{split}$$

最后，我们将大量依赖链式法则：

$$ f(x) = g(h(x)) \rightarrow f'(x) = g'(h(x))h'(x). $$

作为链式法则的一个例子，假设 $f(x) = \log(x²)$。令 $h(x) = x²$，意味着 $f(x) = \log(h(x))$。那么

$$ f'(x) = \frac{1}{h(x)}h'(x) = \frac{1}{x²}\cdot2x = \frac{2}{x}. $$

## 矩阵

虽然在这本书中很少使用线性代数，但数据的矩阵和向量表示非常常见。下面回顾了最重要的矩阵和向量运算。

设 $\mathbf{u}$ 和 $\mathbf{v}$ 是长度为 $D$ 的两个列向量。$\mathbf{u}$ 和 $\mathbf{v}$ 的**点积**是一个标量值，表示为

$$ \mathbf{u} \cdot \mathbf{v} = \mathbf{u}^\top \mathbf{v} = \sum_{d = 1}^D u_d v_d = u_1v_1 + u_2v_2 + \dots + u_Dv_D. $$

如果 $\bv$ 是一个特征向量（为截距项添加一个前导 1），而 $\bu$ 是一个权重向量，这个点积也被称为 $\bv$ 中预测因子的**线性组合**。

**L1 范数**和**L2 范数**衡量向量的幅度。对于一个向量 $\bu$，这些分别表示为

$$\begin{split} \begin{align} ||\bu||_1 &= \sum_{d = 1}^D |u_d| \\ ||\bu||_2 &= \sqrt{\sum_{d = 1}^D u_d²}. \\ \end{align} \end{split}$$

设 $\mathbf{A}$ 为一个 $(N \times D)$ 矩阵，定义为

$$\begin{split} \mathbf{A} = \begin{pmatrix} A_{11} & A_{12} & ... & A_{1D} \\ A_{21} & A_{22} & ... & A_{2D} \\ & & ... & \\ A_{N1} & A_{N2} & ... & A_{ND} \end{pmatrix} \end{split}$$

$\mathbf{A}$ 的转置是一个 $(D \times N)$ 矩阵，表示为

$$\begin{split} \mathbf{A}^T = \begin{pmatrix} A_{11} & A_{21} & ... & A_{N1} \\ A_{12} & A_{22} & ... & A_{N2} \\ & & ... & \\ A_{1D} & A_{2D} & ... & A_{ND} \end{pmatrix} \end{split}$$

如果 $\mathbf{A}$ 是一个平方 $(N \times N)$ 矩阵，其逆矩阵 $\mathbf{A}^{-1}$ 是一个矩阵，使得

$$ \mathbf{A}\mathbf{A}^{-1} = \mathbf{A}^{-1}\mathbf{A} = I_N. $$

## 矩阵微积分

处理多个参数、多个观测值以及有时多个损失函数时，在这本书中我们经常需要同时计算多个导数。这是通过矩阵微积分完成的。

在这本书中，我们将使用 [分子布局](https://en.wikipedia.org/wiki/Matrix_calculus#Numerator-layout_notation) 的约定来表示矩阵导数。这可以通过例子最简单地展示。首先，设 $a$ 是一个标量，$\mathbf{u}$ 是一个长度为 $I$ 的向量。$a$ 关于 $\bu$ 的导数由以下给出

$$ \dadb{a}{\mathbf{u}} = \begin{pmatrix} \dadb{a}{u_1} & .. & \dadb{a}{u_I}\end{pmatrix} \in \R^{I}, $$

$\bu$ 关于 $a$ 的导数由以下给出

$$\begin{split} \dadb{\bu}{a} = \begin{pmatrix} \dadb{u_1}{a} \\ ... \\ \dadb{u_I}{a} \end{pmatrix} \R^I. \end{split}$$

注意，在两种情况下，导数的第一维由分子中的内容决定。同样，设 $\bv$ 是一个长度为 $J$ 的向量，$\bu$ 关于 $\bv$ 的导数用以下表示

$$\begin{split} \dadb{\bu}{\bv} = \begin{pmatrix} \dadb{u_1}{v_1} & ... & \dadb{u_1}{v_J} \\ & ... & \\ \dadb{u_I}{v_1} & ... & \dadb{u_I}{v_J}\end{pmatrix} \in \R^{I \times J}. \end{split}$$

我们还必须对矩阵进行求导或相对于矩阵求导。设 $\bX$ 为一个 $(N \times D)$ 矩阵。$\bX$ 对常数 $a$ 的导数由以下公式给出

$$\begin{split} \dadb{\bX}{a} = \begin{pmatrix} \dadb{X_{11}}{a} & ... & \dadb{X_{1D}}{a} \\ & ... & \\ \dadb{X_{N1}}{a} & ... & \dadb{X_{ND}}{a}\end{pmatrix} \in \R^{N \times D}, \end{split}$$

相反，$a$ 对 $\bX$ 的导数由以下公式给出

$$\begin{split} \dadb{a}{\bX} = \begin{pmatrix} \dadb{a}{X_{11}} & ... & \dadb{a}{X_{1D}} \\ & ... & \\ \dadb{a}{X_{N1}} & ... & \dadb{a}{X_{ND}} \end{pmatrix} \in \R^{N \times D}. \end{split}$$

最后，我们有时需要求向量和矩阵之间的导数，或者反之。这会导致一个 3 维或更多维的**张量**。以下给出两个例子。首先，$\bu \in \R^I$ 对 $\bX \in \R^{N \times D}$ 的导数由以下公式给出

$$\begin{split} \dadb{\bu}{\bX} = \begin{pmatrix} \begin{pmatrix} \dadb{u_1}{X_{11}} & ... & \dadb{u_1}{X_{1D}} \\ & ... & \\ \dadb{u_1}{X_{N1}} & ... & \dadb{u_1}{X_{ND}} \end{pmatrix} & ... & \begin{pmatrix} \dadb{u_I}{X_{11}} & ... & \dadb{u_I}{X_{1D}} \\ & ... & \\ \dadb{u_I}{X_{N1}} & ... & \dadb{u_I}{X_{ND}} \end{pmatrix} \end{pmatrix} \in \R^{I \times N \times D}, \end{split}$$

$\bX$ 对 $\bu$ 的导数由以下公式给出

$$\begin{split} \dadb{\bX}{\bu} = \begin{pmatrix} \begin{pmatrix} \dadb{X_{11}}{u_1} & ... & \dadb{X_{11}}{u_I} \end{pmatrix} & ... & \begin{pmatrix} \dadb{X_{1D}}{u_1} & ... & \dadb{X_{1D}}{u_I}\end{pmatrix} \\ & ... & \\ \begin{pmatrix}\dadb{X_{N1}}{u_1} & ... & \dadb{X_{N1}}{u_I} \end{pmatrix} & ... & \begin{pmatrix}\dadb{X_{ND}}{u_1} & ... & \dadb{X_{ND}}{u_I}\end{pmatrix} \\ \end{pmatrix} \in \R^{N \times D \times I}. \end{split}$$

再次注意，我们所求的导数的**被导数**决定了导数的第一维（或几维），而我们求导的**相对于**决定了导数的最后一维。
