# 停止浏览，这里有你需要的例子(GoogleDrive API + Python)

> 原文：<https://medium.com/mlearning-ai/stop-browsing-heres-the-example-you-need-googledrive-api-python-5b3d3075baaa?source=collection_archive---------5----------------------->

![](img/cbb8b8e03dc8255fa829ed71931414e2.png)

这篇文章包含了一个非常有用的使用 Google Drive API + Python 处理文件的例子。希望对你有帮助！

动机:我很难用谷歌提供的 API 找到一个关于列表、创建、更新等文件的清晰实现。

**背景:**在我目前的工作中，我的团队不得不减少在 Google Drive 上管理文件的人力。所以我开始写脚本。注意，这篇文章只是用 html 文件来解释你，但是代码片段和其他格式是一样的。

**问题:**潜入[谷歌提供的文档](https://developers.google.com/drive/api/v3/about-sdk)，收录的例子自己都不满意。同样的事情也发生在其他博客和 Stackoverflow 的 Q & A 上。我所发现的，并没有解释应该关注的最重要的方面。

**假设**:你是一个大开发者，虽然你不知道如何创建 Google API 密匙，但你的背包里有成吨的文档，所以如果你遵循这个 [gude](https://developers.google.com/workspace/guides/configure-oauth-consent) ，我相信你不会有任何问题。

从这个开始。

导入我们的库(建议:阅读[或](https://pycqa.github.io/isort/)文档。它有助于您改进代码)

```
# Built in packages
import time# Third party packages
import pandas as pd
from apiclient.http import MediaFileUpload# Credentials storaged local
from googleapiclient.discovery import build
from oauth2client.service_account import ServiceAccountCredentials# Local packages
#credentials stored local
from .credentials import gcp_credentials# In my case, i handle with html files.
FILE_NAME = 'file_name.html'
```

这是我还没找到的最重要的提示之一:确保你的文件存在于你指向的回购协议中。这个 API 不够智能，无法识别您的文件是否已经在它的 repo 中。所以如果没有，那就创造。如果是，更新。(没有人期望找到重复的文件)

加载凭据

```
# Load credentials
scope = ['[https://www.googleapis.com/auth/drive'](https://www.googleapis.com/auth/drive')]
gcp_cred = gcp_credentials()
creds = ServiceAccountCredentials.from_json_keyfile_dict(gcp_cred, scope)
```

用凭证实例化 build 方法，并将 mimetype 定义为 text/csv，以明确我们正在处理的文件类型。

```
#Create my google drive service
service = build('drive', 'v3', credentials=creds)
mimetype = 'text/csv'
```

您需要 folder_id 参数。它位于浏览器的顶部，复制字符串，如下图突出显示的例子所示。

![](img/ae28ed871f798699d7a486d2a5b214e8.png)

```
#Example folder id
FOLDER_ID = '000111222333444AAABBBCCCDDDEEE'
```

如前所述，首先你需要列出所有项目包括在您的驱动器。让我们继续前进！

我们需要创建一个[查询参数](https://developers.google.com/drive/api/v3/search-files)并给出 drive_id 作为参数。最后一个在 FOLDER_ID 所在的位置找到。

```
query = f"parents in '{FOLDER_ID}' and trashed = false" 
page_token = None
DRIVE_ID = 'ThisIsAString0123456789'
list_of_files = []
while True:
     response = service.files().list(
                corpora = 'drive',
                driveId = DRIVE_ID,
                q=query,
                pageSize=20,
                fields="nextPageToken, files(id, name)",
                pageToken=page_token,
                includeItemsFromAllDrives = True,
                supportsAllDrives= True).execute()

     list_of_files += response.get('files')
     page_token = response.get('nextPageToken', None)
     if page_token is None:
           break# All files included at our repo
files = []
for file in range(len(list_of_files)):
    if list_of_files[file].get('name'):
        files.append({'name': list_of_files[file].get('name'),
                            'id':list_of_files[file].get('id')})df_files = pd.DataFrame(files)
```

列出所有项目后，该检查文件是否存在，以便创建或替换。

第二个关键提示:创建新文件时，FOLDER_ID 必须是字符串的**列表。如果你不这样做，API 既不会失败也不会出错，所以要小心。**

```
if FILE_NAME in list(df_files.name):
    # Exists
    # YES:Update
    media = MediaFileUpload(FILE_NAME, mimetype=mimetype)
    index = df_files[df_files["name"] == html_name].index
    service.files().update(
                fileId=df_files.loc[index[0], "id"],
                media_body=media,
                supportsAllDrives=True).execute()       
else:    
    # NO: CREATE
    file_metadata = {
                "name": FILE_NAME,
                "parents": FOLDER_ID, #REMEMBER: This is a list
                "mimeType": mimetype,
                    }
    media = MediaFileUpload(html_name,
                            mimetype=mimetype,
                            resumable=True)
    service.files().create(
                body=file_metadata,
                media_body=media,
                supportsAllDrives=True).execute()
```

亲爱的读者，希望这种开发，减少人类的努力，也有助于你成为一个更好的开发人员。如果对你有用，请告诉我，我会阅读你所有的评论。

如果你愿意，可以通过 [LinkedIn](https://www.linkedin.com/in/pablo-rosa-2b1183aa/) 联系我。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)