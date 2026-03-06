# 与 Jupyter 一起启动 Plotly Dash

> 原文：<https://medium.com/mlearning-ai/launching-plotly-dash-with-jupyter-83dd6b0a55eb?source=collection_archive---------4----------------------->

![](img/74cc40d98c52873a30461c1502ce6775.png)

Launching PlotlyDash App with Jupyter Lab

5 **—用 Jupyter 快速启动你的 Plotly Dash 应用程序。**

**1 —创建一个项目目录。**

*   打开 finder 并创建一个您想要存储项目的文件夹。

**2 —从此目录打开终端。**

*   右键单击文件夹。向下滚动到服务，然后单击文件夹中的*新建终端。*

**3 —创建并激活您的虚拟环境。**

*   在您的终端中键入下面的代码。注意，您可以通过用 *NewEnvironment* 替换它来定义您的环境名称。

```
python -m venv NewEnvironment
```

**4 —安装所需的库并启动 Jupyter**

*   在终端中键入以下内容，安装所需的库。

```
pip install pandas
pip install plotly
pip install dash
pip install jupyter lab#now launch Jupyter Lab
jupyter lab
```

**5 —运行您的 Dash 应用程序**

*   Jupyter Lab 在您的默认浏览器中启动后，复制并粘贴您的应用程序代码，然后执行或使用下面的演示代码。

*   **注意:**使用以下代码更改您的应用程序在笔记本中的显示方式。

```
# view your app on its own browser page 
app.run_server(mode='external')# view your app on its own jupyter lab tab
app.run_server(mode='jupyterlab')# view your app below your code
app.run_server(mode='inline'
```

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)