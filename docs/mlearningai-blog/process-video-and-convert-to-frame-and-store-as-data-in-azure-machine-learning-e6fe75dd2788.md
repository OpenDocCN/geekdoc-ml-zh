# 处理视频并转换为帧，然后作为数据存储在 Azure 机器学习中

> 原文：<https://medium.com/mlearning-ai/process-video-and-convert-to-frame-and-store-as-data-in-azure-machine-learning-e6fe75dd2788?source=collection_archive---------5----------------------->

# 在 Azure 机器学习中处理视频并转换为帧和存储为数据

# 先决条件

*   Azure 机器学习工作区
*   Azure 存储帐户
*   要使用的视频

# 本教程的目标

*   展示如何拍摄视频并转换为帧
*   一旦帧是图像格式，在 Azure 机器学习中创建一个数据集
*   上传图片
*   然后我们可以使用数据标记来标记图像

# 密码

*   确保我们将视频手动上传到笔记本文件夹
*   创建笔记本
*   我选择 python 3.10 和 SDK V2
*   要使用使用 Azure ML SDK V2

```
import cv2
import time
import osfrom matplotlib import pyplot as plttime_start = time.time()
```

*   创建一个函数来一帧一帧地写回 jpg 格式

```
filename = "pinballframe"def video_to_frames(input_loc, output_loc):
    """Function to extract frames from input video file
    and save them as separate frames in an output directory.
    Args:
        input_loc: Input video file.
        output_loc: Output directory to save the frames.
    Returns:
        None
    """
    try:
        os.mkdir(output_loc)
    except OSError:
        pass
    # Log the time
    time_start = time.time()
    # Start capturing the feed
    cap = cv2.VideoCapture(input_loc)
    # Find the number of frames
    video_length = int(cap.get(cv2.CAP_PROP_FRAME_COUNT)) - 1
    print ("Number of frames: ", video_length)
    count = 0
    print ("Converting video..\n")
    # Start converting the video
    while cap.isOpened():
        # Extract the frame
        ret, frame = cap.read()
        if not ret:
            continue
        # Write the results back to output location.
        #cv2.imwrite(output_loc + "/%#05d.jpg" % (count+1), frame)
        cv2.imwrite(output_loc + filename + "%d.jpg" % (count+1), frame)
        #cv2.imshow("Tracking", frame)
        count = count + 1
        # If there are no more frames left
        if (count > (video_length-1)):
            # Log the time again
            time_end = time.time()
            # Release the feed
            cap.release()
            # Print stats
            print ("Done extracting frames.\n%d frames extracted" % count)
            print ("It took %d seconds forconversion." % (time_end-time_start))
            break
```

*   现在用文件夹和视频文件名调用上面的函数
*   提供存储图像的目录名
*   我用的是带 mp4 分机的弹子球视频
*   对于 input_loc，更改视频文件所在的位置
*   output_loc 是代码将单个帧保存为图像的地方

```
if __name__=="__main__": input_loc = 'pinballvideo/WIN_20220809_19_57_13_Pro.mp4'
    output_loc = 'pinballimages1/'
    video_to_frames(input_loc, output_loc)
```

*   可能需要一些时间，这取决于处理了多少帧
*   等待它完成

```
Done extracting frames.
11293 frames extracted
It took 1118 seconds forconversion.
```

*   处理 11293 帧大约需要 18 分钟 c

# 注册数据集

*   现在我们必须创建一个数据集
*   将图像上传到数据集

```
from azure.ai.ml.entities import Data
from azure.ai.ml.constants import AssetTypes
```

*   调用工作空间 ml 客户端来连接和利用工作空间

```
from azure.identity import DefaultAzureCredential
from azure.ai.ml import MLClientcredential = DefaultAzureCredential()
ml_client = None
try:
    #ml_client = MLClient.from_config(credential)
    subscription_id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    resource_group = "rgname"
    workspace = "wkspacename"
    ml_client = MLClient(credential, subscription_id, resource_group, workspace)
except Exception as ex:
    print(ex)
    # Enter details of your AzureML workspace
    subscription_id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    resource_group = "rgname"
    workspace = "wkspacename"
    ml_client = MLClient(credential, subscription_id, resource_group, workspace)
```

*   现在创建数据集并上传

```
my_path = 'pinballimages1'my_data = Data(
    path=my_path,
    type=AssetTypes.MLTABLE,
    description="Pin Ball Images for AI Hackathon",
    name="pinballimages1",
    version='1'
)ml_client.data.create_or_update(my_data)
```

*   等待上传完成
*   仅上传最大 100MB 的文件。

# 下一步是什么(尚未实施)

*   使用数据标记工具来标记图像
*   为图像创建标签
*   一旦在 azure 机器学习中完成数据标记
*   注册为 ML 数据集
*   为 vision 创建一个自动化的 ML 项目并运行该作业。

原文—[samples 2022/proccessvidframedata . MD at main balakreshnan/samples 2022(github.com)](https://github.com/balakreshnan/Samples2022/blob/main/AzureMLV2/ProccessVidframedata.md)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)