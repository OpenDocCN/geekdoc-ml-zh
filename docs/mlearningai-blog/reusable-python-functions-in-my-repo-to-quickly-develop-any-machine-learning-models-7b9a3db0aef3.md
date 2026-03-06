# 我的 repo 中的可重用 Python 函数可以快速开发任何机器学习模型

> 原文：<https://medium.com/mlearning-ai/reusable-python-functions-in-my-repo-to-quickly-develop-any-machine-learning-models-7b9a3db0aef3?source=collection_archive---------4----------------------->

一次建成多次使用

![](img/cf3e6a516ab9e838e99a582903776486.png)

Build Once Use Many

W 在执行任何端到端数据科学项目时，任何*数据科学专业人员或学生*都必须主要关注问题定义、数据收集、数据调查、清理、统计和可视化分析、特征工程、决策和模型构建。在一个数据科学项目的整个*周期中，一个不应该投入太多时间的一个步骤就是编写构建任何 ML 模型的代码。*

本文假设读者已经很好地理解了 ML 模型以及如何使用 python 中的“scikit-learn([*sk learn*](https://scikit-learn.org/stable/)*)*”库来构建/实现它们。这是关于使我们的代码可重用，以便我们可以开发任何模型，而不用浪费太多时间编码，并避免编写重复的代码。

![](img/8eca4dfb4e0a3cc444dd8941c70597a7.png)

Always try to write reusable codes wherever possible

在对给定数据集进行 [*探索性数据分析*](https://en.wikipedia.org/wiki/Exploratory_data_analysis#:~:text=In+statistics%2C+exploratory+data+analysis,and+other+data+visualization+methods.) *的漫长而必要的*过程之后，我们使用 **train_test_split 将数据集( *X —自变量，y —目标变量)*拆分成训练集和测试集。**现在我们有 4 个由函数返回的变量— *X_train、X_test、y_train 和 y_test。***

```
from sklearn.model_selection import train_test_splitX_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=0)print(X_train.shape)
print(y_train.shape)print(X_test.shape)
print(y_test.shape)
```

**注**:所有的*特征工程和特征选择*过程都应该在下面给出的步骤之前进行。

为了实现任何 ML 模型，我在我的代码库中保存了以下函数，我重用这些函数来开发任何 cl *辅助器*或*回归器*模型。

## ***代码片段 1*** *:*

***初始化数据帧以存储和比较模型的性能指标。***

我用下面的代码片段将模型性能分数存储在一个数据框中，以便在预测后比较不同的模型。

```
 ***# For Classifier***import pandas as pd
import numpy as np#This dataframe stores the scores from classifier models
df_model=pd.DataFrame(columns=['Model','Accuracy Score' ,'F1 Score', 'Precision Score' , 'Recall Score' ,'ROC AUC'])
**df_model_performance** =df_model#This dataframe stores the train and test accuracy from classifier models to compare at the end of the model building. This can also be further modified to compare the other scores such as F1 score etc
df_model_test_train_acc = pd.DataFrame(columns=['Model' , 'Train Accuracy Score' ,'Test Accuracy Score'])
**df_model_accuracy** =df_model_test_train_acc
```

```
***# For Regressor***import pandas as pd
import numpy as np#This dataframe stores the scores from regressor models
df_model=pd.DataFrame(columns=['Model', 'MAE' ,'RMSE', 'R2 Score' , 'Adjusted R2 Score'])
**df_model_performance** =df_model#This data frame stores the train and test "adjusted R2 scores" from regressor models to compare at the end of the model building. This can also be further modified to compare the other score such as MSE , RMSE  etc
df_model_test_train_r2 = pd.DataFrame(columns=['Model' , 'Train Adjusted R2 Score' ,'Test Adjusted R2 Score'])
**df_model_r2** =df_model_test_train_r2
```

## 代码片段 *2 :*

***功能通过使用***[***GridSearchCV***](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html)***执行超参数调整来获得最佳模型。***

我定义了一个函数"**get _ best _ hyperparameters "**，它通过将分类器或回归器模型作为输入，使用 GridSearchCV 进行超参数调整。此函数返回可用于拟合和预测的最佳模型。如果只想构建一个基本模型而不执行任何超参数调整，可以跳过这一步。

```
***# For both Classifier and Regressor***from sklearn.model_selection import GridSearchCV 
**def get_best_hyperparameters**(model, params, cv_value , X_train, y_train ): 
    search = GridSearchCV(estimator=model, param_grid=params, n_jobs=-1, verbose=1,cv=cv_value) 
    search.fit(X_train, y_train)  
    print("Best Accuracy    :",  search.best_score_) 
    print("Best Parameters  : ", search.best_params_)
    print("Best Estimators : ",  search.best_estimator_)  
    best_grid = search.best_estimator_
    **return** best_grid
```

## 代码片段 3:

***函数拟合和预测模型:***

此函数(用于分类器和回归器)**get _ classifier _ predictions/get _ regressor _ predictions**将模型作为输入，并返回预测的训练和测试结果。在分类器的情况下，它还返回预测的训练和测试概率。

```
***#For Classifier*****def get_classifier_predictions**(classifier, X_train, y_train, X_test): 
    classifier.fit(X_train,y_train)
    y_pred_train =classifier.predict(X_train)
    y_pred_test = classifier.predict(X_test)
    y_pred_prob_train = classifier.predict_proba(X_train)
    y_pred_prob_test = classifier.predict_proba(X_test)
    **return** y_pred_train, y_pred_test, y_pred_prob_train,y_pred_prob_test
```

```
***#For Regressor*****def get_regressor_predictions**(regressor, X_train, y_train, X_test):  
    regressor.fit(X_train,y_train)
    y_pred_train =regressor.predict(X_train)
    y_pred_test = regressor.predict(X_test)
    **return** y_pred_train, y_pred_test
```

## 代码片段 4:

***函数计算并打印训练和测试数据集的性能指标***

函数**print _ classifier _ scores/print _ regressor _ scores**计算并返回分别与分类/回归算法相关的所有性能指标分数的数据集。

```
***# For Classifier***from sklearn.metrics import accuracy_score ,confusion_matrix ,precision_score , recall_score , f1_score, plot_confusion_matrix ,roc_auc_score
import matplotlib.pyplot as plt                                     # Importing pyplot interface to use matplotlib
%matplotlib inline**def print_classifier_scores**(classifier, X_train, X_test, y_train ,y_test,y_pred_train, y_pred_test,y_pred_prob_train, y_pred_prob_test,algorithm):
# store classifier scores for Training Dataset
    v_recall_score_train =  recall_score(y_train,y_pred_train)
    v_precision_score_train = precision_score(y_train,y_pred_train)
    v_f1_score_train =  f1_score(y_train,y_pred_train)
    v_accuracy_score_train = accuracy_score(y_train,y_pred_train)
    v_roc_auc_train = roc_auc_score(y_train, y_pred_prob_train[:,1])

# print classifier scores for Training Dataset
    print('Train-Set Confusion Matrix:\n', confusion_matrix(y_train,y_pred_train)) 
    print("Recall Score    : ", v_recall_score_train)
    print("Precision Score : ", v_precision_score_train)
    print("F1 Score        : ", v_f1_score_train)
    print("Accuracy Score  : ", v_accuracy_score_train)
    print("ROC AUC         :  {}".format(v_roc_auc_train))
    print("Predict Probability  :" , y_pred_prob_train)
    plot_confusion_matrix(classifier, X_train , y_train , display_labels = ["1" , "0"])
    plt.grid(b=None)
# store classifier scores for Testing Dataset 

    v_recall_score_test =  recall_score(y_test,y_pred_test)
    v_precision_score_test = precision_score(y_test,y_pred_test)
    v_f1_score_test =  f1_score(y_test,y_pred_test)
    v_accuracy_score_test = accuracy_score(y_test,y_pred_test)
    v_roc_auc_test = roc_auc_score(y_test, y_pred_prob_test[:,1])
# Print classifier scores for Testing Dataset    
    print('Test-Set Confusion Matrix:\n', confusion_matrix(y_test,y_pred_test)) 
    print("Recall Score    : ", v_recall_score_test)
    print("Precision Score : ", v_precision_score_test)
    print("F1 Score        : ", v_f1_score_test)
    print("Accuracy Score  : ", v_accuracy_score_test)
    print("ROC AUC         :  {}".format(v_roc_auc_test))
    print("Predict Probability  :" , y_pred_prob_test)
    plot_confusion_matrix(classifier, X_test , y_test , display_labels = ["1" , "0"])
    plt.grid(b=None)
# store to append the results in dataframe for final comparison of performance 
    df_model_test_train_acc = dict({'Model' : algorithm, 'Train Accuracy Score' :v_accuracy_score_train,'Test Accuracy Score' :v_accuracy_score_test })
    df_model_performance = dict({'Model' : algorithm, 'Accuracy Score' :v_accuracy_score_test, 'F1 Score' : v_f1_score_test, 'Precision Score' : v_precision_score_test, 'Recall Score' :v_recall_score_test, 'ROC AUC' : v_roc_auc_test})

    **return** df_model_test_train_acc , df_model_performance
```

```
**# For regressor** from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
**def print_regressor_scores**(regressor, X_train, X_test, y_train ,y_test,y_pred_train, y_pred_test,algorithm):

    # store regressor scores for Training Dataset
    MAE_train = mean_absolute_error(y_train, y_pred_train)
    RMSE_train = np.sqrt( mean_squared_error(y_train, y_pred_train))
    r2_score_train = r2_score(y_train, y_pred_train)
    # Calculating Adjusted R2 for training set
    SS_Residual_train = sum((y_train-y_pred_train)**2)
    SS_Total_train = sum((y_train-np.mean(y_train))**2)
    r_squared_train = 1 - (float(SS_Residual_train))/SS_Total_train
    adj_r_sq_train = 1 - (1-r_squared_train)*(len(y_train)-1)/(len(y_train)-X_train.shape[1]-1)

    # print regressor scores for Training Dataset
    print('MAE for training set is {}'.format(MAE_train))
    print('RMSE for training set is {}'.format(RMSE_train))
    print('R squared score for training set is {}'.format(r2_score_train))
    print('Adjusted R squared score for training set is {}'.format(adj_r_sq_train))

    # store regressor scores for Test Dataset
    MAE_test = mean_absolute_error(y_test, y_pred_test)
    RMSE_test = np.sqrt(mean_squared_error(y_test, y_pred_test))
    r2_score_test = r2_score(y_test, y_pred_test)
    # Calculating Adjusted R2 for test set
    SS_Residual_test = sum((y_test-y_pred_test)**2)
    SS_Total_test = sum((y_test-np.mean(y_test))**2)
    r_squared_test = 1 - (float(SS_Residual_test))/SS_Total_test
    adj_r_sq_test = 1 - (1-r_squared_test)*(len(y_test)-1)/(len(y_test)-X_test.shape[1]-1)

    # print regressor scores for Test Dataset 
    print('MAE for test set is {}'.format(MAE_test))
    print('RMSE for test set is {}'.format(RMSE_test))
    print('R squared score for test set is {}'.format(r2_score_test))
    print('Adjusted R squared score for testing set is {}'.format(adj_r_sq_test))

    # store to append the results in dataframe for final comparison of performance
    df_model_test_train_r2= dict({'Model' : algorithm, 'Train Adjusted R2 Score' :adj_r_sq_train,'Test Adjusted R2 Score' :adj_r_sq_test })
    df_model_performance = dict({'Model' : algorithm, 'MAE' : MAE_test, 'RMSE' : RMSE_test, 'R2 Score' : r2_score_test, 'Adjusted R2 Score' :adj_r_sq_test})
    **return** df_model_test_train_r2 , df_model_performance
```

在那里！

现在，我可以开发任何 ML 模型，我可以进行预测，计算分数，并比较模型性能，只需为上述函数提供正确的模型和参数。

## 分类器示例:

下面的例子展示了如何使用这些函数建立一个**逻辑回归模型** (GitHub 链接[此处](https://github.com/gayathrig21/Medium_Examples/tree/main/ML_Code_Repo_Article)):

1.  设置超参数调整的参数，并将初始化的模型传递给函数**get _ best _ hyperparameters**以获得最佳网格。这一步是可选的，也可以传递一个空的参数列表。

```
from sklearn.linear_model import LogisticRegression
logreg_params = {'penalty' : ['l2'],
                 'C' : np.logspace(-1, 2, 100),
                 'solver' :['liblinear'],
                 'random_state' :[42,99]
                 }
lr_best_grid= **get_best_hyperparameters**(LogisticRegression(), logreg_params, 5, X_train, y_train)
```

2.将最佳模型传递给函数**get _ classifier _ predictions**以获得预测结果和概率。

```
y_pred_train, y_pred_test, y_pred_prob_train, y_pred_prob_test = **get_classifier_predictions**(lr_best_grid, X_train, y_train, X_test )
```

3.将预测结果输入到函数**print _ classifier _ scores**中，计算性能指标得分并打印结果。

```
df_model_test_train_acc1, df_model_performance1=**print_classifier_scores**(lr_best_grid, X_train, X_test, y_train , y_test, y_pred_train, y_pred_test, y_pred_prob_train, y_pred_prob_test , 'Logistic Regression')
```

4.将结果附加到数据帧，以比较所有构建的模型性能

```
df_model=df_model.append(df_model_performance1,ignore_index=True )
df_model_test_train_acc= df_model_test_train_acc.append(df_model_test_train_acc1, ignore_index=True)
```

## 回归变量示例:

下面的例子展示了如何使用这些函数建立一个**线性回归模型** (GitHub 链接[此处为](https://github.com/gayathrig21/Medium_Examples/tree/main/ML_Code_Repo_Article)):

1.  设置超参数调整的参数，并将初始化的模型传递给函数**get _ best _ hyperparameters**以获得最佳网格。这一步是可选的，也可以传递一个空的参数列表。

```
from sklearn.linear_model import LinearRegression
parameters = {'fit_intercept':[True,False],  'copy_X':[True, False]}
lr_best_grid= **get_best_hyperparameters**(LinearRegression(), parameters, 5, X_train, y_train)
```

2.将最佳模型传递给函数**get _ regressor _ predictions**以获得预测结果。

```
y_pred_train, y_pred_test = **get_regressor_predictions**(lr_best_grid, X_train, y_train, X_test )
```

3.将预测结果输入到函数 **print_regressor_scores** 中，以计算性能指标分数并打印结果。

```
df_model_test_train_r2_1, df_model_performance1=**print_regressor_scores**(lr_best_grid, X_train, X_test, y_train , y_test, y_pred_train, y_pred_test , 'Linear Regression')
```

4.将结果附加到数据帧，以比较所有构建的模型性能

```
df_model=df_model.append(df_model_performance1,ignore_index=True )
df_model_r2= df_model_r2.append(df_model_test_train_r2_1, ignore_index=True)
```

现在，我们可以使用这些函数，通过传递模型特定参数来开发任何模型，如上例中对*线性回归*和*逻辑回归*所做的那样。

耶……快乐模型建筑！！！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)