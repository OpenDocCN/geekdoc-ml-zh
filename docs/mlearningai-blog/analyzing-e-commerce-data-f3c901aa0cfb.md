# 分析电子商务数据

> 原文：<https://medium.com/mlearning-ai/analyzing-e-commerce-data-f3c901aa0cfb?source=collection_archive---------6----------------------->

分析来自电子商务的原始数据，以获得一些洞察力，我们可以从中得出一个答案，作为一个商业想法或计划。

# 介绍

此报告包含对电子商务数据的分析，这些数据包含订单、卖家、评论、订单项目、产品、产品类别、支付类型和地理位置。

我提取了一些信息作为见解，我们可以查看这些信息，以了解增加产品销售的最佳策略。

让我们先开始了解数据，我们可以看看如何从中产生洞察力。

# 理解数据

我们的数据集并不特别，事实上，它只是简单的关系数据，你可以在下面看到 ERD。

![](img/074196f19506bb1038b48904604992ec.png)

ERD of e-commerce data

我们有一个订单表，通过其 *order_id* 链接到**审查**表、**订单项目**表和**付款类型**表。

**客户**表也通过其*客户 id* 与订单表链接，其*邮政编码前缀*与**地理位置**表链接。

订单项目表通过其*销售者 id* 链接到**销售者**表，还通过其*产品 id 链接到**产品**表。*

# 产生洞察力

我们已经了解了数据，还知道什么？让我们试着从这些数据中获得洞察力。

我有几个问题可以问。

1.  每个城市下了多少订单？
2.  最受欢迎的类别是什么？
3.  最受欢迎的支付方式是什么？

让我们回答每个部分中的问题。

# *订单最多的城市*

为了回答第一个问题，我将尝试从两个不同的表中提取数据，并使用`pandas`将它们组合在一起，对数据进行排序，只获得前 5 名，并使用`matplotlib`将其可视化。

让我们创建客户类。

```
class Customer:
    '''Customer static that use for fetch customers related data
    '''

    def get_customers_count_by_city():
        '''get all customers count by city from db
        '''
        conn = connect_db()
        sql = '''SELECT customer_city, count(*) as total from olist_order_customer_dataset oocd group by customer_city order by count(*) desc LIMIT 5'''

        try:
            result = pd.read_sql_query(sql, conn)
        except sqlite3.Error as err_msg:
            print(f'SQLite error: {err_msg}')

        return result
```

客户类包含`get_customers_count_by_city`方法，该方法将获取`customer_city`并聚合其计数。

我们还将创建订单类。

```
class Order:
    '''Order static that use for fetch orders related data
    '''

    def get_orders():
        '''get all orders from db
        '''
        conn = connect_db()
        sql = '''SELECT * from olist_order_dataset'''

        try:
            result = pd.read_sql_query(sql, conn)
        except sqlite3.Error as err_msg:
            print(f'SQLite error: {err_msg}')

        return result
```

order 类包含`get_orders`方法，该方法将从数据库中获取所有订单。

现在，我们可以使用这两种方法，并用 pandas 合并它们。

```
def get_customers_count_by_city():
    '''get_customers_count_by_city will fetch get customers and city data from SQL that already use INNER JOIN in the query
    and will merge into orders data and aggregating using count
    '''
    customers = Customer.get_customers_and_city()
    order = Order.get_orders()

    customer_order = customers.merge(order, on="customer_id", how="left")
    customer_order.fillna(0)

    customer_grouped = customer_order.groupby(
        "customer_city")["customer_id"].count().sort_values(ascending=False).reset_index(name="count")

    # Returning TOP - 5 Data for plotting
    return customer_grouped.head(n=5)
```

基本上这段代码会完成这些工作

1.  获取`customer_city`并累计其计数。
2.  从数据库中获取所有订单
3.  通过`customer_id`将`customer`表与`orders`表合并为左连接
4.  使用`fillna`处理 NaN，因为我们不想丢弃空数据。
5.  按`customer_city`将它们分组并再次聚合。
6.  按照降序对值进行排序，因此我们按照最大值进行排序，在本例中，是`customer_city`聚合的总和。
7.  返回前 5 个最高计数数据。

现在，我们已经有了干净的数据，让我们把它可视化。

```
import matplotlib.pyplot as plt
from cases import city_with_most_order

city_with_most_order.get_customers_count_by_city().plot(
    kind="bar", x="customer_city", y="count").set_title("Cases 1 - City with most orders - TOP 5")
plt.show()
```

这段代码将完成这些工作

1.  导入我们已经使用熊猫完成的干净数据
2.  使用条形图绘制，x =客户城市，y =计数

![](img/bda2990a7e0ffdeb5096b58d9c1bccde.png)

会显示 ***圣保罗*** 是 17k 左右订单最多的城市，其次是 ***里约热内卢***at*7k。*

*由此，我们知道我们可以通过做更多的活动或广告来最大化里约热内卢这座城市。*

# *最受欢迎的类别*

*要回答第二个问题，我可以从`order_items`和`categories`表中提取数据，将它们连接起来，按类别名称分组，然后合计计数。*

*我们可以从添加一个新方法到我们将要创建的`Product`类开始。*

```
*class Product:
    '''Product static that use for fetch products related data
    '''

    def get_products_categories_by_order_count():
        '''get all product categories based on orders from db 
        '''
        conn = connect_db()
        sql = '''
            WITH product_id_agg AS (
                select product_id, count(*) as total from olist_order_items_dataset ooid group by product_id
            )
            select
                opd.product_category_name, count(*) as total
            from olist_products_dataset opd
            inner join product_id_agg p ON opd.product_id = p.product_id
            group by opd.product_category_name
            order by count(*) desc;
        '''

        try:
            result = pd.read_sql_query(sql, conn)
        except sqlite3.Error as err_msg:
            print(f'SQLite error: {err_msg}')

        return result*
```

*`get_products_categories_by_order_count`方法将调用数据库，通过`product_id`查询`order_items`表和组，并对其进行聚合。*

*之后将对`category`表进行内部连接，并按`product_category_name`对其进行分组，然后合计计数。*

*所以查询的结果，会返回每个`product_category_name.`上的计数列表*

*我们已经得到了数据，让我们消费它。*

```
*from services.product import Product

def get_orders_categories_by_product_count():
    '''get_orders_categories_by_product_count will fetch categories count on each product that being orders
    '''
    categories = Product.get_products_categories_by_order_count()

    categories["total"].dropna()

    # Returning TOP - 5 Data for plotting
    return categories.head(n=5)*
```

*基本上，我们只是做这些*

1.  *从我们已经在`get_products_categories_by_order_count`中完成的 SQL 中获取类别聚合数据*
2.  *放弃所有的 NaN 值，因为我们不需要它*
3.  *返回前 5 个最受欢迎的类别*

*我们已经清理了数据，现在让我们可视化它。*

```
*import matplotlib.pyplot as plt
from cases import product_category_with_most_order

product_category_with_most_order.get_orders_categories_by_product_count().plot(
    kind="bar", x="product_category_name", y="total").set_title("Cases 2 - Top 5 Product Category with the most order")
plt.show()*
```

*它将获得干净的数据，并开始使用 matplotlib 进行可视化，x 轴= product_category_name，y 轴= total。*

*![](img/2fdf7c66ddd41bd27d1eca3cbbc8bc24.png)*

*最受欢迎的第一名和第五名之间的差别并不大，这意味着对该产品的需求相当广泛。*

*我不认为我们有太多的话要说，通过看图表，因为类别的需求是分散的，当我们想要通过流行的类别进行营销时，我们需要挖掘得更深。*

# *最受欢迎的付款方式*

*通过从`payments`表和`orders`表中提取数据来回答这个问题，我们需要连接这两个表，并在按`payment_type`分组时对计数进行聚合。*

*我将在我们已经创建的`Order`类上添加一个新方法。*

```
*class Order:
    '''Order static that use for fetch orders related data
    '''

    def get_order_payment():
        '''get get_order_payment order payments and aggregate
        '''
        conn = connect_db()
        sql = '''
            WITH payment_order AS (
                select * from olist_order_payments_dataset oopd inner join olist_order_dataset ood on oopd.order_id  = ood.order_id
            )
            select payment_type, count(*) as total from payment_order group by payment_type
        '''

        try:
            result = pd.read_sql_query(sql, conn)
        except sqlite3.Error as err_msg:
            print(f'SQLite error: {err_msg}')

        return result*
```

*它将返回按`payment_type`分组并按计数汇总的`payments`数据。*

```
*from services.orders import Order

def get_order_payments():
    '''get_order_payments will fetch payment counts by order
    '''
    payments = Order.get_order_payment().sort_values(by="total", ascending=False)
    payments["total"].dropna()

    # Returning TOP - 3 Data for plotting
    return payments.head(n=3)*
```

*现在，我们将消耗数据，并尝试通过删除 NaN 值来清理数据。*

*我们还按照总值降序对值进行了排序。*

*之后，我们返回前 3 名最受欢迎的付款。*

*让我们想象一下。*

```
*import matplotlib.pyplot as plt
from cases import payment_most_use

payment_most_use.get_order_payments().plot(
    kind="bar", x="payment_type", y="total").set_title("Cases 3 - Top 3 Payment type that is used")
plt.show()*
```

*![](img/17572083302f7433ac9eb63bee067232.png)*

*这么多人用*信用卡订购，也许我们可以在使用 ***信用卡*** 付款方式时通过增加更多促销来增加销售额。**

# **摘要**

**我们已经做了一个很棒的任务，我们试图从根本不值得看的原始数据中创造出商业洞察力。**

**我们提取了一些东西，比如，**

1.  *****订单最多的城市*** 从这些信息中，我相信我们可以尝试去接近一个有潜在客户的新城市，比如 ***里约热内卢。*****
2.  *****最受欢迎的品类*** 利用这些信息，我们可以了解有多少品类是客户需求的，我们可以看到前 5 个品类几乎相等，这意味着我们的客户不仅仅专注于一个品类。我们可以通过增加广告来增加这 5 种产品的销量。**
3.  *****最受欢迎的付款类型*** 利用这些信息，我们可以给排名第一的付款类型更多折扣，从而达成更多交易。
    例如，信用卡用户数量确实很高，我们需要在用信用卡购物时打上折扣。**

# **项目链接**

**您可以访问源代码，并尝试在这里使用分析[](https://github.com/beebeewijaya-tech/ecommerce-analysis)**

# ****信用****

****Pacmann.ai 为我提供了分析数据，也是我在建立这种分析时可以参考的一门好课程。****

****Datacamp 还帮助我理解了可以优化的 python-data science 工具包。****

****[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)****