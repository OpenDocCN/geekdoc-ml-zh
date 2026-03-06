# 使用 statsmodels 组合来建模 Python 中所有可能的模型

> 原文：<https://medium.com/mlearning-ai/using-combinations-to-model-all-possible-models-in-python-with-pandas-statsmodels-969aee159fd0?source=collection_archive---------0----------------------->

![](img/16475c4725d0f5c33c420762e9573e3a.png)

我喜欢的关于数据科学管道的事情之一是建立模型。我发现这是最令人兴奋的部分之一——尝试新技术，开发新功能，从模型输出的内容中收集见解，最重要的是，利用所有这些来推动企业向更好的方向发展。简而言之，我热爱建筑模型。

然而，在过去的几个项目和 AutoML 的兴起中，我开始思考必须有一个更好的方法来做到这一点并避免黑箱。我仍然想建立模型并逆向理解它，但如果有更好的方法，我不想花一周(或更长时间)来建立模型。在散步(我的灵感时刻)之后，我决定使用组合作为我建模过程的一部分，这样我就可以花更少的时间来构建实际的模型，而花更多的时间来比较各种模型和结果。

在这个例子中，我已经获得了怀孕数据集，并使用它来建立一个回归模型，以了解哪些因素可以预测某人是否怀孕。我可以做我的分析，也许在我建立我的模型之前在数据上扔一个随机森林…或者我可以运行所有可能的模型，用直觉过滤哪些因素应该是积极的或消极的，找到我的理想模型。

首先，让我们导入包和数据

```
import pandas as pd
import statsmodels.formula.api as smfpreg_data = pd.read_csv(“C:\\Users\\bilal\\Desktop\\pregnancy_data.csv”)
```

接下来，我们想要创建一个列表，其中只包含我们想要建模的变量

```
vars_to_model = list(preg_data)cols_to_remove=[]cols_to_remove.append(‘PREGNANT’)vars_to_model= [item for item in vars_to_model if item not in cols_to_remove]
```

此后，使用 itertools 的组合让我们遍历每一个可能的模型。这可能需要很长时间，具体取决于您的机器。我用的是一台 10 年前的笔记本电脑，它有一个单核处理器，所以让它整夜运行(如果这台不能用了，我才会买一台新的！)

```
from itertools import combinationscombi = []
output = pd.DataFrame()modelNumber = 1for i in range(1,len(vars_to_model)):
 combi = (list(combinations(vars_to_model, i)))
 for c in combi:
 print('— — — — — — -')
 variable_string = ‘PREGNANT ~ 1 ‘
 var_iter =1
 for j in list(c):
 variable_string +=’ + ‘ + str(j)
 if var_iter == len(list(c)):
 try:
 result = smf.logit(formula= variable_string, data=preg_data).fit()
 coeffs = result.params
 coeffs = pd.DataFrame({‘Variable’:coeffs.index, ‘Values’:coeffs.values})
 predTable = result.pred_table()
 prsq = result.prsquared tp = predTable[1,1]
 tn = predTable[0,0]
 fp = predTable[0,1]
 fn = predTable[1,0]

 precision = tp/(tp+fp)
 recall = tp/(tp+fn)
 accuracy = (tp + tn)/(tp+tn+fp+fn) row1 = [{‘Variable’:’modelNumber’, ‘Values’:modelNumber}
 , {‘Variable’:’pRSQ’, ‘Values’:prsq}
 , {‘Variable’:’precision’, ‘Values’:precision}
 , {‘Variable’:’recall’, ‘Values’:recall}
 , {‘Variable’:’accuracy’, ‘Values’:accuracy}
 , {‘Variable’:’truepos’, ‘Values’:tp}
 , {‘Variable’:’trueneg’, ‘Values’:tn}
 , {‘Variable’:’falsepos’, ‘Values’:fp}
 , {‘Variable’:’falseneg’, ‘Values’:fn}
 , {‘Variable’:’variableString’, ‘Values’:variable_string}
 , {‘Variable’:’NumVars’, ‘Values’:len(c)} 
 ]
 coeffs = coeffs.append(row1, ignore_index=True)
 output= output.append(coeffs.set_index(‘Variable’).T) modelNumber += 1
 except :
 pass
 var_iter +=1
```

我们在上面的脚本中可以看到，我们用要建模的变量创建了一个字符串，然后运行这个模型。我们获取系数和一些模型统计数据，将它们附加到输出数据帧中，我们可以用它来找到模型

在处理完所有的模型之后，我们可以很容易地检查输出，并找到符合标准的一个或多个模型

```
out = output[(output[‘Maternity_Clothes’]>=0)
 &(output[‘Folic_Acid’]>=0)
 &(output[‘Pregnancy_Test’]>=0)
 &(output[‘Prenatal_Vitamins’]>=0)
 &(output[‘Stopped_buying_wine’]>=0)
 &(output[‘Body_Pillow’]>=0)
 &(output[‘accuracy’]>=0.75)]
```

在本例中，只有一个型号(型号 18045)符合标准。然后，您可以深入研究数据框架，并根据需要进一步过滤。

这就是创建每个可能的模型并查看它们各自的输出所需要做的全部工作。您可以修改代码，添加额外的指标或简化它。这不完全是我在项目中做的事情——我想和你分享最激动人心的部分。

我通常会从数据验证、EDA 开始，然后在建立模型之前确定我需要使用的变量。我还会将这些变量插入到代码中，创建所有可能的组合，然后过滤输出以节省时间。

让我知道你的想法，如果有更好的方法，请分享。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)