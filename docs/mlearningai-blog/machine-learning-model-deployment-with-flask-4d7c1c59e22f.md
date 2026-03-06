# 使用 Flask 部署机器学习模型

> 原文：<https://medium.com/mlearning-ai/machine-learning-model-deployment-with-flask-4d7c1c59e22f?source=collection_archive---------2----------------------->

# **局部环境模型**

创建微调机器学习模型的过程包括加载数据源、数据准备、理解及其分析和模型开发，然后是超参数调整。所有这些都只存在于开发人员这一方。一般来说，在一个典型的场景中，机器学习模型往往会永远存在于我们的机器上。但是为了让终端用户可以使用和体验，我们部署了我们的机器学习模型。从此，烧瓶出现在画面中。

**内容**

*   瓶
*   安装烧瓶
*   问题陈述
*   代码结构
*   网页
*   结束注释

# **烧瓶**

Flask 是一个最初用 Python 编写的 web 框架。它完全是通过 Python 开发的，用于实现基于 Web 的应用程序。在机器学习场景中，Flask 是在网页上部署模型时最重要的部分。

# 安装烧瓶

目前我正在使用我的 Windows 64 位操作系统来设置 Flask。执行一个命令就可以安装 Flask。就这么简单！

```
$ pip install Flask
```

为了在 Ubuntu 上设置；

```
sudo apt-get install python3-flask
```

现在，为了确保 Flask 的安装是正确的，在命令提示符/终端中执行下面的命令。

```
C:\Users\Aryan>Flask --version
Python 3.9.2
Flask 1.1.2
```

您将在终端上看到如上所示的内容。确保使用与完全创建模型相同的 python 解释器来运行 Flask 应用程序。

# **问题陈述**

我目前考虑的数据集是心力衰竭预测数据集。你可以在这里看看详情[！粗略地说一下我们的数据集，我们有 12 个特征作为我们机器学习模型的输入——年龄、贫血、肌酐磷酸激酶、糖尿病、射血分数、高血压、血小板、血清肌酐、血清钠、性别、吸烟和时间。目标变量是死亡事件(0/1)。](https://archive.ics.uci.edu/ml/datasets/Heart+failure+clinical+records)

因此，一旦我们创建了预测目标变量的机器学习模型，下一步就是将我们的模型权重保存在。pkl 格式，为此我们使用 joblib/pickle 库。

```
from sklearn.externals import joblib# Saving
joblib.dump(prediction_model, ‘modelname.pkl’)# Loading
model = joblib.load(‘modelname.pkl’)# Prediction
model.predict(x_test)
```

根据数据集上使用的模型数量设置此代码。我在这边做基于树的分类器。

下一步是创建一个网页，从客户端获取输入，然后输入到我们的模型中。

# 代码结构

我们现在最重要的焦点是精确设置后端！烧瓶装置的结构如下——

1.  在现有工作目录下创建一个文件夹**“Templates”**
2.  在 Templates 文件夹中添加两个 HTML 文件，名为**“Home”**，用于主页接收用户输入和“Output”以显示预测消息。
3.  回到主工作目录，添加名为**“app”**的 Python 文件，这里是整个功能发生的地方。你可以把它看作是整个后端:D 的控制室

# 网页

给定的代码实现了一个基本的网页，用于设置输入字段及其相应的功能名称。

*   【Home.html 号

```
<html>
 <body>
  <center><h1> HEART ATTACK PREDICTION </h1>
   <body background=''><form method="POST", action="{{url_for('home')}}">
     <b> Age : <input type="number", name='a', placeholder='Age'><br><br>
     <b> Anaemia : <input type="number", name='b', placeholder='Anaemia'><br><br>
     <b> Creatinine Phosphokinase : <input type="number", name='c', placeholder='Creatinine Phosphokinase'><br><br>
     <b> Diabetes : <input type="number", name='d', placeholder='Diabetes'><br><br>
     <b> Ejection Fraction : <input type="number", name='e', placeholder='Ejection Fraction'><br><br>
     <b> High Blood Pressure : <input type="number", name='f', placeholder='High Blood Pressure'><br><br>
     <b> Platelets : <input type="number", name='g', placeholder='Platelets'><br><br>
     <b> Serum Creatinine : <input type="number", name='h', placeholder='Serum Creatinine'><br><br>
     <b> Serum Sodium : <input type="number", name='i', placeholder='Serum Sodium'><br><br>
     <b> Gender : <input type="number", name='j', placeholder='Gender'><br><br>
     <b> Smoking : <input type="number", name='k', placeholder='Smoking'><br><br>
     <b> Time : <input type="number", name='l', placeholder='Time'><br><br>
     <input type="submit", value='pred'>
    </b>
   </form> </center>
 </body>
</html>
```

现在我们继续创建预测页面，我们的模型根据从用户端接收的输入显示输出消息。

*   【Output.html 

```
<html> <body> <center><h1> PREDICTION </h1>
   <br>
   <br>
   <br>
   {%if data==0%}
   <h2> You have not encountered a heart attack </h2> {{pred}}{%else%}
   <h2> You need to look for doctors because you have encountered a Heart Attack</h2>
   <br>
   <br>
   <br>{%endif%}
   <h6> Thank You </h6> </center> </body>

</html>
```

目标变量属于分类类型。因此该模型只能以二进制格式进行预测。根据预测，我们设置了 if-else 块。

为了使这两个文件同步并按照模型输出正常工作，我们创建了一个称为 Flask 控制室的东西(好吧，我这样称呼它:P ),整个流连接在这里发生！转到您的工作目录，在 app.py 中编写以下代码。

*   **app.py**

```
# Importing necessary modules
from flask import Flask, render_template, request
import pickle
import numpy as np# Loading the Models
dclf_heart = pickle.load(open('dclf.pkl', 'rb'))
xrclf_heart = pickle.load(open('xrclf.pkl', 'rb'))
xclf_heart = pickle.load(open('xclf.pkl', 'rb'))# Instantiate app object 
app = Flask(__name__)# Setting up app routes [@app](http://twitter.com/app).route('/')
def man(): 
 return render_template('Home.html')[@app](http://twitter.com/app).route('/predict', methods=['POST'])
def home():
 feature1 = int(request.form['a'])
 feature2 = int(request.form['b'])
 feature3 = int(request.form['c'])
 feature4 = int(request.form['d'])
 feature5 = int(request.form['e'])
 feature6 = int(request.form['f'])
 feature7 = int(request.form['g'])
 feature8 = int(request.form['h'])
 feature9 = int(request.form['i'])
 feature10 = int(request.form['j'])
 feature11 = int(request.form['k'])
 feature12 = int(request.form['l']) arr = np.array([[feature1, feature2, feature3, feature5, feature6, feature7, feature8, feature9, feature11, feature12]])
 pred1 = dclf_heart.predict(arr)
 pred2 = xrclf_heart.predict(arr)
 pred3 = xclf_heart.predict(arr)final_pred = [pred1, pred2, pred3]
 if final_pred.count(1) > final_pred.count(0):
  pred = 1
 else:
  pred = 0return render_template('Output.html', data=pred)# Flask server
if __name__=="__main__":
 app.run(debug=True)
```

这在一开始可能会令人生畏，但让我们一次一个地分解它…

1.  最初的几行是我们导入模块的地方。
2.  使用 pickle 模块，我们将读取二进制格式下的模型分配给 dclf_heart、xrclf_heart、xclf_heart。(这些是我们的模型所在的 pickle 文件，为了便于理解，将它们视为模型)
3.  我们为要运行的代码实例化 app 对象，认为这是后端代码库的核心。
4.  下一步是指定应用程序路由来连接我们之前创建的网页:)第一个路由在 Templates 文件夹下呈现我们的主页，下一个路由呈现预测页面。
5.  在第二个应用程序路径中，从 Home.html 以整数格式(主要是因为我的数据集的所有要素都处理整数)请求值(从用户端通过网页提供给模型的输入)，并将其分配给变量 feature1、feature2、feature3 等。
6.  在这里，我们的工作完成得非常完美！但如前所述，我使用了 3 个基于树的分类器，为了获得所需的输出，我使用了聚合器功能，该功能主要基于多数投票来选择输出。代码非常简单，易于理解。if-else 块的基本实现。
7.  调用 run 函数在本地启动 Flask 服务器。最后一行是我们的后台！！！！应用程序在默认参数设置为 True 的情况下运行。

**Flask 服务器**接收来自该端的请求并获取其响应，其中来自用户端的输入被接收并馈入模型。

# **结束注释**

这就是后端使用 Flask 加载和部署**机器学习模型的工作方式。根据处理的数据，开发人员可以设计网页前端。但是说到后端，最初可能有点难以理解它是如何以及为什么以这种方式运行的，但是随着时间的推移，事情会变得清晰并且更容易理解！**

这是**“如何使用 Flask 部署机器学习模型”**，我希望这个过程清晰简洁，并附有评论和解释。

如果你喜欢我写的东西，一定要看看我的其他博客，我很想知道你对此的看法，谢谢:)