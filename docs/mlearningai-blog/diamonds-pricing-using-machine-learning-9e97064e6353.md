# 使用机器学习的钻石定价

> 原文：<https://medium.com/mlearning-ai/diamonds-pricing-using-machine-learning-9e97064e6353?source=collection_archive---------2----------------------->

![](img/14220ba9eb2cb2be50dc36eab7864a64.png)

photo by [Edgar Soto](https://unsplash.com/@edgardo1987) unsplash

# 正在导入所需的库。

```
from sklearn.ensemble import RandomForestRegressor, AdaBoostRegressor, ExtraTreesRegressor, GradientBoostingRegressor
from sklearn.metrics import mean_squared_error, r2_score,mean_absolute_error 
from sklearn.model_selection import cross_val_score
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt
from sklearn import ensemble
import sklearn.naive_bayes
import seaborn as sns
import pandas as pd
import sklearn
```

# 加载数据集

```
data_path= 'datasets/diamonds.csv'
diamonds_org = pd.read_csv(data_path)
diamonds_org.head()
```

![](img/62ab6a829c503088e2c4448ac804e76f.png)

# 检查 Nan 值

```
diamonds_org.isnull().sum()
```

![](img/fec5583de1bb26ba57b1afa5c655e1fb.png)

# 检查每列中的唯一值

```
diamonds_org[‘carat’].unique()
```

![](img/74685c9ebf3ea41b84a555394768d350.png)

```
diamonds_org[‘clarity’].unique()
```

![](img/5e81764275b7214749f2312f7406449c.png)

```
diamonds_org['cut'].unique()
```

![](img/92e021e2d1952120d0553bf98a4f1146.png)

```
diamonds_org['depth'].unique()
```

![](img/5b7c11d844e3b5b5bef223cae0014125.png)

```
diamonds_org['table'].unique()
```

![](img/3dad3ab472a83e77ec1bd75be1f44bff.png)

```
diamonds_org.loc[(diamonds_org[‘x’]<=0) | (diamonds_org[‘y’]<=0) | (diamonds_org[‘z’]<=0)]
```

![](img/785f344617b7e8d0effc8d793da10eb8.png)

长度、宽度或高度中的任何一个为零都没有任何意义。但是我们可以看到有些行的值为“0”。因此，让我们从数据框中移除这些行。

```
len(diamonds_org[(diamonds_org['x']<=0) | (diamonds_org['y']<=0) | (diamonds_org['z']<=0)])
```

产量:20

```
diamonds_org = diamonds_org[(diamonds_org[[‘x’,’y’,’z’]] != 0).all(axis=1)]
```

![](img/3a5dd4fe497a72d8deb6bba7006d79bc.png)

从上面的数据帧中，我们可以看到它包含以下特征:

```
1\. carat
2\. cut
3\. color
4\. depth 
5\. table 
6\. price 
7\. x (length in mm) 
8\. y (width in mm) 
9\. z (height in mm)
```

7，8，9 个属性并合并成单个属性为 volume，为 volume = x *y* z，做法如下。

```
diamonds_org[‘volume’] = diamonds_org[‘x’]*diamonds_org[‘y’]*diamonds_org[‘z’]
diamonds_org[:5]
```

![](img/24bdf8ccd9a4f57ef96d20340e8413d0.png)

现在不需要 x、y、z，因为它们被合并到单个柱体积中。所以我们要删除这些列。

```
diamonds_org = diamonds_org.drop(['x', 'y', 'z'], axis=1)
diamonds_org[:5]
```

![](img/91178e3f861ae739cdb40abdc5e7eadd.png)

# 特征之间的相关性

```
sns.set(rc={'figure.figsize':(6,6)})
corr = diamonds_org.corr()
sns.heatmap(data=corr, square=True , annot=True, cbar=True, cmap="YlGnBu")
```

![](img/13d7e113727af8c954fb467131dd658a.png)

从上图得出的结论:

1.  深度与价格成反比。
2.  钻石的价格与克拉及其尺寸密切相关。
3.  钻石的重量(克拉)对其价格影响最大。
4.  成交量似乎与价格甚至彼此高度相关。
5.  自我关系。一个特性的值是 1。
6.  还可以得出其他一些推论。

# 所有功能的可视化

# 克拉

```
sns.kdeplot(diamonds_org['carat'], shade=True , color='r')
```

![](img/bd8e072112aa00317970299c47851078.png)

# 克拉与价格

```
sns.lineplot(diamonds_org[‘carat’], diamonds_org[‘price’], color=’blue’)
```

![](img/b17859af70432477ee748b93f690a23b.png)

# 切口

```
sns.factorplot(x='cut', data=diamonds_org , kind='count',aspect=2.5 )
```

![](img/8ecfd460feaeb66b0fda778a4c9ecb37.png)

# 削减与价格

```
sns.lineplot(diamonds_org['cut'], diamonds_org['price'], color='orange')
```

![](img/e3d118acab6ac0dfa0b5ce8c07e78833.png)

# 颜色

```
sns.factorplot(x=’color’, data=diamonds_org , kind=’count’,aspect=2.5 )
```

![](img/76fa12ea0a1b46aaaf6158aa246fccad.png)

# 颜色与价格

```
sns.lineplot(diamonds_org['color'], diamonds_org['price'], color='gray')
```

![](img/346fe7b7658f6b52fa6910de4e634fe6.png)

# 清楚

```
sns.factorplot(x='clarity', data=diamonds_org , kind='count',aspect=2.5 )
```

![](img/912c19b1611e5ee2175bfb88a43357c9.png)

# 透明度与价格

```
sns.lineplot(diamonds_org['clarity'], diamonds_org['price'], color='yellow')
```

![](img/64988112bc8acde5599a97baadb68341.png)

# 深度

```
plt.hist('depth' , data=diamonds_org , bins=25)
```

![](img/b88b3900b067bf8aa656e6f480d729ff.png)

# 深度与价格

```
sns.lineplot(diamonds_org['depth'], diamonds_org['price'], color='red')
```

![](img/ede9d697d78d070606663831b785b17a.png)

# 桌子

```
sns.kdeplot(diamonds_org[‘table’] ,shade=True , color=’orange’)
```

![](img/f76128a4e010639c406bb24d74c48be0.png)

# 桌子与价格

```
sns.lineplot(diamonds_org['table'], diamonds_org['price'], color='green')
```

![](img/aed9bc5ddec985ea022ba2f865c7259e.png)

# 卷

```
plt.figure(figsize=(5,5))
plt.hist( x=diamonds_org['volume'] , bins=30)
plt.xlim(0,800)
plt.ylim(0,50000)
```

![](img/c62126363024407603d0c0962302d8e5.png)

# 数量与价格

```
sns.lineplot(diamonds_org[‘volume’], diamonds_org[‘price’])
```

![](img/f802002441c2dd5c10a695d2722123b1.png)

# 特征编码

检查要素的数据类型

```
diamonds_org.dtypes
```

![](img/365dfed8f32ff3dc8d7e7a18b5c4b93f.png)

“剪切”的一键编码

```
dummy_cut = pd.get_dummies(diamonds_org['cut'], prefix = 'cut' , drop_first = True)
dummy_cut.head()
```

![](img/119f03203dc2a77fac2243f0e3395c6c.png)

```
print(diamonds_org.shape)
print(dummy_cut.shape)
```

![](img/f73bdd511659663794082abb8fd84962.png)

连接 dummy_cut 和 diamonds_org 数据框

```
X = pd.concat([diamonds_org, dummy_cut], axis=1)
print(X.shape)
print(X[:3])
```

![](img/00830562e3bd1cd252e7175f6b862bad.png)

删除已编码的剪切列

```
X = X.drop([‘cut’], axis=1)
print(X.shape)
X[:3]
```

![](img/00830562e3bd1cd252e7175f6b862bad.png)

彩色一键编码

```
dummy_color = pd.get_dummies(X['color'], prefix = 'color' , drop_first = True)print(dummy_color.shape)
print(X.shape)
```

![](img/a3a229ab45d8f1778608322f80577af9.png)

连接 X 和 dummy_color 数据帧，并删除颜色列

```
X = pd.concat([X, dummy_color], axis=1)
X = X.drop(['color'], axis=1)
print(X.shape)X[:3]
```

![](img/680de45e1570773633d0ca2dc35c65c1.png)

```
dummy_clarity = pd.get_dummies(X[‘clarity’], prefix = ‘clarity’ , 
 drop_first = True)
print(dummy_clarity.shape)
print(X.shape)X = pd.concat([X, dummy_clarity], axis=1)
X = X.drop([‘clarity’], axis=1)
X[:5]
```

![](img/881cd52adece64918c7ec38e3c379551.png)

# 特征缩放

将数据分成 X，y

```
y = X['price']
X = X.drop(['price'], axis=1)
```

![](img/e8ce1166f73790f8b1579ecd4df0bfab.png)

将数据分为训练和测试数据

```
X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.2, random_state=66)
```

# 建模算法

```
models = [‘RandomForestRegressor’, ‘AdaBoostRegressor’, ‘ExtraTreesRegressor’, ‘GradientBoostingRegressor’, ‘LinearRegression’]
r2_scores = []
```

# 随机森林回归量

```
model_RF =  RandomForestRegressor()
model_RF.fit(X_train , y_train)y_pred = model_RF.predict(X_test)
accuracies = cross_val_score(estimator = model_RF, X = X_train, y = y_train, cv = 5,verbose = 1)
print('')
print('###### RandomForestRegressor ######')
print('Score : %.4f' % model_RF.score(X_test, y_test))
print(accuracies)mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)print('')
print('MSE    : %0.2f ' % mse)
print('MAE    : %0.2f ' % mae)
print('R2     : %0.2f ' % r2)
r2_scores.append(r2)
```

![](img/af3a462f8f0135337866fcd2c4a53722.png)

# AdaBoostRegressor

```
model_ABR =  AdaBoostRegressor()
model_ABR.fit(X_train , y_train)y_pred = model_ABR.predict(X_test)
accuracies = cross_val_score(estimator = model_ABR, X = X_train, y = y_train, cv = 5,verbose = 1)
print('')
print('###### AdaBoostRegressor ######')
print('Score : %.4f' % model_ABR.score(X_test, y_test))
print(accuracies)mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)print('')
print('MSE    : %0.2f ' % mse)
print('MAE    : %0.2f ' % mae)
print('R2     : %0.2f ' % r2)
r2_scores.append(r2)
```

![](img/27963246431b35f70d7113de807097ea.png)

# 树外回归因子

```
model_ETR =  ExtraTreesRegressor()
model_ETR.fit(X_train , y_train)y_pred = model_ETR.predict(X_test)
accuracies = cross_val_score(estimator = model_ETR, X = X_train, y = y_train, cv = 5,verbose = 1)
print('')
print('###### ExtraTreesRegressor ######')
print('Score : %.4f' % model_ETR.score(X_test, y_test))
print(accuracies)mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)print('')
print('MSE    : %0.2f ' % mse)
print('MAE    : %0.2f ' % mae)
print('R2     : %0.2f ' % r2)
r2_scores.append(r2)
```

![](img/ee2abb0e7e565dbe3d1e6fbccbfe5816.png)

# 梯度推进回归器

```
model_GBR =  GradientBoostingRegressor()
model_GBR.fit(X_train , y_train)y_pred = model_GBR.predict(X_test)
accuracies = cross_val_score(estimator = model_GBR, X = X_train, y = y_train, cv = 5,verbose = 1)
print('')
print('###### GradientBoostingRegressor ######')
print('Score : %.4f' % model_GBR.score(X_test, y_test))
print(accuracies)mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)print('')
print('MSE    : %0.2f ' % mse)
print('MAE    : %0.2f ' % mae)
print('R2     : %0.2f ' % r2)
r2_scores.append(r2)
```

![](img/22a82651596a5d0dd2e8321a13bef371.png)

# 线性回归

```
model_LR =  LinearRegression()
model_LR.fit(X_train , y_train)y_pred = model_LR.predict(X_test)
accuracies = cross_val_score(estimator = model_LR, X = X_train, y = y_train, cv = 5,verbose = 1)
print('')
print('###### LinearRegression ######')
print('Score : %.4f' % model_LR.score(X_test, y_test))
print(accuracies)mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)print('')
print('MSE    : %0.2f ' % mse)
print('MAE    : %0.2f ' % mae)
print('R2     : %0.2f ' % r2)
r2_scores.append(r2)
```

![](img/3745973db046a9ff236e5a71239f2097.png)

# 可视化算法的 R2 分数

```
compare = pd.DataFrame({‘Algorithms’ : models , ‘R2-Scores’ : r2_scores})
compare.sort_values(by=’R2-Scores’ ,ascending=False)
```

![](img/d7289a974bcac31c63ebbc99bc35b3f8.png)

```
plt.plot(compare['Algorithms'], compare['R2-Scores'], label = "R2-Score")
```

![](img/b828f6fec50e989b2cf96078b05ffe54.png)

```
sns.barplot(x='R2-Scores' , y='Algorithms' , data=compare)
```

![](img/57347675de475dbe4f1c6a81aeace97a.png)

# 额外的树回归，因为它具有最高的 r2 分数(98%)