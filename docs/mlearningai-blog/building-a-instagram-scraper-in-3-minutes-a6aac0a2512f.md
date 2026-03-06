# 在 3 分钟内建立一个 Instagram 刮刀

> 原文：<https://medium.com/mlearning-ai/building-a-instagram-scraper-in-3-minutes-a6aac0a2512f?source=collection_archive---------1----------------------->

![](img/6405b0d317c97d093bbd3a39ef1cb5d5.png)

Image from Instagram

Twitter 是为情感分析、文本分类和机器学习项目收集文本的最广泛使用的来源。Twitter 为用户提供了自己的 API 来抓取他们的文本数据。然而，现在的 Instagram 更多地被二三十岁的青少年和年轻人使用。不幸的是，你需要自己做一个刮刀或者使用开源库来抓取他们的文本数据或者评论。我发现使用开源库包来抓取 Instagram 评论极其困难。本文将指导你在 *3 分钟*内建立自己的 Instagram 评论抓取器！

## 1.安装并加载所需的库包

```
import pandas as pd
import time
import re
import os
import csv
import json
from urllib.request import urlopen
from urllib.parse import quote_plus
from bs4 import BeautifulSoup
from selenium import webdriver
```

然后你需要从这个网站下载网络驱动:【https://chromedriver.chromium.org/downloads 

将你下载的网络驱动程序(默认情况下可能会命名为 ***chromedriver*** )放在你想要工作的文件夹中。

## 2.保存驱动程序并获取 URL

```
driver = webdriver.Chrome(‘path to your driver’)# link to the post you want to scrap comments from (This is Kim Kardashian's Instagram URL)
url = "[https://www.instagram.com/p/CQWQDCOgSt9/](https://www.instagram.com/p/CQWQDCOgSt9/)"# Driver will access to the URL
driver.get(url)
```

然后你保存你的驱动程序，并把它放入到你的驱动程序的路径中！所以我的应该是这样的: *webdriver。chrome('/用户/irenekim/桌面/个人/抓取/chromedriver')*

Instagram 允许从公众账号收集评论和图片。还有，只要你用的是公众人物的公开账号，就不算侵犯隐私。但是一定要反复检查，不要侵犯任何人的数据隐私！

如果你想知道当前的工作目录，你可以用一个简单的代码找到它，它会显示你现在在 python 中的哪个工作目录:

```
os.getcwd()
```

## 3.操作你的驱动抓取评论

```
# Create an empty container to put all the comments inside
comments = []
```

这些代码会自动为你点击 Instagram 上的加载更多按钮！

```
hasLoadMore = True
while hasLoadMore:
    time.sleep(1)
    try:
        if driver.find_element_by_css_selector('#react-root > section > main > div > div > article > div.eo2As > div.EtaWk > ul > li > div > button > span'):
            driver.find_element_by_css_selector('#react-root > section > main > div > div > article > div.eo2As > div.EtaWk > ul > li > div > button > span').click()
    except:
        hasLoadMore = False
```

**_6lAjh** '是 Instagram 中用户名的类名。'**find _ elements _ by _ CSS _ selector**中写的路径是 Instagram 中 span 按钮加载更多评论的路径。您可以通过右键单击页面并进入'*检验*'页面来试验该路径。

```
users_list = []
users = driver.find_elements_by_class_name('_6lAjh')for user in users:
    users_list.append(user.text)

i = 0
texts_list = []texts = driver.find_elements_by_css_selector('#react-root > section > main > div > div > article > div.eo2As > div.EtaWk > ul > ul > div > li > div > div > div.C4VMK > span')for txt in texts:
    texts_list.append(txt.text)
    i += 1
    comments_count = len(users_list)

for i in range(1, comments_count):
    user = users_list[i]
    text = texts_list[i]
    comments.append(users_list[i])
    comments.append(texts_list[i])
    print("User ",user)
    print("Text ",text)
    print()
```

结果应该是这样的！出于隐私考虑，我删除了这里的用户名。

```
User  user1
Text  🙌 I love this 😍

User  user2
Text  Fatherhood ❤️
```

调用 comments 会给你所有的用户名列表和他们的评论。

```
['user1',
 'This post is everything ❤️',
 'user2',
 'Kids r adorable']
```

## 4.为模型训练制作好看的 CSV 文件

我感兴趣的是分离所有的用户名和他们的评论，因为我们需要的通常是文本，而不是用户名。因此，我将创建一个 CSV 文件，在“用户”列中包含所有帐户，在“文本”列中包含相应的评论。

```
rows = []
for user, text in zip(comments[::2], comments[1::2]):
    print(user, text)
    rows.append([user, text])fields = ["User", "Text"]
filename = "cleanfile.csv"
with open(filename, 'w',  encoding='utf-8', newline='') as csvfile: 
    csvwriter = csv.writer(csvfile) 
    csvwriter.writerow(fields) 
    csvwriter.writerows(rows) df = pd.read_csv(r"path to your filename")
print(df)
```

只是提醒一下，别忘了放。csv 在你的路径的末端到你的文件名！

结果是一个干净的数据帧:

```
 User                Text
0  user100         I love you Kim
1  user200         Causal nothing, it's called wanting attention
2  user300         This women needs mental health this is just wr...
```

正如你所看到的，有一些刻薄的评论…
我实际上写了我的硕士论文，关于检测网络欺凌的评论，这些评论专门针对使用深度学习的女性！无论如何，这是你创建自己的 Instagram 评论抓取工具的时候了。

轻松点。