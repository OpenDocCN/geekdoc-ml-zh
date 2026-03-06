# 在 Docker 容器中使用气流、DBT、Postgres 的可扩展 ELT 数据仓库管道

> 原文：<https://medium.com/mlearning-ai/scalable-elt-data-warehousing-pipeline-using-airflow-dbt-postgres-in-a-docker-container-a0078362bc3?source=collection_archive---------4----------------------->

**简介**

许多组织使用传感器和物联网设备从多个来源收集数据。为此构建系统的主要挑战是将一个工具与另一个工具集成。这份报告将展示如何创建一个有效的管道和我们将用于该项目的技术栈。

**目标**

一个城市交通部门希望使用 swarm UAVs(无人机)从该市的几个位置收集交通数据，并将收集的数据用于改善该市的交通流量和其他几个未公开的项目。该项目将负责创建一个可扩展的数据仓库，该数据仓库将托管通过分析 swarm 无人机拍摄的镜头提取的车辆轨迹数据。

为此，我们将使用 ELT 管道。ELT 是一个数据集成过程，用于将原始数据从源服务器传输到目标服务器上的数据系统(如数据仓库或数据湖),然后为下游使用准备信息。让我们简单看看 ELT 代表什么。

*   提取数据:它是从一个或多个源系统中识别和读取数据的过程。
*   加载数据:加载是将提取的数据添加到目标数据库的过程。
*   转换数据:这是将数据从其源格式转换为分析所需格式的过程

**数据**

数据通过 [pNEUMA 网站](https://open-traffic.epfl.ch/index.php/downloads/#1599047632450-ebe509c8-1330)提供。pNEUMA 是一个开放的大规模数据集，包含 50 万辆汽车的自然轨迹，由一群无人机在希腊雅典拥挤的市区进行的一项独一无二的实验收集而成。单个(区域、日期、时间)的每个文件大约是 87MB 的数据。

**技术堆栈**

下面是我们将在这个项目中使用的主要工具，我们将看到项目如何流动。

*   **Python** :一种面向对象的编程语言，用于编写数据提取、加载和转换脚本。
*   **Docker** :用于创建适当且有控制地使用技术堆栈的环境。
*   **Airflow** : Apache Airflow 是一个开源的工作流管理平台，提供了以编程方式开发、监控和调度工作流的能力。气流管道用 Python 定义，然后转换成有向无环图(DAG)。
*   **DBT** : DBT 是一个开发框架，它将模块化 SQL 与软件工程最佳实践相结合，使数据转换可靠而快速。它使具有数据分析师技能的人能够访问数据工程活动，使用简单的 select 语句转换仓库中的数据，有效地用代码创建整个转换过程。
*   **Postgres** :这是一个高级的企业级开源关系数据库，支持 SQL(关系)和 JSON(非关系)查询。我们用它来存储原始数据和转换后的数据。
*   **Redash** :是一个开源的 web 应用程序，用于探索、查询、可视化和共享来自我们数据源的数据。

![](img/fc9cca8d773f0f1d13fa30e72ffd93a8.png)

The workflow pipelines

**对数据进行探索性数据分析**

pNEUMA 数据集以原始 CSV 格式提供。数据集的每一行代表一辆车的数据。第一行的前 10 列包括列名，而前 4 列包括关于轨迹的信息，如唯一的 trackID、车辆类型、行驶的距离(米)和车辆的平均速度(公里/小时)

为了使原始 CSV 数据可用，我用分隔符加载了它。

```
import pandas as pd
df = pd.read_csv('./data/drone3.csv', sep='[;:]', index_col=False)
```

*   数据是干净的，没有丢失的值。

## 气流编排

首先，我们需要建立开发环境。我创建了一个 python 虚拟环境。之后，我在 airflow 目录下创建了一个 docker-compose.yaml 文件。

这是 docker-compose.yaml 文件的样子。

```
version: "3"
x-airflow-common:
  &airflow-common #image: ${AIRFLOW_IMAGE_NAME:-apache/airflow:2.4.1}
  build: .
  environment: &airflow-common-env
    AIRFLOW__CORE__EXECUTOR: CeleryExecutor
    AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    # For backward compatibility, with Airflow <2.3
    AIRFLOW__CORE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__RESULT_BACKEND: db+postgresql://airflow:airflow@postgres/airflow
    AIRFLOW__CELERY__BROKER_URL: redis://:@redis:6379/0
    AIRFLOW__CORE__FERNET_KEY: ""
    AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION: "true"
    # AIRFLOW__CORE__LOAD_EXAMPLES: "true"
    AIRFLOW__API__AUTH_BACKENDS: "airflow.api.auth.backend.basic_auth"
    _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:-}
    depends_on: "postgres-dbt"
  volumes:
    - ./dags:/opt/airflow/dags
    - ./logs:/opt/airflow/logs
    - ./plugins:/opt/airflow/plugins
    - ./data:/opt/airflow/data
    - ../dbt:/opt/dbt
  user: "${AIRFLOW_UID:-50000}:0"
  depends_on: &airflow-common-depends-on
    redis:
      condition: service_healthy
    postgres:
      condition: service_healthy

services:
  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    ports:
      - "5434:5432"
    volumes:
      - postgres-db-volume:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "airflow"]
      interval: 5s
      retries: 5
    restart: always

  redis:
    image: redis:latest
    expose:
      - 6379
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 30s
      retries: 50
    restart: always

  airflow-webserver:
    <<: *airflow-common
    command: webserver
    ports:
      - 8080:8080
    healthcheck:
      test: ["CMD", "curl", "--fail", "http://localhost:8080/health"]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-scheduler:
    <<: *airflow-common
    command: scheduler
    healthcheck:
      test:
        [
          "CMD-SHELL",
          'airflow jobs check --job-type SchedulerJob --hostname "$${HOSTNAME}"',
        ]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-worker:
    <<: *airflow-common
    command: celery worker
    healthcheck:
      test:
        - "CMD-SHELL"
        - 'celery --app airflow.executors.celery_executor.app inspect ping -d "celery@$${HOSTNAME}"'
      interval: 10s
      timeout: 10s
      retries: 5
    environment:
      <<: *airflow-common-env
      # Required to handle warm shutdown of the celery workers properly
      # See https://airflow.apache.org/docs/docker-stack/entrypoint.html#signal-propagation
      DUMB_INIT_SETSID: "0"
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-triggerer:
    <<: *airflow-common
    command: triggerer
    healthcheck:
      test:
        [
          "CMD-SHELL",
          'airflow jobs check --job-type TriggererJob --hostname "$${HOSTNAME}"',
        ]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-init:
    <<: *airflow-common
    entrypoint: /bin/bash
    # yamllint disable rule:line-length
    command:
      - -c
      - |
        function ver() {
          printf "%04d%04d%04d%04d" $${1//./ }
        }
        airflow_version=$$(AIRFLOW__LOGGING__LOGGING_LEVEL=INFO && gosu airflow airflow version)
        airflow_version_comparable=$$(ver $${airflow_version})
        min_airflow_version=2.2.0
        min_airflow_version_comparable=$$(ver $${min_airflow_version})
        if (( airflow_version_comparable < min_airflow_version_comparable )); then
          echo
          echo -e "\033[1;31mERROR!!!: Too old Airflow version $${airflow_version}!\e[0m"
          echo "The minimum Airflow version supported: $${min_airflow_version}. Only use this or higher!"
          echo
          exit 1
        fi
        if [[ -z "${AIRFLOW_UID}" ]]; then
          echo
          echo -e "\033[1;33mWARNING!!!: AIRFLOW_UID not set!\e[0m"
          echo "If you are on Linux, you SHOULD follow the instructions below to set "
          echo "AIRFLOW_UID environment variable, otherwise files will be owned by root."
          echo "For other operating systems you can get rid of the warning with manually created .env file:"
          echo "    See: https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#setting-the-right-airflow-user"
          echo
        fi
        one_meg=1048576
        mem_available=$$(($$(getconf _PHYS_PAGES) * $$(getconf PAGE_SIZE) / one_meg))
        cpus_available=$$(grep -cE 'cpu[0-9]+' /proc/stat)
        disk_available=$$(df / | tail -1 | awk '{print $$4}')
        warning_resources="false"
        if (( mem_available < 4000 )) ; then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough memory available for Docker.\e[0m"
          echo "At least 4GB of memory required. You have $$(numfmt --to iec $$((mem_available * one_meg)))"
          echo
          warning_resources="true"
        fi
        if (( cpus_available < 2 )); then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough CPUS available for Docker.\e[0m"
          echo "At least 2 CPUs recommended. You have $${cpus_available}"
          echo
          warning_resources="true"
        fi
        if (( disk_available < one_meg * 10 )); then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough Disk space available for Docker.\e[0m"
          echo "At least 10 GBs recommended. You have $$(numfmt --to iec $$((disk_available * 1024 )))"
          echo
          warning_resources="true"
        fi
        if [[ $${warning_resources} == "true" ]]; then
          echo
          echo -e "\033[1;33mWARNING!!!: You have not enough resources to run Airflow (see above)!\e[0m"
          echo "Please follow the instructions to increase amount of resources available:"
          echo "   https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#before-you-begin"
          echo
        fi
        mkdir -p /sources/logs /sources/dags /sources/plugins
        chown -R "${AIRFLOW_UID}:0" /sources/{logs,dags,plugins}
        exec /entrypoint airflow version
    # yamllint enable rule:line-length
    environment:
      <<: *airflow-common-env
      _AIRFLOW_DB_UPGRADE: "true"
      _AIRFLOW_WWW_USER_CREATE: "true"
      _AIRFLOW_WWW_USER_USERNAME: ${_AIRFLOW_WWW_USER_USERNAME:-airflow}
      _AIRFLOW_WWW_USER_PASSWORD: ${_AIRFLOW_WWW_USER_PASSWORD:-airflow}
      _PIP_ADDITIONAL_REQUIREMENTS: ""
    user: "0:0"
    volumes:
      - .:/sources

  airflow-cli:
    <<: *airflow-common
    profiles:
      - debug
    environment:
      <<: *airflow-common-env
      CONNECTION_CHECK_MAX_COUNT: "0"
    # Workaround for entrypoint issue. See: https://github.com/apache/airflow/issues/16252
    command:
      - bash
      - -c
      - airflow

volumes:
  postgres-db-volume:
```

编写了一个 Airflow Dag 脚本，该脚本读取数据，创建一个表，并将提取的数据加载到创建的 postgres 表中，该脚本使用 python 操作符 SQL Alchemy 引擎将数据加载到 docker 映像上的 Postgres 数据库中。

```
import airflow
import os,sys
import pandas as pd

from sqlalchemy import create_engine
from datetime import timedelta,datetime

from airflow import DAG
from airflow.operators.python import PythonOperator

sys.path.append(os.path.abspath(".."))

default_args = {
    'owner':'nahom',
    'retries':5,
    'retry_delay':timedelta(minutes=2)
}

def load_clean_data(path, table_name):
    """task to load clean data to postgres database

    Args:
        path (str): path to clean data 
        table_name (str): database table name
    """
    print("writing data..............")
    engine = create_engine("postgresql+psycopg2://airflow:airflow@postgres/airflow", echo=True)
    df = pd.read_csv(path, sep=",", index_col=False)

    df.to_sql(table_name, con=engine, if_exists='replace',index_label='id')
    print("!!!!Done!!!!")

def clean_df(path):
    """function to clean raw data 

    Args:
        path (str): path of the data to be cleaned
    """

    df = pd.read_csv(path,sep='[;:]',index_col=False)
    print('data loaded successfully!!!')
    df.columns = df.columns.str.replace(' ','')
    try:
        df.to_csv('./data/cleanned_data.csv',index=False)
    except Exception as e:
        print(f'error: {e}')

with DAG(
    dag_id='load_to_dwh_v2',
    default_args=default_args,
    description='This loads cleaned data to the postgres warehouse on railway',
    start_date=airflow.utils.dates.days_ago(1),
    schedule_interval='@once'
)as dag:
    task1 = PythonOperator(
        task_id='load_clean_data',
        python_callable=load_clean_data,
        op_kwargs={
            "path": "./data/cleanned_data.csv",
            "table_name":"traffic_raw"
        }
    )
    task_2 = PythonOperator(
        task_id='extract_and_clean_data',
        python_callable=clean_df,
        op_kwargs={
            "path":"./data/drone3.csv"}
    )
    task_2>>task1
```

编写完脚本后，我们启动 docker 来运行 dag 并执行任务。从 airflow 目录运行以下命令:

```
docker-compose up
```

![](img/2b418a020951e6d67c7e5acaf615117d.png)

Dag graph

![](img/c80d7fc0211941a8ab758bed28e4b5ae.png)

partial table image from the docker postgres database

## **DBT 转型**

DBT 在英语教学中做测试。它是一个数据工程工具，有助于构建可用于仓库数据转换的相互依赖的 SQL 模型。

dbt 项目是一个目录，包含。sql 和。yml 文件。最少需要的文件是:

*   名为 dbt_project.yml 的项目文件:该文件包含 dbt 项目的配置。
*   型号(。sql 文件):dbt 中的模型只是一个简单的。包含单个 select 语句的 sql 文件。

每个 dbt 项目都需要一个 dbt_project.yml 文件——这是 dbt 知道一个目录是 dbt 项目的方式。它还包含告诉 dbt 如何操作您的项目的重要信息。

一些用于创建转换的 dbt 命令

*   *dbt init —* 初始化我们项目中的 dbt
*   *dbt 调试—* 调试 dbt 连接
*   *dbt 运行—* 执行编译后的 SQL 语句
*   *dbt 测试—* 测试查询。
*   *dbt 文档生成* e 和 *dbt 文档服务—* 服务于文档并将其部署在本地服务器上。

下面是一些 dbt SQL 语句，用于生成转换，如数据点计数、按车型列出的平均速度和按车型列出的平均行驶距离。它们存储在不同的。sql 文件。但首先，我们需要创建 Postgres 数据库的实例。

```
# casting database feature and selecting from traffic_raw table
{{ config(materialized='table') }}
with traffic_model as (
  select * from traffic_raw
)
select *
from traffic_model
# average speed
select type,AVG(avg_speed) as speed_avg from {{ ref('feature')}} group by type
# average distance traveled by vehicle type
select type , AVG(traveled_d) as dist_avg
from {{ ref('feature') }}
group by type
# data point count grouped by vehicle type
select type,COUNT(*) as data_count
from {{ ref('feature')}}
group by type
```

以下是 dbt 沿袭的外观:

![](img/6af736ccda36fa6fb5653961f7e4f971.png)

dbt linage graph

## **用于可视化仓库数据的红杉**

Redash 是一个数据工程工具，用于清除数据库表和可视化结果。这些结果可以通过 redash 为您自动创建的仪表板查看。我们将使用红灰执行以下任务。

*   将 redash 与 PostgreSQL 仓储工具连接起来，以便引入数据和 dbt 转换模型。
*   编写查询以使用业务特定的筛选器分析数据。这个查询可以直接在资料来源上执行。
*   构建 Dashboard，使用不同类型的图表创建强大的可视化效果，并为用户自定义提供更多控制。
*   创建查询并保存它们以供以后重用，因为相同的查询可以作为仪表板的输入。
*   使用 UI 轻松安排查询。
*   针对数据中的任何差异创建警报。我们将通过电子邮件、slack 或 webhook 提醒用户。

以下是执行上述任务后生成的 Redash 控制面板。

![](img/8b0136d0bcf439f9d1e423911c52b53d.png)![](img/396385545a196335dcb6c2d74ee2ba2f.png)

Redash Dashboard on the dbt transformed data

**参考**

*   [https://www . kdkings . com/2021/07/dbt-data-transformation-tutorial . html](https://www.kdnuggets.com/2021/07/dbt-data-transformation-tutorial.html)
*   [https://media . com/Jakarta smartcity/data-pipeline-use-Apache-airflow-to-import-data-from-public-API-7ff 719118 AC 8](/jakartasmartcity/data-pipeline-using-apache-airflow-to-import-data-from-public-api-7ff719118ac8)
*   [https://redash.io/help/user-guide/getting-started](https://redash.io/help/user-guide/getting-started)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 的作者

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)