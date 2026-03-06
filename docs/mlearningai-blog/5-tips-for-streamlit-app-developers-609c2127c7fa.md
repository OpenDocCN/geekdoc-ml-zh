# Streamlit 应用程序开发人员的 5 个技巧

> 原文：<https://medium.com/mlearning-ai/5-tips-for-streamlit-app-developers-609c2127c7fa?source=collection_archive---------7----------------------->

![](img/71b2205994f191f2c153f76e8ed48e86.png)

Photo by [Antonio Janeski](https://unsplash.com/@janesky?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Streamlit 是一个流行的 pythonic web 应用程序开发框架，最近已经使 web 应用程序开发民主化。使用几行代码就可以开发一个 web 应用程序。它是开发数据密集型应用程序进行快速测试和原型制作的有力工具。任何人都可以使用 streamlit 文档页面上非常常用的功能。

为了开发一个更好的用户友好的 web 应用程序，经常需要使用自定义函数。我总结了 5 个有价值的功能/技巧来制作一个更好的 streamlit 应用程序。我觉得这些功能对其他开发者来说会很方便。

# 1.配置:

Streamlit 以默认的标题和图标开始。然而，我对默认的标题和页面标志/图标不满意。我想把我的文字和标志放在我的应用程序中。Streamlit config 是我定义页面布局、标题和徽标的地方。以下代码片段显示了如何根据您的需要配置您的 web 应用程序。在这里，我使用我的图像作为 streamlit 应用程序图标的图标。

# 2.菜单条

我的 streamlit 应用是一个多页面应用。尽管最新版本的 streamlit 支持多页面选项，但我发现了一个更好的包来实现这一点。 [streamlit_option_menu](https://github.com/victoryhb/streamlit-option-menu) 包优雅地组织了多页选项。我可以很容易地添加左侧或顶部或两侧的菜单选项。使用引导图标为每个页面合并不同的图标也很容易。我已经安装了这个软件包' pip 安装流线-选项-菜单'。下面的代码片段显示了将侧边栏菜单和菜单放在顶部的相关代码。

# 3.简化页面背景

在开发了一个工作的 streamlit 应用程序后，我想通过为侧面板和主面板提供两个不同的背景图像来改变应用程序的背景。以下代码片段显示了更改我的 streamlit 应用程序的背景所需的代码。它从我的电脑中取出一张图片用于左侧面板，从 Unsplash 中取出另一张图片用于主面板的背景。

# **4。下载 CSV 文件**

虽然这个函数可以直接从 streamlit 文档中获得，但是我添加了这个函数来最小化所需的代码。这个函数将只把 pandas 数据报作为输入，并在单击 download 按钮时将这些输入数据下载为一个 CSV 文件。

# 5.移除默认简化菜单

开发过程中，看到自己 app 的右侧菜单很烦。我觉得用户也会烦。因此，我决定将它从我的应用程序中删除。下面的代码片段将删除 streamlit 应用程序中可用的默认文本。

# **最后备注:**

我希望所报道的功能将对其他 streamlit 应用程序开发人员的下一个激动人心的 streamlit 项目有用。我也感谢其他 streamlit 贡献者，我从他们那里学到了很多。

# 关于作者

我是加州一家软件开发公司的高级数据科学家。技术写作是我的新爱好。

非常感谢您到目前为止阅读这篇文章。非常感谢您的评论和订阅。如果你想知道我在写什么，请关注我的个人资料。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)