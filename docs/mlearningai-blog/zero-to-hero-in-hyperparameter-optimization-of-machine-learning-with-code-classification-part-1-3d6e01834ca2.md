# 代码机器学习的超参数优化:分类:第一部分

> 原文：<https://medium.com/mlearning-ai/zero-to-hero-in-hyperparameter-optimization-of-machine-learning-with-code-classification-part-1-3d6e01834ca2?source=collection_archive---------2----------------------->

机器学习算法已经广泛应用于各种应用和领域。为了使机器学习模型适合不同的问题，必须调整其超参数。为机器学习模型选择最佳超参数配置对模型的性能有直接影响。它通常需要深入了解机器学习算法和适当的超参数优化技术。虽然存在几种自动优化技术，但是当应用于不同类型的问题时，它们具有不同的优点和缺点。

首先我们将做分类的超参数优化。

**分类算法:**

我们将应用以下算法

1.  随机森林
2.  支持向量机(SVM)
3.  k 近邻(KNN)
4.  人工神经网络

**超参数优化技术:**

1.  网格搜索
2.  随机搜索
3.  超波段
4.  高斯过程贝叶斯优化(BO-GP)
5.  基于树结构 Parzen 估计量的贝叶斯优化
6.  粒子群优化算法
7.  遗传算法。

# 加载 MNIST 数据集

MNIST 数据库(改进的国家标准和技术研究所数据库)是一个手写数字的大型数据库，通常用于训练各种图像处理系统。MNIST 数据库具有 60，000 个样本的训练集和 10，000 个样本的测试集。这是从 NIST 可获得的更大集合的子集。数字已经过大小标准化，并在固定大小的图像中居中。

# 具有默认超参数的分类器

随机森林:

```
Accuracy:0.896553659132438
```

SVM:

```
Accuracy:0.9705030401091262
```

KNN:

```
clf **=** KNeighborsClassifier()
clf**.**fit(X,y)
scores **=** cross_val_score(clf, X, y, cv**=**3,scoring**=**'accuracy')
print("Accuracy:"**+** str(scores**.**mean()))Accuracy:0.9627317178438357
```

安:

```
*#ANN*
**from** keras.models **import** Sequential, Model
**from** keras.layers **import** Dense, Input
**from** sklearn.model_selection **import** GridSearchCV
**from** keras.wrappers.scikit_learn **import** KerasClassifier
**from** keras.callbacks **import** EarlyStopping
**def** ANN(optimizer **=** 'sgd',neurons**=**32,batch_size**=**32,epochs**=**20,activation**=**'relu',patience**=**3,loss**=**'categorical_crossentropy'):
    model **=** Sequential()
    model**.**add(Dense(neurons, input_shape**=**(X**.**shape[1],), activation**=**activation))
    model**.**add(Dense(neurons, activation**=**activation))
    model**.**add(Dense(10,activation**=**'softmax'))  *# 10 is the number of classes in the dataset, you can change it based on your dataset*
    model**.**compile(optimizer **=** optimizer, loss**=**loss)
    early_stopping **=** EarlyStopping(monitor**=**"loss", patience **=** patience)*# early stop patience*
    history **=** model**.**fit(X, pd**.**get_dummies(y)**.**values,
              batch_size**=**batch_size,
              epochs**=**epochs,
              callbacks **=** [early_stopping],
              verbose**=**0) *#verbose set to 1 will show the training* 
    **return** modelclf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
scores **=** cross_val_score(clf, X, y, cv**=**3,scoring**=**'accuracy')
print("Accuracy:"**+** str(scores**.**mean()))Accuracy:0.9894268224819144
```

现在我们将尝试超参数优化技术。

# 1.网格搜索

**优点:**

*   简单的实现。

**缺点:**

*   耗时，
*   仅对分类 HPs 有效。

随机林实现:

```
*#Random Forest*
**from** sklearn.model_selection **import** GridSearchCV
*# Define the hyperparameter configuration space*
rf_params **=** {
    'n_estimators': [10, 20, 30],
    *#'max_features': ['sqrt',0.5],*
    'max_depth': [15,20,30,50],
    *#'min_samples_leaf': [1,2,4,8],*
    *#"bootstrap":[True,False],*
    "criterion":['gini','entropy']
}
clf **=** RandomForestClassifier(random_state**=**0)
grid **=** GridSearchCV(clf, rf_params, cv**=**3, scoring**=**'accuracy')
grid**.**fit(X, y)
print(grid**.**best_params_)
print("Accuracy:"**+** str(grid**.**best_score_)){'n_estimators': 30, 'max_depth': 15, 'criterion': 'entropy'}
Accuracy:0.9332220367278798
```

SVM 实施:

```
*#SVM*
**from** sklearn.model_selection **import** GridSearchCV
rf_params **=** {
    'C': [1,10, 100],
    "kernel":['linear','poly','rbf','sigmoid']
}
clf **=** SVC(gamma**=**'scale')
grid **=** GridSearchCV(clf, rf_params, cv**=**3, scoring**=**'accuracy')
grid**.**fit(X, y)
print(grid**.**best_params_)
print("Accuracy:"**+** str(grid**.**best_score_)){'kernel': 'rbf', 'C': 10}
Accuracy:0.9744017807456873
```

KNN 实施:

```
*#KNN*
**from** sklearn.model_selection **import** GridSearchCV
rf_params **=** {
    'n_neighbors': [2, 3, 5,10,15,20],
}
clf **=** KNeighborsClassifier()
grid **=** GridSearchCV(clf, rf_params, cv**=**3, scoring**=**'accuracy')
grid**.**fit(X, y)
print(grid**.**best_params_)
print("Accuracy:"**+** str(grid**.**best_score_)){'n_neighbors': 3}
Accuracy:0.9682804674457429
```

人工神经网络实施:

```
*#ANN*
**from** sklearn.model_selection **import** GridSearchCV
rf_params **=** {
    'optimizer': ['adam','rmsprop','sgd'],
    'activation': ['relu','tanh'],
    'batch_size': [16,32],
    'neurons':[16,32],
    'epochs':[20,50],
    'patience':[2,5]
}
clf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
grid **=** GridSearchCV(clf, rf_params, cv**=**3,scoring**=**'accuracy')
grid**.**fit(X, y)
print(grid**.**best_params_)
print("MSE:"**+** str(grid**.**best_score_)){'activation': 'tanh', 'epochs': 50, 'optimizer': 'adam', 'patience': 5, 'batch_size': 32, 'neurons': 32}
MSE:0.9961046188091264
```

# 2:随机搜索

在搜索空间中随机搜索超参数组合

**优点:**

*   比 GridSearch 更高效。
*   启用并行化。

**缺点:**

*   不考虑以前的结果。
*   对条件超参数无效。

随机林实现:

```
*#Random Forest*
**from** scipy.stats **import** randint **as** sp_randint
**from** random **import** randrange **as** sp_randrange
**from** sklearn.model_selection **import** RandomizedSearchCV
*# Define the hyperparameter configuration space*
rf_params **=** {
    'n_estimators': sp_randint(10,100),
    "max_features":sp_randint(1,64),
    'max_depth': sp_randint(5,50),
    "min_samples_split":sp_randint(2,11),
    "min_samples_leaf":sp_randint(1,11),
    "criterion":['gini','entropy']
}
n_iter_search**=**20 *#number of iterations is set to 20, you can increase this number if time permits*
clf **=** RandomForestClassifier(random_state**=**0)
Random **=** RandomizedSearchCV(clf, param_distributions**=**rf_params,n_iter**=**n_iter_search,cv**=**3,scoring**=**'accuracy')
Random**.**fit(X, y)
print(Random**.**best_params_)
print("Accuracy:"**+** str(Random**.**best_score_)){'n_estimators': 36, 'max_features': 6, 'max_depth': 42, 'min_samples_split': 6, 'min_samples_leaf': 2, 'criterion': 'gini'}
Accuracy:0.9309961046188091
```

SVM 实施:

```
*#SVM*
**from** scipy.stats **import** randint **as** sp_randint
**from** sklearn.model_selection **import** RandomizedSearchCV
rf_params **=** {
    'C': stats**.**uniform(0,50),
    "kernel":['linear','poly','rbf','sigmoid']
}
n_iter_search**=**20
clf **=** SVC(gamma**=**'scale')
Random **=** RandomizedSearchCV(clf, param_distributions**=**rf_params,n_iter**=**n_iter_search,cv**=**3,scoring**=**'accuracy')
Random**.**fit(X, y)
print(Random**.**best_params_)
print("Accuracy:"**+** str(Random**.**best_score_)){'kernel': 'rbf', 'C': 17.026713515892954}
Accuracy:0.9744017807456873
```

KNN 实施:

```
*#KNN*
**from** scipy.stats **import** randint **as** sp_randint
**from** sklearn.model_selection **import** RandomizedSearchCV
rf_params **=** {
    'n_neighbors': range(1,20),
}
n_iter_search**=**10
clf **=** KNeighborsClassifier()
Random **=** RandomizedSearchCV(clf, param_distributions**=**rf_params,n_iter**=**n_iter_search,cv**=**3,scoring**=**'accuracy')
Random**.**fit(X, y)
print(Random**.**best_params_)
print("Accuracy:"**+** str(Random**.**best_score_)){'n_neighbors': 4}
Accuracy:0.9643850862548692
```

人工神经网络实施:

```
*#ANN*
**from** scipy.stats **import** randint **as** sp_randint
**from** random **import** randrange **as** sp_randrange
**from** sklearn.model_selection **import** RandomizedSearchCV
rf_params **=** {
    'optimizer': ['adam','rmsprop','sgd'],
    'activation': ['relu','tanh'],
    'batch_size': [16,32,64],
    'neurons':sp_randint(10,100),
    'epochs':[20,50],
    *#'epochs':[20,50,100,200],*
    'patience':sp_randint(3,20)
}
n_iter_search**=**10
clf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
Random **=** RandomizedSearchCV(clf, param_distributions**=**rf_params,n_iter**=**n_iter_search,cv**=**3,scoring**=**'accuracy')
Random**.**fit(X, y)
print(Random**.**best_params_)
print("Accuracy:"**+** str(Random**.**best_score_)){'activation': 'relu', 'epochs': 20, 'optimizer': 'adam', 'patience': 8, 'batch_size': 16, 'neurons': 89}
Accuracy:1.0
```

# 3:超波段

生成小型子集，并根据其性能为每个超参数组合分配预算

**优点:**

*   启用并行化。

**缺点:**

*   对有条件的 HPs 无效。
*   要求预算较小的子集具有代表性。

随机林实现:

```
*#Random Forest*
**from** hyperband **import** HyperbandSearchCV
**from** scipy.stats **import** randint **as** sp_randint
**from** random **import** randrange **as** sp_randrange
*# Define the hyperparameter configuration space*
rf_params **=** {
    'n_estimators': sp_randint(10,100),
    "max_features":sp_randint(1,64),
    'max_depth': sp_randint(5,50),
    "min_samples_split":sp_randint(2,11),
    "min_samples_leaf":sp_randint(1,11),
    "criterion":['gini','entropy']
}
clf **=** RandomForestClassifier(random_state**=**0)
hyper **=** HyperbandSearchCV(clf, param_distributions **=**rf_params,cv**=**3,min_iter**=**10,max_iter**=**100,scoring**=**'accuracy')
hyper**.**fit(X, y)
print(hyper**.**best_params_)
print("Accuracy:"**+** str(hyper**.**best_score_)){'n_estimators': 100, 'max_features': 9, 'max_depth': 30, 'min_samples_split': 10, 'min_samples_leaf': 2, 'criterion': 'gini'}
Accuracy:0.9321090706733445
```

SVM 实施:

```
*#SVM*
**from** hyperband **import** HyperbandSearchCV
**from** scipy.stats **import** randint **as** sp_randint
**from** random **import** randrange **as** sp_randrange
rf_params **=** {
    'C': stats**.**uniform(0,50),
    "kernel":['linear','poly','rbf','sigmoid']
}
clf **=** SVC(gamma**=**'scale')
hyper **=** HyperbandSearchCV(clf, param_distributions **=**rf_params,cv**=**3,min_iter**=**1,max_iter**=**50,scoring**=**'accuracy',resource_param**=**'C')
hyper**.**fit(X, y)
print(hyper**.**best_params_)
print("Accuracy:"**+** str(hyper**.**best_score_)){'kernel': 'rbf', 'C': 16}
Accuracy:0.9744017807456873
```

KNN 实施:

```
*#KNN*
**from** hyperband **import** HyperbandSearchCV
**from** scipy.stats **import** randint **as** sp_randint
**from** random **import** randrange **as** sp_randrange
rf_params **=** {
    'n_neighbors': range(1,20),
}
clf **=** KNeighborsClassifier()
hyper **=** HyperbandSearchCV(clf, param_distributions **=**rf_params,cv**=**3,min_iter**=**1,max_iter**=**20,scoring**=**'accuracy',resource_param**=**'n_neighbors')
hyper**.**fit(X, y)
print(hyper**.**best_params_)
print("Accuracy:"**+** str(hyper**.**best_score_)){'n_neighbors': 2}
Accuracy:0.9621591541457986
```

人工神经网络实施:

```
*#ANN*
**from** hyperband **import** HyperbandSearchCV
**from** scipy.stats **import** randint **as** sp_randint
rf_params **=** {
    'optimizer': ['adam','rmsprop','sgd'],
    'activation': ['relu','tanh'],
    'batch_size': [16,32,64],
    'neurons':sp_randint(10,100),
    'epochs':[20,50],
    *#'epochs':[20,50,100,200],*
    'patience':sp_randint(3,20)
}
clf **=** KerasClassifier(build_fn**=**ANN, epochs**=**20, verbose**=**0)
hyper **=** HyperbandSearchCV(clf, param_distributions **=**rf_params,cv**=**3,min_iter**=**1,max_iter**=**10,scoring**=**'accuracy',resource_param**=**'epochs')
hyper**.**fit(X, y)
print(hyper**.**best_params_)
print("Accuracy:"**+** str(hyper**.**best_score_)){'activation': 'tanh', 'epochs': 10, 'batch_size': 16, 'patience': 7, 'optimizer': 'adam', 'neurons': 76}
Accuracy:0.9994435169727324
```

# 4:高斯过程贝叶斯优化(BO-GP)

**优点:**

*   连续 HPs 的快速收敛速度。

**缺点:**

*   并行能力差。
*   对有条件的 HPs 无效。

随机林实现:

```
*#Random Forest*
**from** skopt **import** Optimizer
**from** skopt **import** BayesSearchCV 
**from** skopt.space **import** Real, Categorical, Integer
*# Define the hyperparameter configuration space*
rf_params **=** {
    'n_estimators': Integer(10,100),
    "max_features":Integer(1,64),
    'max_depth': Integer(5,50),
    "min_samples_split":Integer(2,11),
    "min_samples_leaf":Integer(1,11),
    "criterion":['gini','entropy']
}
clf **=** RandomForestClassifier(random_state**=**0)
Bayes **=** BayesSearchCV(clf, rf_params,cv**=**3,n_iter**=**20, n_jobs**=-**1,scoring**=**'accuracy')
*#number of iterations is set to 20, you can increase this number if time permits*
Bayes**.**fit(X, y)
print(Bayes**.**best_params_)
bclf **=** Bayes**.**best_estimator_
print("Accuracy:"**+** str(Bayes**.**best_score_)){'n_estimators': 100, 'max_features': 1, 'max_depth': 19, 'min_samples_split': 2, 'min_samples_leaf': 1, 'criterion': 'gini'}
Accuracy:0.9398998330550918
```

SVM 实施:

```
*#SVM*
**from** skopt **import** Optimizer
**from** skopt **import** BayesSearchCV 
**from** skopt.space **import** Real, Categorical, Integer
rf_params **=** {
    'C': Real(0.01,50),
    "kernel":['linear','poly','rbf','sigmoid']
}
clf **=** SVC(gamma**=**'scale')
Bayes **=** BayesSearchCV(clf, rf_params,cv**=**3,n_iter**=**20, n_jobs**=-**1,scoring**=**'accuracy')
Bayes**.**fit(X, y)
print(Bayes**.**best_params_)
bclf **=** Bayes**.**best_estimator_
print("Accuracy:"**+** str(Bayes**.**best_score_)){'kernel': 'rbf', 'C': 26.52140440440126}
Accuracy:0.9744017807456873
```

KNN 实施:

```
*#KNN*
**from** skopt **import** Optimizer
**from** skopt **import** BayesSearchCV 
**from** skopt.space **import** Real, Categorical, Integer
rf_params **=** {
    'n_neighbors': Integer(1,20),
}
clf **=** KNeighborsClassifier()
Bayes **=** BayesSearchCV(clf, rf_params,cv**=**3,n_iter**=**10, n_jobs**=-**1,scoring**=**'accuracy')
Bayes**.**fit(X, y)
print(Bayes**.**best_params_)
bclf **=** Bayes**.**best_estimator_
print("Accuracy:"**+** str(Bayes**.**best_score_)){'n_neighbors': 3}
```

人工神经网络实施:

```
*#ANN*
**from** skopt **import** Optimizer
**from** skopt **import** BayesSearchCV 
**from** skopt.space **import** Real, Categorical, Integer
rf_params **=** {
    'optimizer': ['adam','rmsprop','sgd'],
    'activation': ['relu','tanh'],
    'batch_size': [16,32,64],
    'neurons':Integer(10,100),
    'epochs':[20,50],
    *#'epochs':[20,50,100,200],*
    'patience':Integer(3,20)
}
clf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
Bayes **=** BayesSearchCV(clf, rf_params,cv**=**3,n_iter**=**10, scoring**=**'accuracy')
Bayes**.**fit(X, y)
print(Bayes**.**best_params_)
print("Accuracy:"**+** str(Bayes**.**best_score_)){'activation': 'tanh', 'epochs': 47, 'optimizer': 'adam', 'patience': 10, 'batch_size': 16, 'neurons': 54}
Accuracy:1.0
```

# 5:使用树结构 Parzen 估计量 BO-TPE 的贝叶斯优化

**优点:**

*   适用于所有类型的 HP。
*   保留条件依赖。

**缺点:**

*   并行能力差。

随机林实现:

```
*#Random Forest*
**from** hyperopt **import** hp, fmin, tpe, STATUS_OK, Trials
**from** sklearn.model_selection **import** cross_val_score, StratifiedKFold
*# Define the objective function*
**def** objective(params):
    params **=** {
        'n_estimators': int(params['n_estimators']), 
        'max_depth': int(params['max_depth']),
        'max_features': int(params['max_features']),
        "min_samples_split":int(params['min_samples_split']),
        "min_samples_leaf":int(params['min_samples_leaf']),
        "criterion":str(params['criterion'])
    }
    clf **=** RandomForestClassifier( ******params)
    score **=** cross_val_score(clf, X, y, scoring**=**'accuracy', cv**=**StratifiedKFold(n_splits**=**3))**.**mean()
    *#print("ROC-AUC {:.3f} params {}".format(score, params))*

    **return** {'loss':**-**score, 'status': STATUS_OK }
*# Define the hyperparameter configuration space*
space **=** {
    'n_estimators': hp**.**quniform('n_estimators', 10, 100, 1),
    'max_depth': hp**.**quniform('max_depth', 5, 50, 1),
    "max_features":hp**.**quniform('max_features', 1, 64, 1),
    "min_samples_split":hp**.**quniform('min_samples_split',2,11,1),
    "min_samples_leaf":hp**.**quniform('min_samples_leaf',1,11,1),
    "criterion":hp**.**choice('criterion',['gini','entropy'])
}

best **=** fmin(fn**=**objective,
            space**=**space,
            algo**=**tpe**.**suggest,
            max_evals**=**20)
print("Random Forest: Hyperopt estimated optimum {}"**.**format(best))100%|██████████████████████████████████████████████████| 20/20 [00:11<00:00,  1.76it/s, best loss: -0.9348679045482652]
Random Forest: Hyperopt estimated optimum {'n_estimators': 95.0, 'max_features': 13.0, 'max_depth': 39.0, 'min_samples_split': 3.0, 'min_samples_leaf': 2.0, 'criterion': 0}
```

SVM 实施:

```
*#SVM*
**from** hyperopt **import** hp, fmin, tpe, STATUS_OK, Trials
**from** sklearn.model_selection **import** cross_val_score, StratifiedKFold
**def** objective(params):
    params **=** {
        'C': abs(float(params['C'])), 
        "kernel":str(params['kernel'])
    }
    clf **=** SVC(gamma**=**'scale', ******params)
    score **=** cross_val_score(clf, X, y, scoring**=**'accuracy', cv**=**StratifiedKFold(n_splits**=**3))**.**mean()

    **return** {'loss':**-**score, 'status': STATUS_OK }

space **=** {
    'C': hp**.**normal('C', 0, 50),
    "kernel":hp**.**choice('kernel',['linear','poly','rbf','sigmoid'])
}

best **=** fmin(fn**=**objective,
            space**=**space,
            algo**=**tpe**.**suggest,
            max_evals**=**20)
print("SVM: Hyperopt estimated optimum {}"**.**format(best))100%|██████████████████████████████████████████████████| 20/20 [00:02<00:00,  7.18it/s, best loss: -0.9749661645191837]
SVM: Hyperopt estimated optimum {'kernel': 2, 'C': 5.89740125613865}
```

KNN 实施:

```
*#KNN*
**from** hyperopt **import** hp, fmin, tpe, STATUS_OK, Trials
**from** sklearn.model_selection **import** cross_val_score, StratifiedKFold
**def** objective(params):
    params **=** {
        'n_neighbors': abs(int(params['n_neighbors']))
    }
    clf **=** KNeighborsClassifier( ******params)
    score **=** cross_val_score(clf, X, y, scoring**=**'accuracy', cv**=**StratifiedKFold(n_splits**=**3))**.**mean()

    **return** {'loss':**-**score, 'status': STATUS_OK }

space **=** {
    'n_neighbors': hp**.**quniform('n_neighbors', 1, 20, 1),
}

best **=** fmin(fn**=**objective,
            space**=**space,
            algo**=**tpe**.**suggest,
            max_evals**=**10)
print("KNN: Hyperopt estimated optimum {}"**.**format(best))100%|███████████████████████████████████████████████████| 10/10 [00:02<00:00,  4.34it/s, best loss: -0.968293886616605]
KNN: Hyperopt estimated optimum {'n_neighbors': 3.0}
```

人工神经网络实施:

```
*#ANN*
**from** hyperopt **import** hp, fmin, tpe, STATUS_OK, Trials
**from** sklearn.model_selection **import** cross_val_score, StratifiedKFold
**def** objective(params):
    params **=** {
        "optimizer":str(params['optimizer']),
        "activation":str(params['activation']),
        'batch_size': abs(int(params['batch_size'])),
        'neurons': abs(int(params['neurons'])),
        'epochs': abs(int(params['epochs'])),
        'patience': abs(int(params['patience']))
    }
    clf **=** KerasClassifier(build_fn**=**ANN,******params, verbose**=**0)
    score **=** **-**np**.**mean(cross_val_score(clf, X, y, cv**=**3, 
                                    scoring**=**"accuracy"))

    **return** {'loss':score, 'status': STATUS_OK }

space **=** {
    "optimizer":hp**.**choice('optimizer',['adam','rmsprop','sgd']),
    "activation":hp**.**choice('activation',['relu','tanh']),
    'batch_size': hp**.**quniform('batch_size', 16, 64, 16),
    'neurons': hp**.**quniform('neurons', 10, 100, 10),
    'epochs': hp**.**quniform('epochs', 20, 50, 10),
    'patience': hp**.**quniform('patience', 3, 20, 3),
}

best **=** fmin(fn**=**objective,
            space**=**space,
            algo**=**tpe**.**suggest,
            max_evals**=**10)
print("ANN: Hyperopt estimated optimum {}"**.**format(best))100%|█████████████████████████████████████████████████████████████████| 10/10 [06:29<00:00, 38.92s/it, best loss: -1.0]
ANN: Hyperopt estimated optimum {'activation': 1, 'epochs': 30.0, 'optimizer': 0, 'patience': 9.0, 'batch_size': 48.0, 'neurons': 60.0}
```

# 6:粒子群优化算法

粒子群优化(PSO):群中的每个粒子与其他粒子通信，在每次迭代中检测和更新当前的全局最优值，直到检测到最终的最优值。

**优点:**

*   适用于所有类型的 HP。
*   启用并行化。

**缺点:**

*   需要正确的初始化。

随机林实现:

```
*#Random Forest*
**import** optunity
**import** optunity.metrics

data**=**X
labels**=**y**.**tolist()
*# Define the hyperparameter configuration space*
search **=** {
    'n_estimators': [10, 100],
    'max_features': [1, 64],
    'max_depth': [5,50],
    "min_samples_split":[2,11],
    "min_samples_leaf":[1,11],
    "criterion":[0,1]
         }
*# Define the objective function*
@optunity**.**cross_validated(x**=**data, y**=**labels, num_folds**=**3)
**def** performance(x_train, y_train, x_test, y_test,n_estimators**=None**, max_features**=None**,max_depth**=None**,min_samples_split**=None**,min_samples_leaf**=None**,criterion**=None**):
    *# fit the model*
    **if** criterion**<**0.5:
        cri**=**'gini'
    **else**:
        cri**=**'entropy'
    model **=** RandomForestClassifier(n_estimators**=**int(n_estimators),
                                   max_features**=**int(max_features),
                                   max_depth**=**int(max_depth),
                                   min_samples_split**=**int(min_samples_split),
                                   min_samples_leaf**=**int(min_samples_leaf),
                                   criterion**=**cri,
                                  )
    *#predictions = model.predict(x_test)*
    scores**=**np**.**mean(cross_val_score(model, X, y, cv**=**3, n_jobs**=-**1,
                                    scoring**=**"accuracy"))
    *#return optunity.metrics.roc_auc(y_test, predictions, positive=True)*
    **return** scores*#optunity.metrics.accuracy(y_test, predictions)*

optimal_configuration, info, _ **=** optunity**.**maximize(performance,
                                                  solver_name**=**'particle swarm',
                                                  num_evals**=**20,
                                                   ******search
                                                  )
print(optimal_configuration)
print("Accuracy:"**+** str(info**.**optimum)){'n_estimators': 72.7099609375, 'max_features': 7.49072265625, 'max_depth': 28.31298828125, 'min_samples_split': 10.63525390625, 'min_samples_leaf': 5.6337890625, 'criterion': 0.22998046875}
Accuracy:0.9255791726758763
```

SVM 实施:

```
*#SVM*
**import** optunity
**import** optunity.metrics

data**=**X
labels**=**y**.**tolist()

search **=** {
    'C': (0,50),
    'kernel':[0,4]
         }
@optunity**.**cross_validated(x**=**data, y**=**labels, num_folds**=**3)
**def** performance(x_train, y_train, x_test, y_test,C**=None**,kernel**=None**):
    *# fit the model*
    **if** kernel**<**1:
        ke**=**'linear'
    **elif** kernel**<**2:
        ke**=**'poly'
    **elif** kernel**<**3:
        ke**=**'rbf'
    **else**:
        ke**=**'sigmoid'
    model **=** SVC(C**=**float(C),
                kernel**=**ke
                                  )
    *#predictions = model.predict(x_test)*
    scores**=**np**.**mean(cross_val_score(model, X, y, cv**=**3, n_jobs**=-**1,
                                    scoring**=**"accuracy"))
    *#return optunity.metrics.roc_auc(y_test, predictions, positive=True)*
    **return** scores*#optunity.metrics.accuracy(y_test, predictions)*

optimal_configuration, info, _ **=** optunity**.**maximize(performance,
                                                  solver_name**=**'particle swarm',
                                                  num_evals**=**20,
                                                   ******search
                                                  )
print(optimal_configuration)
print("Accuracy:"**+** str(info**.**optimum)){'kernel': 1.55078125, 'C': 47.998046875}
Accuracy:0.9604833211865952
```

KNN 实施:

```
*#KNN*
**import** optunity
**import** optunity.metrics

data**=**X
labels**=**y**.**tolist()

search **=** {
    'n_neighbors': [1, 20],
         }
@optunity**.**cross_validated(x**=**data, y**=**labels, num_folds**=**3)
**def** performance(x_train, y_train, x_test, y_test,n_neighbors**=None**):
    *# fit the model*
    model **=** KNeighborsClassifier(n_neighbors**=**int(n_neighbors),
                                  )
    scores**=**np**.**mean(cross_val_score(model, X, y, cv**=**3, n_jobs**=-**1,
                                    scoring**=**"accuracy"))
    **return** scores

optimal_configuration, info, _ **=** optunity**.**maximize(performance,
                                                  solver_name**=**'particle swarm',
                                                  num_evals**=**10,
                                                   ******search
                                                  )
print(optimal_configuration)
print("Accuracy:"**+** str(info**.**optimum)){'n_neighbors': 3.12451171875}
Accuracy:0.968293886616605
```

人工神经网络实施:

```
*#ANN*
**import** optunity
**import** optunity.metrics

data**=**X
labels**=**y**.**tolist()

search **=** {
    'optimizer':[0,3],
    'activation':[0,2],
    'batch_size': [0, 2],
    'neurons': [10, 100],
    'epochs': [20, 50],
    'patience': [3, 20],
         }
@optunity**.**cross_validated(x**=**data, y**=**labels, num_folds**=**3)
**def** performance(x_train, y_train, x_test, y_test,optimizer**=None**,activation**=None**,batch_size**=None**,neurons**=None**,epochs**=None**,patience**=None**):
    *# fit the model*
    **if** optimizer**<**1:
        op**=**'adam'
    **elif** optimizer**<**2:
        op**=**'sgd'
    **else**:
        op**=**'rmsprop'
    **if** activation**<**1:
        ac**=**'relu'
    **else**:
        ac**=**'tanh'
    **if** batch_size**<**1:
        ba**=**16
    **else**:
        ba**=**32
    model **=** ANN(optimizer**=**op,
                activation**=**ac,
                batch_size**=**ba,
                neurons**=**int(neurons),
                epochs**=**int(epochs),
                patience**=**int(patience)
                                  )
    clf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
    scores**=**np**.**mean(cross_val_score(clf, X, y, cv**=**3, 
                                    scoring**=**"accuracy"))

    **return** scores

optimal_configuration, info, _ **=** optunity**.**maximize(performance,
                                                  solver_name**=**'particle swarm',
                                                  num_evals**=**20,
                                                   ******search
                                                  )
print(optimal_configuration)
print("MSE:"**+** str(info**.**optimum)){'activation': 1.962890625, 'epochs': 21.611328125, 'optimizer': 2.6572265625, 'patience': 16.0322265625, 'batch_size': 1.041015625, 'neurons': 56.318359375}
MSE:0.9905397885364496
```

# 7:遗传算法

遗传算法在每一代中检测性能良好的超参数组合，并将它们传递给下一代，直到识别出性能最佳的组合。

**优点:**

*   适用于所有类型的 HP。
*   不需要良好的初始化。

**缺点:**

*   并行能力差

随机林实现:

```
*#Random Forest*
**from** evolutionary_search **import** EvolutionaryAlgorithmSearchCV
*# Define the hyperparameter configuration space*
rf_params **=** {
    'n_estimators': np**.**logspace(1,1.8,num **=** 10 ,base**=**20,dtype**=**'int'),
    'max_depth': np**.**logspace(1,2,num **=** 10 ,base**=**10,dtype**=**'int'),
    "max_features":np**.**logspace(0.2,1,num **=** 5 ,base**=**8,dtype**=**'int'),
    "min_samples_split":np**.**logspace(0.4, 1, num**=**5, base**=**10, dtype**=**'int'), *#[2, 3, 5, 7, 10],*
    "min_samples_leaf":np**.**logspace(0.1,1,num **=** 5 ,base**=**11,dtype**=**'int'),
    "criterion":['gini','entropy']
}
rf_params **=** {
    'n_estimators': range(10,100),
    "max_features":range(1,64),
    'max_depth': range(5,50),
    "min_samples_split":range(2,11),
    "min_samples_leaf":range(1,11),
    *#Categorical(name='criterion', categories=['gini','entropy'])#*
    "criterion":['gini','entropy']
}
clf **=** RandomForestClassifier(random_state**=**0)
*# Set the hyperparameters of GA* 
ga1 **=** EvolutionaryAlgorithmSearchCV(estimator**=**clf,
                                   params**=**rf_params,
                                   scoring**=**"accuracy",
                                   cv**=**3,
                                   verbose**=**1,
                                   population_size**=**10,
                                   gene_mutation_prob**=**0.10,
                                   gene_crossover_prob**=**0.5,
                                   tournament_size**=**3,
                                   generations_number**=**5,
                                   n_jobs**=**1)
ga1**.**fit(X, y)
print(ga1**.**best_params_)
print("Accuracy:"**+** str(ga1**.**best_score_))Types [1, 1, 1, 1, 1, 1] and maxint [89, 62, 44, 8, 9, 1] detected
--- Evolve in 45927000 possible combinations ---
gen	nevals	avg     	min     	max     	std      
0  	10    	0.898664	0.871452	0.920423	0.0133659
1  	8     	0.90345 	0.883139	0.919866	0.00932254
2  	6     	0.911408	0.902059	0.919866	0.00498231
3  	6     	0.914969	0.904285	0.919866	0.00508078
4  	6     	0.918976	0.913189	0.919866	0.0020401 
5  	6     	0.919588	0.917084	0.919866	0.000834725
Best individual is: {'n_estimators': 38, 'max_features': 3, 'max_depth': 16, 'min_samples_split': 6, 'min_samples_leaf': 3, 'criterion': 'gini'}
with fitness: 0.9204229271007234
{'n_estimators': 38, 'max_features': 3, 'max_depth': 16, 'min_samples_split': 6, 'min_samples_leaf': 3, 'criterion': 'gini'}
Accuracy:0.9204229271007234
```

SVM 实施:

```
*#SVM*
**from** evolutionary_search **import** EvolutionaryAlgorithmSearchCV
rf_params **=** {
    'C': np**.**random**.**uniform(0,50,1000),
    "kernel":['linear','poly','rbf','sigmoid']
}
clf **=** SVC(gamma**=**'scale')
ga1 **=** EvolutionaryAlgorithmSearchCV(estimator**=**clf,
                                   params**=**rf_params,
                                   scoring**=**"accuracy",
                                   cv**=**3,
                                   verbose**=**1,
                                   population_size**=**10,
                                   gene_mutation_prob**=**0.10,
                                   gene_crossover_prob**=**0.5,
                                   tournament_size**=**3,
                                   generations_number**=**5,
                                   n_jobs**=**1)
ga1**.**fit(X, y)
print(ga1**.**best_params_)
print("Accuracy:"**+** str(ga1**.**best_score_))Types [1, 2] and maxint [3, 999] detected
--- Evolve in 4000 possible combinations ---
gen	nevals	avg     	min     	max     	std     
0  	10    	0.906177	0.760156	0.974402	0.089037
1  	7     	0.949527	0.753478	0.974402	0.0655796
2  	7     	0.974402	0.974402	0.974402	0        
3  	6     	0.952977	0.760156	0.974402	0.0642738
4  	4     	0.974402	0.974402	0.974402	0        
5  	3     	0.971341	0.943795	0.974402	0.00918197
Best individual is: {'kernel': 'rbf', 'C': 26.705081969400556}
with fitness: 0.9744017807456873
{'kernel': 'rbf', 'C': 26.705081969400556}
Accuracy:0.9744017807456873
```

KNN 实施:

```
*#KNN*
**from** evolutionary_search **import** EvolutionaryAlgorithmSearchCV
rf_params **=** {
    'n_neighbors': range(1,20),
}
clf **=** KNeighborsClassifier()
ga1 **=** EvolutionaryAlgorithmSearchCV(estimator**=**clf,
                                   params**=**rf_params,
                                   scoring**=**"accuracy",
                                   cv**=**3,
                                   verbose**=**1,
                                   population_size**=**10,
                                   gene_mutation_prob**=**0.10,
                                   gene_crossover_prob**=**0.5,
                                   tournament_size**=**3,
                                   generations_number**=**5,
                                   n_jobs**=**1)
ga1**.**fit(X, y)
print(ga1**.**best_params_)
print("Accuracy:"**+** str(ga1**.**best_score_))Types [1] and maxint [18] detected
--- Evolve in 19 possible combinations ---
gen	nevals	avg     	min     	max    	std       
0  	10    	0.955092	0.946578	0.96828	0.00705242
1  	7     	0.962994	0.951029	0.96828	0.00558289
2  	5     	0.967501	0.964385	0.96828	0.00155815
3  	8     	0.967891	0.964385	0.96828	0.00116861
4  	8     	0.966221	0.947691	0.96828	0.00617696
5  	6     	0.96828 	0.96828 	0.96828	0         
Best individual is: {'n_neighbors': 3}
with fitness: 0.9682804674457429
{'n_neighbors': 3}
Accuracy:0.9682804674457429
```

人工神经网络实施:

```
*#ANN*
**from** evolutionary_search **import** EvolutionaryAlgorithmSearchCV
*# Define the hyperparameter configuration space*
rf_params **=** {
    'optimizer': ['adam','rmsprop','sgd'],
    'activation': ['relu','tanh'],
    'batch_size': [16,32,64],
    'neurons':range(10,100),
    'epochs':[20,50],
    *#'epochs':[20,50,100,200],*
    'patience':range(3,20)
}
clf **=** KerasClassifier(build_fn**=**ANN, verbose**=**0)
*# Set the hyperparameters of GA* 
ga1 **=** EvolutionaryAlgorithmSearchCV(estimator**=**clf,
                                   params**=**rf_params,
                                   scoring**=**"accuracy",
                                   cv**=**3,
                                   verbose**=**1,
                                   population_size**=**10,
                                   gene_mutation_prob**=**0.10,
                                   gene_crossover_prob**=**0.5,
                                   tournament_size**=**3,
                                   generations_number**=**5,
                                   n_jobs**=**1)
ga1**.**fit(X, y)
print(ga1**.**best_params_)
print("Accuracy:"**+** str(ga1**.**best_score_))Types [1, 1, 1, 1, 1, 1] and maxint [1, 1, 2, 16, 2, 89] detected
--- Evolve in 55080 possible combinations ---
gen	nevals	avg     	min    	max     	std      
0  	10    	0.985031	0.93044	0.999444	0.0217448
1  	6     	0.998164	0.995548	0.999444	0.00174296
2  	4     	0.999444	0.999444	0.999444	1.11022e-16
3  	3     	0.999444	0.999444	0.999444	1.11022e-16
4  	6     	0.999444	0.999444	0.999444	1.11022e-16
5  	8     	0.999444	0.999444	0.999444	1.11022e-16
Best individual is: {'activation': 'relu', 'epochs': 50, 'optimizer': 'sgd', 'patience': 14, 'batch_size': 16, 'neurons': 64}
with fitness: 0.9994435169727324
{'activation': 'relu', 'epochs': 50, 'optimizer': 'sgd', 'patience': 14, 'batch_size': 16, 'neurons': 64}
Accuracy:0.9994435169727324
```

**信用:**

长度杨和 a .，“机器学习算法的超参数优化:理论与实践”，神经计算，第 415 卷，第 295–316 页，2020 年，

https://doi.org/10.1016/j.neucom.2020.07.061。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)