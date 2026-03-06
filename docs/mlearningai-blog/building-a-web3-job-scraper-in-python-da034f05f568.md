# 用 python 构建 web3 作业抓取器。

> 原文：<https://medium.com/mlearning-ai/building-a-web3-job-scraper-in-python-da034f05f568?source=collection_archive---------5----------------------->

![](img/6a7e6eb06fba8c04db79f65b2bca68aa.png)

Photo by [Shubham Dhage](https://unsplash.com/@theshubhamdhage?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Web3 是最受欢迎的新词之一，我猜是最近要进入的行业。我曾在家族理财室的 it 部门工作过，觉得参与其中很有收获。

不幸的是，我目前失业了，因为我所在的家族办公室不适合我。因此，作为一名自学成才的程序员和黑客，我认为尝试建立一个 web3 工作抓取器是一个好主意，因为这将允许我以更快的方式访问工作，并找到最适合我的行业和专业知识的工作。

这里有一个到 Github 回购的链接。

我要完全归功于这个项目的原创者，PythonEatsSquirrel。确实为谁创造了工作机会。我修改了这段代码，让它为我工作，允许我从一个定制的网站上抓取工作，在这个例子中，这个网站是 web3 jobs。然而，如果你想要原始视频的链接，[给你。](https://www.youtube.com/watch?v=070e7nMYt6c)

在经历这个过程时，要记住的主要事情是提取、转换和学习。

这个项目使用的外部库是:
-beautiful soup
-Pandas
-Requests

我们想从创建一个名为 extract 的函数开始，它接受一个名为 page 的参数。

好的，首先，你要收集你抓取的页面的 URL，但是把它转换成 f 字符串，这样你就可以遍历你需要抓取的不同页面。
您还想找出您的用户代理，以确保它在您的机器上正确运行。这对于每个人来说是不同的，但是在 google 中输入我的用户代理应该对你有用。然后，您希望获取一个请求变量，该变量包含标题和 URL。在此基础上，我们将使用现在调用 beautiful soup、调用 r.content 和调用 HTML 解析器的一个库。你想确保这个变量返回 soup 您可以在下面找到这个函数的代码。

```
def extract(page):headers = {"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.88 Safari/537.36"}url =  f"https://web3.career/marketing+remote-jobs?page={page}"r = requests.get(url,headers)soup = BeautifulSoup(r.content, "html.parser")return soup
```

这是我们转移到代码库的转换方面的地方。
在这里，我们将创建一个名为 transform 的新函数，并将来自 extract 函数的汤传递给它。这是事情变得棘手的地方，对您的代码来说会有所不同。您将需要进入正在运行的浏览器的调试工具，并开始读取 HTML。来看看你需要你的工作伙伴阅读的代码。它很可能是一个 div，其中包含了您正在寻找的所有招聘信息。然后，您需要扫描您正在寻找的工作角色的各个方面，例如工作名称和工资，并将其存储在一个变量中。这个变量的最后一步是将每次迭代的所有内容存储在一个字典中，并将其添加到您的作业列表中。

```
def transform(soup):divs = soup.find_all('div', class_="d-flex align-middle")for item in divs:title = item.find('a').text.strip()company = item.find('div', class_="mt-auto d-block d-md-flex").text.strip()try:salary = item.find('p', class_="text-salary").text.strip()except:salary = ''job = {"title":title,"company":company,"salary":salary}joblist.append(job)return
```

该项目的学习方面相对简单。您只需创建存储所有作业的作业列表，并对其进行格式化，使其适合 pandas 数据框。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)