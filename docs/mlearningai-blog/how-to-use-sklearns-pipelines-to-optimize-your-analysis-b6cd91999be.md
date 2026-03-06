# 如何使用 Sklearn 的管道优化您的分析

> 原文：<https://medium.com/mlearning-ai/how-to-use-sklearns-pipelines-to-optimize-your-analysis-b6cd91999be?source=collection_archive---------3----------------------->

![](img/4f428f7b0f0f781897527fb0f225bf3c.png)

Photo by [Mike Benna](https://unsplash.com/@mbenna?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在数据科学和机器学习中，**管道是一组连续的步骤，允许我们控制数据流**。它们非常有用，因为它们使我们的代码更干净、更具可伸缩性和可读性。它们用于组织项目的各个阶段，例如预处理、训练模型等等。事实上，通过管道，我们可以将所有这些操作压缩到一个对象中，从而使我们的代码简洁明了。

实现管道不是强制性的，但有显著的优势，例如

*   **更干净的代码**:我们以更有条理的方式编写更少的代码。这促进了结果的可读性和解释
*   **减少出错的空间**——通过编写有组织的代码，我们减少了在“自由”环境中经常发生的人为错误
*   使用 Scikit-Learn，**管道的使用类似于带有。适合()。**

下面是一个如何将管道用于合成 Scikit-Learn 数据集的示例。文档可以在[这里](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)找到。

```
**from** **sklearn.svm** **import** SVC
**from** **sklearn.preprocessing** **import** StandardScaler
**from** **sklearn.datasets** **import** make_classification
**from** **sklearn.model_selection** **import** train_test_split
**from** **sklearn.pipeline** **import** Pipeline# create dataset
X, y = make_classification(random_state=**0**)# create train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=**0**)# create a Pipeline object that scales the data and feeds it to a Support Vector Machine
pipe = Pipeline([('scaler', StandardScaler()), ('svc', SVC())])

# an Sklearn Pipeline is used as any other estimator - run everything with .fit()
pipe.fit(X_train, y_train)
Pipeline(steps=[('scaler', StandardScaler()), ('svc', SVC())])
pipe.score(X_test, y_test)

>>> **0.88**
```

这里我们通过 Scikit-Learn 的 *make_classification* 创建我们的 *X* 特性集和 *y* 目标。然后我们使用 *train_test_split* 将 X 和 y 分成训练集和测试集。

然后，我们将在管道中放置**两个步骤:第一个是处理数据标准化的步骤(定标器)，另一个是 SVC 模型的应用(支持向量分类器)。**

您可以在 Pipeline 对象的列表中看到这两个步骤。最后，就像我们通过*训练任何模型一样训练管道。*fit(X _ train，y_train)。流水线将负责在各个步骤中“传递”X_train 和 y_train，并通过*返回能够进行预测的模型。预测()。*

# 实践示例+模板

以下是在具有回归任务的机器学习项目中使用 Pipeline 的模板。

```
**import** **pandas** **as** **pd**
**from** **sklearn.model_selection** **import** train_test_split
**from** **sklearn.compose** **import** ColumnTransformer
**from** **sklearn.impute** **import** SimpleImputer
**from** **sklearn.preprocessing** **import** OneHotEncoder
**from** **xgboost** **import** XGBRegressor

# bring in our dataset
data = pd.read_csv("./my_dataset.csv")

# split features and target
y = data.Price
X = data.drop("Price", axis=**1**)

# create train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=**0.75**)

# isolate categorical variables (if present)
categorical_columns = [col **for** col **in** X_train.columns **if** X_train[col].dtype == "object"]

# isolate numerical variables (if present)
numerical_columns = [col **for** col **in** X_train.columns **if** X_train[col].dtype \
    **in** ["int64", "float64"]]

# define preprocessing steps
# 1) Manage missing values in numerical columns
# 2) Manage missing values and apply one-hot encoding in categorical columns
# We'll use Sklearn's ColumnTransformer to group the objects that will apply transformations to our columns

# preprocessing for numerical data 
# only one step in this case - imputation for missing values
numerical_transformer = SimpleImputer()

# preprocessing for categorical data
# two steps: missing values imputation, one-hot encoding
categorical_transformer = Pipeline(steps=[
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("ohe", OneHotEncoder(handle_unknown="ignore"))
])

# bundle everything in ColumnTransformer
preprocessor = ColumnTransformer(
    transformers=[
        ("numerical", numerical_transformer, numerical_columns),
        ("categorical", categorical_transformer, categorical_columns),
    ]
)

# preprocessor completed - now initialize a regression model with XGB
regressor = XGBRegressor()
```

让我们花点时间来分析一下 *ColumnTransformer* 。非常简单:这个 Sklearn 对象允许我们对 dataframe 或 Numpy 数组中的列进行转换。在上面的代码中，我们已经将转换器(比如 One-Hot 编码器和 Simple Imputer)应用于数字和分类列。然后，作为其中一个步骤，ColumnTransformer 被直接传递到管道中。如果你想了解更多关于它是如何工作的，请参考这里的文档。

我们以一种清晰和可解释的方式创建了预处理管道。现在让我们创建最终的管道，它包含预处理和 XGBRegressor 模型。

```
final_pipeline = Pipeline(steps=[
    ("preprocessor", preprocessor),
    ("xgb", regressor)
])

# pass training data to the pipeline
final_pipeline.fit(X_train, y_train)

# create predictions
preds = final_pipeline.predict(X_test)

# evaluate the model
**from** **sklearn.metrics** **import** mean_squared_error

# we use RMSE (Root Mean Squared Error) using mean_squared_error with argument squared=False
eval_metric = mean_squared_error(y_test, preds, squared=False)
**print**(eval_metric)
```

我们现在有了一个使用 Scikit-Learn 的管道结构的复制粘贴模板。