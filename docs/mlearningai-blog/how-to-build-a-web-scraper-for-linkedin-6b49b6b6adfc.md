# 如何为 LinkedIn 建立一个网页抓取器

> 原文：<https://medium.com/mlearning-ai/how-to-build-a-web-scraper-for-linkedin-6b49b6b6adfc?source=collection_archive---------0----------------------->

![](img/5133bae5994d3c8e1d9f0bd580212f17.png)

Photo by [Greg Bulla](https://unsplash.com/@gregbulla) on [Upslash](https://unsplash.com/)

构建机器学习算法来获取和分析 Linkedin 数据是 ML 爱好者中的一个流行想法。但每个人都遇到的一个障碍是缺乏数据，考虑到从 Linkedin 收集数据是多么乏味以及这一切背后的合法性。

美国第九巡回上诉法院裁定，CFAA 不禁止公司收集互联网上公开的数据，尽管 LinkedIn 声称这侵犯了用户隐私。

总部位于旧金山的初创企业 hiQ Labs 从 LinkedIn 收集用户资料，并利用这些资料分析劳动力数据，例如预测员工何时可能离职，或者哪里可能出现技能短缺。

在 LinkedIn 采取措施阻止 hiQ 这么做后，hiQ 在两年前赢得了强制令，迫使微软所有的公司取消了封锁。该禁令现已被美国第九巡回上诉法院以 3 比 0 的裁决维持。

在本文中，我们将讨论如何使用 Selenium 和 Beautiful Soup 构建并自动化您自己的 Web 抓取工具，从任何 Linkedin 个人资料中提取数据。这是一个分步指南，每个步骤都有完整的代码片段。对于那些想要完整代码的人，我在文章末尾添加了 GitHub 资源库链接。

# 要求

*   python(显然。3+推荐)
*   美汤(美汤是一个库，可以很容易的从网页中抓取信息。)
*   selenium(selenium 包用于自动化 Python 中的 web 浏览器交互。)
*   一个网络驱动，我用过 Chrome 的网络驱动。
*   此外，您还需要 pandas、time 和 regex 库。
*   一个代码编辑器，我用的是 Jupyter Notebook，你可以用 Vscode/Atom/Sublime 或者任何你选择的。

> *使用* `*pip install selenium*` *安装硒库。*
> 
> *使用* `*pip install beautifulsoup4*` *安装美汤库。*
> 
> *你可以从这里下载 Chrome 网络驱动:*
> 
> [*https://chromedriver.chromium.org/*](https://chromedriver.chromium.org/)

# 概观

这里是本文涵盖的所有主题的完整列表。

*   如何使用 Selenium 实现 Linkedin 自动化？
*   如何使用 BeautifulSoup 从给定的 Linkedin 个人资料中提取帖子及其作者。
*   将数据写入. csv 或。xlsx 文件可供以后使用。
*   如何自动处理一次获得多个作者的帖子。

# 说完这些，让我们开始吧

*   我们将从进口我们需要的一切开始。

```
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium import webdriver
from bs4 import BeautifulSoup as bs
import re as re
import time
import pandas as pd
```

*   除了 selenium 和 beautiful soup 之外，我们还将使用 regex 库来获取帖子的作者，使用 time 库来使用 sleep 和 pandas 等时间功能来处理大规模数据，以及写入电子表格。马上会有更多的报道！
*   Selenium 需要一些东西来启动自动化过程。系统中 web 驱动程序的位置、用户名和登录密码。因此，让我们从获取它们开始，并分别将它们存储在变量 PATH、USERNAME 和 PASSWORD 中。

```
PATH = input("Enter the Webdriver path: ")
USERNAME = input("Enter the username: ")
PASSWORD = input("Enter the password: ")
print(PATH)
print(USERNAME)
print(PASSWORD)
```

*   现在我们将在一个变量中初始化我们的 web 驱动程序，Selenium 将用它来执行所有的操作。姑且称之为司机吧。我们告诉它网络驱动的位置，即路径

```
driver = webdriver.Chrome(PATH)
```

*   接下来，我们将告诉我们的驱动程序它应该获取的链接。在我们的例子中，它是 Linkedin 的主页。

```
driver.get("[https://www.linkedin.com/uas/login](https://www.linkedin.com/uas/login)")
time.sleep(3)
```

> 你会注意到，我在上面的代码片段中使用了睡眠功能。你会发现它在本文中用得很多。sleep 函数基本上将任何进程(在我们的例子中是自动化进程)暂停指定的秒数。如果你需要绕过验证码验证，你可以随意使用它在任何你喜欢的地方暂停这个过程。

*   我们现在将告诉驱动程序使用我们提供的凭据登录。

```
email=driver.find_element_by_id("username")
email.send_keys(USERNAME)
password=driver.find_element_by_id("password")
password.send_keys(PASSWORD)
time.sleep(3)
password.send_keys(Keys.RETURN)
```

*   现在让我们创建几个列表来存储数据，如个人资料链接、文章内容和每篇文章的作者。我们将分别称它们为 post_links、post_texts 和 post_names。
*   一旦完成，我们将开始实际的网页抓取过程。让我们声明一个函数，这样我们就可以使用我们的 web 抓取代码，以递归方式从多个帐户获取帖子。我们就叫它 Scrape_func 吧。

好吧，这个函数很长。不要担心！我会一步步解释。让我们先来看看我们的函数是做什么的。

*   它有三个参数，即 post_links、post_texts 和 post_names，分别为 a、b 和 c。
*   现在我们将进入函数的内部工作。它首先获取概要文件链接，并截取概要文件名称。
*   现在，我们使用驱动程序来获取用户配置文件的“文章”部分。驱动程序滚动浏览帖子，使用 beautiful soup 收集帖子的数据，并将其存储在“容器”中。
*   上述代码的第 17 行规定了驱动程序收集帖子的时间。在我们的例子中，它是 20 秒，但是您可以更改它以适应您的数据需求。

```
if round(end-start)>20:        
    breakexcept:
    pass
```

*   我们还从每个帐户获取用户想要的帖子数量，并将其存储在变量“nos”中。
*   最后，我们遍历每个“容器”，获取存储在其中的文章数据，并将其与 post_names 一起添加到 post_texts 列表中。当达到期望的帖子数量时，我们会中断循环。

> *你会注意到，我们在一个 try-catch 块中包含了容器迭代循环。这是针对可能出现的异常的安全措施。*

*   这就是我们的功能！现在，是时候使用我们的函数了！我们从用户那里获得一个配置文件列表，并以递归方式将其发送给函数，以重复所有帐户的数据收集过程。
*   该函数返回两个列表:包含所有帖子数据的 post_texts 和包含所有相应帖子作者的 post_names。
*   现在我们已经到了自动化最重要的部分:保存数据！

```
data = {
    "Name": post_names,
    "Content": post_texts,
}df = pd.DataFrame(data)
df.to_csv("gtesting2.csv", encoding='utf-8', index=False)writer = pd.ExcelWriter("gtesting2.xlsx", engine='xlsxwriter')
df.to_excel(writer, index =False)
writer.save()
```

*   我们用该函数返回的列表创建一个字典，并使用 pandas 将它保存到一个变量' df '中。
*   您可以选择将收集的数据保存为. csv 文件或. xlsx 文件。

# 对于 csv:

```
df.to_csv("test1.csv", encoding='utf-8', index=False)
```

# 对于 xlsx:

```
writer = pd.ExcelWriter("test1.xlsx", engine='xlsxwriter')
df.to_excel(writer, index =False)
writer.save()
```

> 在上面的代码片段中，我给出了一个示例文件名‘text 1’。你可以给任何你选择的文件名！

# 结论:

唷！那是很长的时间，但是我们已经成功地创建了一个全自动的网络抓取器，它可以让你在一瞬间获得 Linkedin 帖子的数据！希望这对你有帮助！以后一定要关注更多这样的文章！下面是 GitHub 上完整代码的链接:【https://github.com/AhmedKNegm/Scrape_func

谢谢你的来访！快乐学习！