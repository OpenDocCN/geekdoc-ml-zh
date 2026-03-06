# 如何充分利用 Airflow 的动态任务映射

> 原文：<https://medium.com/mlearning-ai/how-to-get-the-most-out-of-airflows-dynamic-task-mapping-7feb3d766645?source=collection_archive---------7----------------------->

动态任务映射(DTM)是一个主要特性，它增加了构建 Dag 的灵活性。然而，它不能提供无限的灵活性，也不能让你摆脱气流模式的束缚。例如，您的任务映射受到 XCom 支持的数据类型的约束，即 Python 字典和列表。例如，我还没有找到为`KubernetesPodOperator`动态设置 Kubernetes 秘密的方法。

Python 允许你 [pickle](https://docs.python.org/3/library/pickle.html) 一个像 Kubernetes secret 这样的对象，它将这样的对象转换成一个字节流，XCom 确实支持 pickle 数据，但是我还没有找到一种方法将它与 DTM 结合使用。

以下是我们在[redic ics](https://www.redactics.com/)与 DTM 一起工作时收集到的一些有用的提示和观察:

# 使用全局变量

请理解，在任务映射函数内部设置的值是在将值赋给这些映射函数外部的变量之后，在运行时设置的。因此，例如，您不能在任务映射中为全局变量赋值，并期望该值在这些映射函数之外可用:

```
secrets = []

@task()
def set_vars(**context):
    global secrets
    secrets.append(Secret('volume', "/secretpath/" + context["params"]["secretname"], context["params"]["secretname"]))
    return secrets

@task()
def init_wf(secrets, **context):
    print(secrets)
    return "hello"

init_workflow = init_wf.partial().expand(
    secrets=set_vars()
)

start_job = KubernetesPodOperator(
    task_id="start-job",
    image="postgres:12"
    cmds=["uname"],
    secrets=secrets ### this value is going to be null
    )
init_workflow >> start_job
```

KubernetesPodOperator 中的`secrets`值将为空，因为在初始化`start_job`时，`set_vars`还没有运行。

# 您可以复制 Dag，使其在不同的环境中运行，例如不同的计划

不是最漂亮的模式，但您可以复制您的 Dag，例如使用 Docker entrypoint 脚本:

```
#!/bin/bash

arr=( "workflow1" "workflow2" )

for workflow_id in "${arr[@]}"
do
    cp /tmp/dag-template.py /opt/airflow/dags/${workflow_id}-mydag.py
done
```

然后，您的 DAG 可以从 API、环境变量、文件或任何对您的 DAG 提供这些变化最有意义的东西中提取配置。这似乎是显而易见的，当然也不好看，但我们必须咬紧牙关，因为我们需要某些参数(如时间表)可用，而这不是 DTM 的地盘。

如果您选择通过 API 检索一些值，这将允许更多的 DAG 是动态的，因此当您想要更改值时，它不需要更新。我们选择在文件名中使用那个`workflow_id`来传递给 API:

```
dag_file = os.path.basename(__file__).split('.')[0]
dag_id = dag_file.replace('-mydag','')

API_KEY = os.environ['API_KEY']
API_HOST = "https://api.yourthing.com"
headers = {'Content-type': 'application/json', 'Accept': 'text/plain', 'x-api-key': API_KEY}
apiUrl = API_HOST + '/airflowconfigs/' + dag_id
request = requests.get(apiUrl, headers=headers)
wf_config = request.json()
```

在 DAG 的顶部附近运行这个程序可以确保`wf_config`作为一个全局变量在整个 DAG 中可用。您可以控制 API 被轮询的频率，如果您关心如何扩展，可以用 Redis 缓存这些配置。

# 访问上下文对象(包括 DagRun 参数)需要任务流 API

例如，如果您正在使用 [Airflow REST API](https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html) 和[向 DAGRun 端点](https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html#operation/post_dag_run)传递一个 conf 对象，那么您不能从`classic`样式的操作符(如`PythonOperator`)中访问这些参数。相反，您必须使用专为 DTM 设计的任务流 API。例如:

```
@task()
def start_job(**context):
    print(context["params"]["myparam"])
```

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)