# 使用身份验证令牌调用 API

> 原文：<https://medium.com/mlearning-ai/calling-apis-with-authentication-token-5e4412bd75b5?source=collection_archive---------1----------------------->

![](img/e000daaa36e64ccb1e557e011fdc2cd8.png)

我不得不写一个 python 脚本来调用一系列基于业务逻辑的 API。问题是这些 API 需要一个认证令牌来执行。在缺少令牌的情况下，我得到了`403 Forbidden`错误，因为 API 无法识别调用(来自程序)是否来自真正的来源。

我谷歌了如何从 python 代码调用 API，但我看到的每篇文章都假设 API 是开放的、公共的，不需要通过令牌进行任何授权。这篇文章是我的研究和各种不成功的代码执行的结果，最终找到了用认证令牌从 python 代码调用 API 的方法。

# 你需要什么？

你应该知道生成令牌的 API 的细节。这通常是第一个 API，所有后续 API 都使用第一个 API 生成的相同令牌进行身份验证。让我们将第一个 API 称为**登录 API** ，它需要一个用户 id 和密码作为输入。首先应该知道正确的用户 id 和密码。我们把第二个叫做**报表 API** 。您还应该知道登录 API 头中的哪个键值对包含 web 令牌，以及 web 令牌应该传递到的报告 API 头中的正确键值对。

# 先决条件

登录 API URL、报告 API URL、用户 id 和密码的详细信息已经存储在特定文件夹的配置文件中。我们知道 web 令牌是登录 API 的响应头中的当前键`at`。

# 假定

由于用户 id 和密码的敏感性，我没有将它们放在代码中，而是放在一个单独的配置文件中，该文件可以读取 url、用户 id 和密码等细节。我在下面写的代码假设你已经使用过 python 中的`configparser`和`logging`包。请阅读我的另一篇文章[配置文件和日志](https://divijsharma.medium.com/config-files-and-logging-a7b4cc377fd5)了解更多细节。`read_config`和`set_logging_basics`功能在[那篇文章](https://divijsharma.medium.com/config-files-and-logging-a7b4cc377fd5)中有描述。

所以让我们开始吧！

配置文件看起来像这样

main_config.ini

首先，让我们为配置文件中的节和键名定义一些静态变量。

Static variables in the code

接下来，我们将定义 2 个小函数来从字典中读取 URL 和登录详细信息，并返回相关的详细信息。

Function to get the API URL details

Function to get the login details

在定义了函数和静态变量之后，是时候获取日志详细信息了。

Details of file where logs will be written

获取登录详细信息，并创建用户名和密码的键值对。键名应该与 API 中的编码完全相同。

User ID and password details

设置完成后，就该调用登录 API 并获取身份验证令牌了。身份验证令牌是登录 API 响应的一部分。它出现在关键字`at`的标题中。令牌必须在有效载荷报头的同一个`at`密钥中传递给后续的报告 API。

Get Authentication Token

到目前为止，我们已经为带有身份验证令牌的报告 API 请求创建了头部。现在调用报告 API。

Report API

现在我们知道了如何从一个 API 读取认证令牌，并将其传递给另一个 API。如果出现错误，我就终止程序——相关的业务逻辑可以放在`if resp_login.status_code != 200`中处理优雅的退出。