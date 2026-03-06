# 葡萄酒质量预测

> 原文：<https://medium.com/mlearning-ai/wine-quality-prediction-6da930eb6ba1?source=collection_archive---------4----------------------->

![](img/a6d60d405f6edddff546da859273af28.png)

photo by [Armands Brants](https://unsplash.com/@winephotos) on Unsplash

# 导入所需的模块

```
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.tree import DecisionTreeClassifier 
from sklearn.metrics import accuracy_score
from sklearn.naive_bayes import GaussianNB
import matplotlib.pyplot as plt
from sklearn.svm import SVC
import pandas as pd
import numpy as np
```

# 正在加载数据集

```
df = pd.read_csv(“datasets/winequality-red.csv”)
```

# 了解数据集

```
df.head()
```

![](img/6e1bd0eb5d60d3845b2a93abb8b35ed6.png)

```
df.info()
```

![](img/3d68c638da57cfdc883dfba6213da9f2.png)

# 形象化

```
plt.bar(‘quality’,’fixed acidity’,data = df)
```

![](img/16921b7f8121732ac3187a4d81eeded7.png)

```
plt.bar(‘quality’,’volatile acidity’,data = df)
```

![](img/bcda9c4a56e743c7bc14f60751b609c9.png)

```
plt.bar('quality','citric acid',data = df)
```

![](img/dbf689b6f38384ffe5b46b67be580f59.png)

```
plt.bar('quality','residual sugar',data = df)
```

![](img/a10c80f6702dd7645af801ca38c2a1c9.png)

```
plt.bar('quality','chlorides',data = df)
```

![](img/4a264b6c8f0eadb3c9f11e369bd633e4.png)

```
plt.bar('quality','free sulfur dioxide',data = df)
```

![](img/69730c4ee17e1a243adc41f71294793d.png)

```
plt.bar('quality','total sulfur dioxide',data = df)
```

![](img/edc9994f24eaa78f267a317bdd91b319.png)

```
plt.scatter('quality','density',data = df)
```

![](img/f4196bcdb770ec1d803ea1c862bece1e.png)

```
plt.bar('quality','pH',data = df)
```

![](img/ae932366c3927ec4ecee503d426a930e.png)

```
plt.bar('quality','sulphates',data = df)
```

![](img/9e996d4ae6d5b3ccb8b654eecf8cb5d3.png)

```
plt.bar('quality','alcohol',data = df)
```

![](img/75798a379cbf08d4234dab7c99157d0d.png)

```
plt.plot(df["quality"])
```

![](img/4eaa824dba9950ee362330d7a60359bf.png)

# 将葡萄酒质量分为好坏

```
#bins will set the limits for the classification.
bins = (3, 6, 8) #qualities ranging from 3–6 are classified as bad and 6–8 as good
group_names = [‘bad’, ‘good’]
df[“quality”] = pd.cut(df[“quality”], bins = bins, labels = group_names)
df["quality"].head()
```

![](img/953f2f973a6ac894affdacc22e354622.png)

```
#one hot-encoding
df[“quality”] = pd.get_dummies(df[“quality”],drop_first=True)
df[“quality”][:5]
```

![](img/fe6dd04afcf9c8f2dbb7ee6c58bff32c.png)

```
df[“quality”].value_counts()
```

![](img/ac5e04b4d1876ca84647c037e926d089.png)

# 将数据集分离为目标变量和特征变量

```
X = df.drop(“quality”, axis = 1)
y = df[‘quality’]
X.head()
```

![](img/785486b9beb7ba2c7c3d7e254f8e472f.png)

```
y.head()
```

![](img/8c2fba9eb9c97b575ab9985163e5df2e.png)

# 将数据集拆分为训练和测试数据

```
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 42)
```

将 y_train 转换为一维

```
y_train = np.ravel(y_train)
y_train
```

![](img/04b9e14cbd0e36f5e09812dddb7c5188.png)

应用标准缩放以获得优化结果

```
sc = StandardScaler()
X_train = sc.fit_transform(X_train)
X_test = sc.fit_transform(X_test)
```

# 创建模型

```
models = [ 'DecisionTreeClassifier', 'Support Vector Machine', 'GaussianNaiveBayes', 'KNeighborsClassifier', 'RandomForestClassifier']
accuracy_score_list = []
```

# 决策树分类器

```
dtc = DecisionTreeClassifier()
dtc.fit(X_train, y_train)
pred_dtc = dtc.predict(X_test)acc = accuracy_score(y_test, pred_dtc)
accuracy_score_list.append(acc)
print(acc)
```

![](img/ab76815f211ab207fdb2b330d7c66e72.png)

# 支持向量机

```
svm = SVC()
svm.fit(X_train, y_train)
pred_svm = svm.predict(X_test)acc = accuracy_score(y_test, pred_svm)
accuracy_score_list.append(acc)
print(acc)
```

![](img/a203363f41d2f81a91f12eb435b01504.png)

# 高斯贝叶斯

```
gnb = GaussianNB()
gnb.fit(X_train, y_train)
pred_gnb = gnb.predict(X_test)acc = accuracy_score(y_test, pred_gnb)
accuracy_score_list.append(acc)
print(acc)
```

![](img/d5e7d3074f540c8a07e349a18c2eba77.png)

# 近邻分类器

```
knn = KNeighborsClassifier(n_neighbors=22)
knn.fit(X_train, y_train)
pred_knn = knn.predict(X_test)acc = accuracy_score(y_test, pred_knn)
accuracy_score_list.append(acc)
print(acc)
```

![](img/f2dc38593c21151d4db0cfdee8a801ef.png)

# 随机森林分类器

```
rfc = RandomForestClassifier()
rfc.fit(X_train, y_train)
pred_rfc = rfc.predict(X_test)acc = accuracy_score(y_test, pred_rfc)
accuracy_score_list.append(acc)
print(acc)
```

![](img/4f606132f2e392a12559e1ef46ee1d79.png)

```
compare = pd.DataFrame({'Algorithms' : models , 'accuracy_score' : accuracy_score_list})
compare.sort_values(by='accuracy_score' ,ascending=False)
```

![](img/b37ff0eab67299f20d261dd5e8173fee.png)

```
plt.plot(compare['Algorithms'], compare['accuracy_score'], label = "accuracy_score")
```

![](img/781fdf33fd8cd98011c6eed86bd6e2a1.png)

# 使用随机森林分类器的最大准确度是 87%