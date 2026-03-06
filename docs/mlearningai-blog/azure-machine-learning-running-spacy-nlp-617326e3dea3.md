# Azure 机器学习运行 spacy NLP

> 原文：<https://medium.com/mlearning-ai/azure-machine-learning-running-spacy-nlp-617326e3dea3?source=collection_archive---------2----------------------->

# 使用 Azure 机器学习为训练和评分建立端到端管道

# 先决条件

*   Azure 帐户
*   Azure 存储
*   Azure 机器学习服务

# 密码

*   该示例代码展示了如何使用 sdk 创建和运行培训和推理 aml 管道
*   不是实际的实现
*   训练和推理代码是示例，还不能用于生产
*   上下文已准备好用于生产
*   在 python 3.8 中用 azure ML 内核测试了这段代码
*   让我们配置要运行的工作区
*   下面的代码假设输入数据设置为 ADLS gen2，数据集是为输入和输出创建的。
*   当我们在存储中有输入和输出时，任何消费应用程序都可以得到结果
*   以下代码仅用于批处理。

```
import azureml.core
from azureml.core import Workspace, Datastorews = Workspace.from_config()
```

*   接下来是配置工作区默认数据存储

```
# Default datastore 
def_data_store = ws.get_default_datastore()# Get the blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")# Get file storage associated with the workspace
def_file_store = Datastore(ws, "workspacefilestore")
```

*   接下来创建计算集群

```
from azureml.core.compute import ComputeTarget, AmlComputecompute_name = "cpu-cluster"
vm_size = "STANDARD_NC6"
if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size=vm_size,  # STANDARD_NC6 is GPU-enabled
                                                                min_nodes=0,
                                                                max_nodes=4)
    # create the compute target
    compute_target = ComputeTarget.create(
        ws, compute_name, provisioning_config) # Can poll for a minimum number of nodes and for a specific timeout.
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(
        show_output=True, min_node_count=None, timeout_in_minutes=20) # For a more detailed view of current cluster status, use the 'status' property
    print(compute_target.status.serialize())
```

*   我们只使用 CPU 集群。如果需要，可以选择 GPU
*   导入 AML 库

```
from azureml.core import Dataset
from azureml.data.dataset_factory import DataType
from azureml.pipeline.steps import PythonScriptStep
from azureml.data import OutputFileDatasetConfig
from azureml.core import Workspace, Datastoredatastore = ws.get_default_datastore()
```

*   为计算创建环境配置

```
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import Environment aml_run_config = RunConfiguration()
# `compute_target` as defined in "Azure Machine Learning compute" section above
aml_run_config.target = compute_targetUSE_CURATED_ENV = True
if USE_CURATED_ENV :
    curated_environment = Environment.get(workspace=ws, name="AzureML-Tutorial")
    aml_run_config.environment = curated_environment
else:
    aml_run_config.environment.python.user_managed_dependencies = False

    # Add some packages relied on by data prep step
    aml_run_config.environment.python.conda_dependencies = CondaDependencies.create(
        conda_packages=['pandas','scikit-learn','seaborn','tqdm'], 
        pip_packages=['azureml-sdk', 'azureml-dataprep[fuse,pandas]','seaborn','tqdm', 'spacy'], 
        pin_sdk_version=False)
```

*   现在让我们编写 train.py 代码
*   创建一个新文件作为文本文件，并重命名为 train.py

```
import sys
import subprocesssubprocess.check_call([sys.executable, '-m', 'pip', 'install', 'spacy'])
subprocess.check_call([sys.executable, '-m', 'spacy', 'download', 'en_core_web_sm'])import spacy
nlp = spacy.load('en_core_web_sm')about_text = ('Gus Proto is a Python developer currently'
              ' working for a London-based Fintech'
              ' company. He is interested in learning'
              ' Natural Language Processing.')
about_doc = nlp(about_text)
sentences = list(about_doc.sents)
len(sentences)for sentence in sentences:
    print (sentence)
```

*   以上代码应该是 train 文件夹
*   如果没有，请创建一个新的
*   上面的代码除了打印句子之外没做什么
*   以上代码可以根据您的情况进行编辑
*   还可以添加添加运行信息的代码
*   接下来，我们设置培训管道

```
train_source_dir = "./train"
train_entry_point = "train.py"
 train_step = PythonScriptStep(
    script_name=train_entry_point,
    source_directory=train_source_dir,
    ##arguments=["--input_data", ds_input],
    compute_target=compute_target, # , "--training_results", training_results
    runconfig=aml_run_config,
    allow_reuse=False
)
```

*   创建管道步骤

```
# list of steps to run (`compare_step` definition not shown)
compare_models = [train_step]from azureml.pipeline.core import Pipeline# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=train_step)
```

*   验证管道

```
pipeline1.validate()
print("Pipeline validation complete")
```

*   运行培训管道

```
from azureml.core import Experiment# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Spacy_Pipeline_Notebook').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

*   显示输出

```
from azureml.widgets import RunDetailsRunDetails(pipeline_run1).show()
```

*   接下来，使用 synapse integrate 或 Azure data factory 为再培训创建管道

```
from azureml.pipeline.core.graph import PipelineParameterpipeline_param = PipelineParameter(
  name="pipeline_arg",
  default_value=10)
```

*   发布管道

```
published_pipeline1 = pipeline_run1.publish_pipeline(
     name="Published_Spacy_Pipeline_Notebook",
     description="Spacy_Pipeline_Notebook Published Pipeline Description",
     version="1.0")
```

*   上述训练脚本中没有使用管道参数，只是为了说明如何传递
*   要执行上述管道，我们首先需要许可
*   设置 Azure 服务主体
*   为参与者提供 AML 工作区的访问权限，以运行管道

```
from azureml.core.authentication import TokenAuthentication, Audience# This is a sample method to retrieve token and will be passed to TokenAuthentication
def get_token_for_audience(audience):
    from adal import AuthenticationContext
    client_id = "clientid"
    client_secret = "xxxxxxxxxxxxxxxx"
    tenant_id = "tenantid"
    auth_context = AuthenticationContext("https://login.microsoftonline.com/{}".format(tenant_id))
    resp = auth_context.acquire_token_with_client_credentials(audience,client_id,client_secret)
    token = resp["accessToken"]
    return token token_auth = TokenAuthentication(get_token_for_audience=get_token_for_audience)
```

*   创建授权标题

```
headerInfo = {'Authorization': 'Bearer ' + aad_token + ''}
```

*   现在调用发布的管道

```
from azureml.pipeline.core import PublishedPipeline
import requestsresponse = requests.post(published_pipeline1.endpoint, 
                         headers=headerInfo,
                         json={"ExperimentName": "Published_Spacy_Pipeline_Notebook",
                               "ParameterAssignments": {"pipeline_arg": 20}})
```

*   现在转到实验页面，等待实验完成
*   一旦实验完成，下一步我们可以尝试分数管道
*   首先创建一个 score.py 文件

```
import sys
import subprocesssubprocess.check_call([sys.executable, '-m', 'pip', 'install', 'spacy'])
subprocess.check_call([sys.executable, '-m', 'spacy', 'download', 'en_core_web_sm'])import spacy
nlp = spacy.load('en_core_web_sm')about_text = ('Gus Proto is a Python developer currently'
              ' working for a London-based Fintech'
              ' company. He is interested in learning'
              ' Natural Language Processing.')
about_doc = nlp(about_text)
sentences = list(about_doc.sents)
len(sentences)for sentence in sentences:
    print (sentence)
```

*   创建推理管道

```
train_source_dir = "./inference"
train_entry_point = "score.py"
 train_step = PythonScriptStep(
    script_name=train_entry_point,
    source_directory=train_source_dir,
    ##arguments=["--input_data", ds_input],
    compute_target=compute_target, # , "--training_results", training_results
    runconfig=aml_run_config,
    allow_reuse=False
)
```

*   创建管道步骤

```
# list of steps to run (`compare_step` definition not shown)
compare_models = [train_step]from azureml.pipeline.core import Pipeline# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=train_step)
```

*   验证管道

```
pipeline1.validate()
print("Pipeline validation complete")
```

*   提交管道运行。
*   进行实验并等待管道完成

```
from azureml.core import Experiment# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Spacy_Pipeline_Inferencing').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

*   一旦实验成功，我们就好了。

原文—[samples 2022/spacyetoe . MD at main balakreshnan/samples 2022(github.com)](https://github.com/balakreshnan/Samples2022/blob/main/AzureML/spacyetoe.md)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)