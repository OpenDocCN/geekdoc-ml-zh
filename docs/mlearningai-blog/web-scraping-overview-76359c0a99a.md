# 网页抓取概述

> 原文：<https://medium.com/mlearning-ai/web-scraping-overview-76359c0a99a?source=collection_archive---------6----------------------->

# 网页抓取

Web 抓取是一种以自动方式从网站中提取信息的技术。该技术包括将非结构化数据或 HTML 格式的数据转换成可以存储在数据库或电子表格中并可以以表格格式呈现的结构化数据。
最常用于抓取，即从 web 中提取信息的语言是 Python。

**在转向网络抓取之前，有两件基本的事情需要知道——**

*   了解互联网上的目标数据——这是我们需要从网络中提取的数据。电子商务、体育等方面的示例数据。
*   列出网站，即列出我们可以从中获取所需数据的网站名称。

现在我们准备刮。在进入 scrape 之前，让我们先了解一下解析器。

# 分析器-

**解析器是一种用于解释或呈现来自 web 文档的信息的工具。解析器接收程序指令、命令和标记标签形式的输入(就像您在 HTML 文档中看到的那样)，并将 web 文档输出为对象、方法及其属性。在处理信息之前，解析器还用于信息验证。**

解析是至关重要的一步。如果该步骤在 web 抓取期间失败，则不能通过后续过程提取和存储数据。

# 抓取网页的步骤如下-

*   **第一步**
    向目标网站发送一个请求，从你想要提取信息的地方，收集所需的数据。
*   **步骤二**
    然后从目标网站接收 HTML 或 XML 格式的信息。
*   **步骤三**
    解析完成，即读取数据，从文档中提取信息。然后，基于数据通过几个解析器对检索到的信息进行解析。HTML 解析器用于读取 HTML 文档，XML 解析器用于读取 XML 文档。
*   **第四步**
    解析后的数据按照要求的格式存储。解析后的数据可以存储在数据库中，也可以传输到其他 web 应用程序。

# 用于网页抓取的 Python 库-

**美丽组图**

*   这是一个简单而健壮的 python 库，主要用于 web 抓取。
*   该图书馆拥有剖析文档和从网页中提取信息所需的高效工具。
*   它有几组方法，可以方便地导航、搜索和修改解析树(一个解析的 HTML 文档)
*   它有一个支持 HTML 和 XML 文档的解析器
*   它可以自动将所有传入的文档转换为 Unicode，这使得它们易于阅读和解析。此外，它自动转换外发文件到 UTF-8。

**BeautifulSoup 支持各种解析器**—

*   **html.parser** 基于 python，快速且宽容。
*   **lxml html** 依赖 C，又快又宽大。
*   **lxml xml** 是唯一的 xml 解析器，依赖于 c
*   html5lib 是基于 python 的，但是它很慢。

# 将数据解析成 Python 对象-

HTML 对象在经过 HTML 解析器后被转换成一个对象树，即它形成一个层次结构，其中 HTML 标签是第一个节点，然后 head 和 body 标签是它的子节点，依此类推。所以所有的标签都被转换成一棵树。然后，这些对象用于通过搜索或浏览文档的各个部分来提取数据或信息。对象(基本上是标签)之间存在一种关系，这使得高效地检索信息变得更加容易。

这个 HTML 文档在解析后被转换成**一个复杂的 python 对象树，它有四种类型的对象-**

*   **标签**是文档中的 HTML 标签，有很多属性和方法
*   **可导航字符串**是对应于标签中文本的一组字符
*   **BeautifulSoup** 表示整个 web 文档，支持文档树的导航和搜索。
*   **注释**是文档的信息部分。一种特殊类型的可导航字符串。

HTML 文件抓取演示-

> 让我们创建一个 html 文档，就像你创建网页一样，
> **document = " " "
> <html>
> <head>
> <title>这是关于刮</title>
> </head>
> <body>
> <em><！—表示注释对象的注释行→</em>
> <p title = " First page " class = " scrape ">这是关于从 Kolkata 使用< b >多个标签< /b >和句子< /div >
> < h3 >来学习 scrape**
> 
> **#从 bs4 导入 BeautifulSoup** 导入刮削库
> 
> **#使用 html 解析器解析文档
> soup _ parsed = beautiful soup(document，' html . parser ')
> soup _ parsed**

输出—
< html >
<头部>
<标题>这是关于刮</标题>
</头部>
<身体>
< em > <！—表示注释对象的注释行→</em>
<p class = " scrape " title = " First page ">这是关于学习刮</p>
<div>使用< b >多标签< /b >和句子</div>
<H3>

> **类型(soup_parsed)**

输出-
bs4。美丽的声音

> **#让我们来获取段落标签
> tag = soup _ parsed . p
> print(tag)
> print(type(tag))**

output-
<p class = " scrape " title = " First page ">这是关于学习刮削</p>
<class ' bs4 . element . tag '>

> **#让我们获取标签属性
> tag.attrs**

Output -
{'title ':'第一页'，' class': ['scrape']}

> **#让我们获取标签值
> tag.string**

Output -
'这是关于学习刮削'

> **#让我们对 div
> tag _ div = soup _ parsed . div
> print(tag _ div)
> print(type(tag _ div))**

输出-
< div >使用< b >多标签< /b >和句子< /div >
<类' bs4.element.Tag' >

> **tag_div.b.string**

输出-
'许多标签'

这是一个网页抓取的概述。因此，总的来说，我们可以从非结构化的 web 数据中生成大量数据，并在以后的阶段对其进行大量操作或存储。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)