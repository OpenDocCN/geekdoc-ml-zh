# 使用 Python 进行算法外汇交易:使用 MetaTrader5 Python 库获得精确数据

> 原文：<https://medium.com/mlearning-ai/algorithmic-forex-trading-with-python-using-metatrader5-python-library-for-accurate-data-19dbafb8573c?source=collection_archive---------0----------------------->

![](img/d1212aa1365670b6be079484b920e970.png)

我是一个数学人。我对金融市场感兴趣是因为它们既随机又充满模式。金融市场的核心是一个复杂的系统；复杂性的出现可能是自发的，市场崩溃可能在经济基本面没有重大变化的情况下突然发生。与定义了开始和结束的结构性机会游戏以及指导你行为的严格规则不同，市场环境更像一条河流，价格不断流动。事实上，市场是无组织的，作为一个交易者，你可以制定所有的交易规则，并且有很大的自由。

我发展交易技巧的过程很自然地引导我走向了算法交易。对我来说，它的优势是多方面的:帮助模式识别、策略开发和回溯测试等等。

# 算法交易

算法交易者的目标是开发一个程序，最终帮助我们做出更好的交易决策。该计划可以有以下角色:

1.  作为一个信号
2.  作为交易者:分析市场数据并执行交易
3.  作为一名交易后分析师

说到算法交易，有两大类潜在引擎:

## 基于规则的算法

基于规则的程序遵循一套明确的指令来分析数据和进行交易。定义的规则集可以基于输入，例如时间、经济基本面、收益或其他数据发布，以及市场数据，例如价格、交易量等。

## 人工智能算法

基于人工智能的程序是一种可以在不遵循显式指令的情况下学习和适应的程序，通过使用机器学习技术和统计模型来分析数据模式并从中得出推论。

定义的指令集基于时间、价格、数量或任何数学模型。除了交易者的盈利机会，算法交易通过排除人类情绪对交易活动的影响，使市场更具流动性，交易更系统化。

# 工具箱里的旧工具

当谈到外汇交易时，绝大多数正在编写代码的交易机器人都是用 mql4 或 mql5 语言编写的，因为 MT4 和 MT5 是人们使用最普遍的平台。Mql4/mql5 是用于 MetaTrader4/5 的编程语言，meta trader 4/5 是最流行的外汇交易平台。

然而，交易算法(称为专家顾问，或 EA)只能使用原生 mql4/5 语言基于规则——没有内置人工智能功能。

对于我们这些喜欢使用 Python 来实现这个目标的人来说，这也并不总是可行的。Python 的局限性不在于它的能力，而在于与代理的连接性。这个事实曾经使用 Python 编写交易算法变得不可扩展，因为人们必须等待单个经纪人建立 API ( [Oanda](https://oanda-api-v20.readthedocs.io/en/latest/installation.html) 、[交互式经纪人](https://interactivebrokers.github.io/tws-api/introduction.html)等)。)用于这种连接。如果没有 API，您必须使用其他来源的替代数据来获取您的仪器的市场数据。

这一事实使得使用 Python 交易算法很难处理-在实践中，人们必须小心从其他经纪人那里获得的市场数据，因为外汇市场中不存在确定价格和交易量的集中交易所。

对于我们这些喜欢使用 Python 的人来说，我们必须适应并使用 mql4/mql5 来构建 EAs。有替代方案可以绕过 MT4/MT5 的这一限制。ZeroMQ 就是其中之一。

# 工具箱的新增内容

不过有了 Python 模块， [**MetaTrader5**](https://pypi.org/project/MetaTrader5/) ，这个坎很快就被消除了！

拥有大量用于数据清理、争论和转换的库，以及各种机器学习库，Python 无疑是数据科学和机器学习中最流行的语言。

## 优势

使用 MetaTrader5 的一个最明显的优势是，你可以从任何经纪商那里获得 OHLC 数据或分笔成交点数据。这给交易开发者带来了巨大的自由:

1.  现在，您可以根据直接从经纪人那里获得的真实市场数据来开发和测试您的算法，就好像您是从 MT5 终端操作一样，但是您将**接收 OHLC 或股票行情数据**，其格式可以通过编程来检查、清理、转换和开发！
2.  由于每个经纪人提供的市场数据略有不同，使用 MetaTrader5 库，您将能够**访问您的交易系统将与之交易的经纪人特定数据**！
3.  此外，您的**开发工作现在可以扩展了**，因为您不再需要受到由于缺少代理 API 而产生的连接性问题的限制。

## **连接到终端和特定账户**

连接到交易账户的步骤如下:

```
if not mt5.initialize():
    print("initialize() failed, error code =", mt5.last_error())
    quit()mt5.login(login = <account_num>, password=<"acct_pass">)
```

请注意，login 参数需要一个整数，password 参数需要一个字符串输入。

## 获取市场数据(OHLC 或分笔成交点数据)

以下代码块将检索过去 150 个日历日的 OHLC 数据:

```
if not mt5.initialize():
        print("initialize() failed, error code =",mt5.last_error())
        quit()mt5.login()timezone = pytz.timezone("Europe/Tallinn")
now = datetime.datetime.now(timezone)
start = datetime.datetime.now(timezone) - timedelta(days=150)
utc_from = datetime.datetime(start.year, start.month, start.day)
utc_to = datetime.datetime(now.year, now.month, now.day, now.hour, now.minute, now.second)rates = mt5.copy_rates_range(pair, mt5.TIMEFRAME_H4, utc_from, utc_to)
print("Ticks received:",len(rates))
htf = pd.DataFrame(rates)
```

要获得分笔成交点数据，您可以使用`copy_ticks_from()`或`copy_ticks_range()`函数。这些非常有用，因为它们让您可以访问分笔成交点数据，并为您提供更多的战略选择。

## 配售交易

下面的代码片段展示了如何使用 MetaTrader5 包在 Python 中实现订单。这里，我已经预先确定目标“阈值”是我愿意交易的最大价差，并且已经为每一对(即`symbol`)计算了目标`entry`、`stop`、`target`、`size`的参数:

```
try:
    if (mt5.symbol_info_tick(symbol).ask \-
        mt5.symbol_info_tick(symbol).bid) <= threshold:
        o1 = mt5.order_send({"action": mt5.TRADE_ACTION_DEAL,
                             "symbol": symbol,
                             "volume": size
                             "type": mt5.ORDER_TYPE_BUY,
                             "price": entry,
                             "sl": stop,
                             "tp": target,
                             "deviation": 10,
                             "magic": 00000,
                             "comment": "insert comment",
                             "type_time": mt5.ORDER_TIME_GTC
                              })
except Exception as e:
    print(e)
```

注意,“comment”参数，一旦给定一个描述性字符串，将在 MT5 终端的“comment”下显示为描述。这是非常有用的信息，因为它为以后的分析提供了有用的信息。

## 修改交易

使用与 MetaTrader5 集成的 Python，您可以按如下方式修改您的现有头寸:

```
to_close = mt5.order_send({
            "action": mt5.TRADE_ACTION_DEAL,
            "symbol": pair,
            "volume": mt5.positions_get(symbol)[0].volume,
            "type": mt5.ORDER_TYPE_SELL, 
            "position": mt5.positions_get(symbol)[0].identifier,
            #"price": mt5.symbol_info_tick(symbol).bid,
            "deviation": 10,
            "magic": 00000,
            "comment": "insert comment",
            "type_time": mt5.ORDER_TIME_GTC})
```

在上面的代码中，`mt5.ORDER_TYPE_SELL`被用来启动一个市场订单来结束一个多头头寸。你会`mt5.ORDER_TYPE_BUY`平仓一个空头头寸。

## 退出交易

```
to_close = mt5.order_send({
            "action": mt5.TRADE_ACTION_DEAL,
            "symbol": pair,
            "volume": mt5.positions_get(symbol=symbol)[0].volume,
            "type": mt5.ORDER_TYPE_SELL (to close long), or mt5.ORDER_TYPE_BUY (to close short)
            "position": mt5.positions_get(symbol=symbol)[0].identifier,
            #"price": mt5.symbol_info_tick(symbol).bid,
            "deviation": 10,
            "magic": 0000,
            "comment": "insert comment",
            "type_time": mt5.ORDER_TIME_GTC,
```

在上面的代码中，`mt5.ORDER_TYPE_SELL`用于启动平仓市场订单。你可以使用`mt5.ORDER_TYPE_BUY` 平仓。

## MetaTrader5 的限制

MetaTrader5 的一个严重缺点是还不支持 MacOS 和 Linux。Mac 和 Linux 用户不得不依赖某种形式的 Windows 虚拟化。