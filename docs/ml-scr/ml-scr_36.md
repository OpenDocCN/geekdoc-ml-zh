# 数据集

> 原文：[`dafriedman97.github.io/mlbook/content/appendix/data.html`](https://dafriedman97.github.io/mlbook/content/appendix/data.html)

本书中的示例使用了几个可通过 `scikit-learn` 或 `seaborn` 获取的数据集。以下简要描述这些数据集。

## 波士顿住房数据集

[波士顿住房数据集](https://www.kaggle.com/c/boston-housing) 包含了马萨诸塞州波士顿 506 个社区的信息。目标变量是业主自住房屋的中位数价值（似乎在 50,000 美元处被截断）。这个变量大约是连续的，因此我们将使用这个数据集来进行回归任务。预测变量全部为数值数据，包括种族人口统计和犯罪率等详细信息。它可通过 `sklearn.datasets` 获取。

## 乳腺癌数据集

[乳腺癌数据集](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+%28Diagnostic%29) 包含了 569 名乳腺癌患者的细胞测量数据。目标变量是癌症是否为恶性或良性，因此我们将用它来进行二元分类任务。预测变量全部为定量数据，包括测量细胞的周长或凹凸度等信息。它可通过 `sklearn.datasets` 获取。

## 企鹅数据集

[企鹅数据集](https://www.kaggle.com/parulpandey/penguin-dataset-the-new-iris) 包含了来自三种不同物种的 344 只企鹅的测量数据：*阿德利企鹅*、*金图企鹅* 和 *帝企鹅*。目标变量是企鹅的物种。预测变量既包括定量数据也包括分类数据，包括企鹅鳍的大小以及它被发现的小岛等信息。由于这个数据集包含分类预测变量，我们将用它来进行基于树的模型（尽管可以通过创建虚拟变量将其用于定量模型）。它可通过 `seaborn.load_dataset()` 获取。

## 小费数据集

[小费数据集](https://www.kaggle.com/ranjeetjain3/seaborn-tips-dataset) 包含了 1990 年一位餐饮服务员 244 次观察结果。目标变量是服务员每次用餐收到的小费金额（以美元计）。预测变量既包括定量数据也包括分类数据：总账单、宴会规模、星期几等。由于数据集包含分类预测变量和定量目标变量，我们将用它来进行基于树的回归任务。它可通过 `seaborn.load_dataset()` 获取。

## 葡萄酒数据集

[葡萄酒数据集](https://archive.ics.uci.edu/ml/datasets/wine) 包含了 178 种葡萄酒的化学分析数据。目标变量是葡萄酒类别，因此我们将用它来进行分类任务。预测变量全部为数值数据，详细描述了每款葡萄酒的化学成分。它可通过 `sklearn.datasets` 获取。

## 波士顿住房数据集

[波士顿住房数据集](https://www.kaggle.com/c/boston-housing) 包含了马萨诸塞州波士顿 506 个社区的信息。目标变量是业主自住房屋的中位数价值（似乎在 50,000 美元处被截断）。这个变量大约是连续的，因此我们将使用这个数据集来进行回归任务。预测变量全部是数值型，包括种族人口统计和犯罪率等详细信息。它可以通过 `sklearn.datasets` 获取。

## 乳腺癌

[乳腺癌数据集](https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+%28Diagnostic%29) 包含了 569 名乳腺癌患者的细胞测量数据。目标变量是癌症是否为恶性或良性，因此我们将用它来进行二元分类任务。预测变量全部是定量数据，包括测量细胞的周长或凹凸度等信息。它可以通过 `sklearn.datasets` 获取。

## 企鹅

[企鹅数据集](https://www.kaggle.com/parulpandey/penguin-dataset-the-new-iris) 包含了来自三种不同物种的 344 只企鹅的测量数据：*阿德利企鹅*、*金图企鹅*和*帝企鹅*。目标变量是企鹅的物种。预测变量既包括定量数据也包括分类数据，包括企鹅鳍的大小以及它被发现的小岛等信息。由于这个数据集包含分类预测变量，我们将用它来进行基于树的模型（尽管可以通过创建虚拟变量将其用于定量模型）。它可以通过 `seaborn.load_dataset()` 获取。

## 小费

[小费数据集](https://www.kaggle.com/ranjeetjain3/seaborn-tips-dataset) 包含了 1990 年一位餐饮服务员 244 个观察结果。目标变量是服务员每餐收到的美元小费金额。预测变量既包括定量数据也包括分类数据：总账单、宴会规模、星期几等。由于数据集包含分类预测变量和定量目标变量，我们将用它来进行基于树的回归任务。它可以通过 `seaborn.load_dataset()` 获取。

## 葡萄酒

[葡萄酒数据集](https://archive.ics.uci.edu/ml/datasets/wine) 包含了 178 种葡萄酒的化学分析数据。目标变量是葡萄酒类别，因此我们将用它来进行分类任务。预测变量全部是数值型，详细描述了每款葡萄酒的化学成分。它可以通过 `sklearn.datasets` 获取。
