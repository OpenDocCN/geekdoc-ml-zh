# 从图像中查找对象—简单流程

> 原文：<https://medium.com/mlearning-ai/find-objects-from-images-simple-flow-4d86afc389a7?source=collection_archive---------3----------------------->

# Azure 认知服务计算机视觉和自定义视觉

# 使用计算机视觉识别图像中的对象

# 用例

*   识别图像中在一系列场景中常见的对象
*   检查检测到的对象的状况，例如，图像中是否存在某些对象
*   使用自定义视觉检测更多自定义对象
*   使用其他认知服务将人工智能融入这个过程
*   基于事件

# 体系结构

![](img/c9e99f3fb455092a837d90415098538a.png)

# 逻辑流程

![](img/a99485ba4b7d2985b3dbc7f590574b10.png)![](img/f334c1ef4d977287d701af37c1925859.png)![](img/ed521c688f51e12621dec36b7434d0e8.png)

# 密码

*   配置 Azure 存储

![](img/95a6b0c2186a12a83485484c0c061e46.png)

*   现在送去分析图像

![](img/e59da3a9e35188ae69d47f00a59a11df.png)

*   删除现有文件

![](img/68a15d21125fd1aa40a53ea9a2eaa243.png)

*   保存输出

![](img/d152794bf323ad1e6731bfd3309ce399.png)

*   解析 JSON

![](img/a792985ccc52d2f0f867dd766a28105d.png)

```
{
    "properties": {
        "categories": {
            "items": {
                "properties": {
                    "detail": {
                        "properties": {
                            "celebrities": {
                                "type": "array"
                            }
                        },
                        "type": "object"
                    },
                    "name": {
                        "type": "string"
                    },
                    "score": {
                        "type": "number"
                    }
                },
                "required": [
                    "name",
                    "score",
                    "detail"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "description": {
            "properties": {
                "captions": {
                    "items": {
                        "properties": {
                            "confidence": {
                                "type": "number"
                            },
                            "text": {
                                "type": "string"
                            }
                        },
                        "required": [
                            "text",
                            "confidence"
                        ],
                        "type": "object"
                    },
                    "type": "array"
                },
                "tags": {
                    "items": {
                        "type": "string"
                    },
                    "type": "array"
                }
            },
            "type": "object"
        },
        "faces": {
            "items": {
                "properties": {
                    "age": {
                        "type": "integer"
                    },
                    "faceRectangle": {
                        "properties": {
                            "height": {
                                "type": "integer"
                            },
                            "left": {
                                "type": "integer"
                            },
                            "top": {
                                "type": "integer"
                            },
                            "width": {
                                "type": "integer"
                            }
                        },
                        "type": "object"
                    },
                    "gender": {
                        "type": "string"
                    }
                },
                "required": [
                    "age",
                    "gender",
                    "faceRectangle"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "metadata": {
            "properties": {
                "format": {
                    "type": "string"
                },
                "height": {
                    "type": "integer"
                },
                "width": {
                    "type": "integer"
                }
            },
            "type": "object"
        },
        "modelVersion": {
            "type": "string"
        },
        "objects": {
            "items": {
                "properties": {
                    "confidence": {
                        "type": "number"
                    },
                    "object": {
                        "type": "string"
                    },
                    "parent": {
                        "properties": {
                            "confidence": {
                                "type": "number"
                            },
                            "object": {
                                "type": "string"
                            }
                        },
                        "type": "object"
                    },
                    "rectangle": {
                        "properties": {
                            "h": {
                                "type": "integer"
                            },
                            "w": {
                                "type": "integer"
                            },
                            "x": {
                                "type": "integer"
                            },
                            "y": {
                                "type": "integer"
                            }
                        },
                        "type": "object"
                    }
                },
                "required": [
                    "rectangle",
                    "object",
                    "confidence"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "requestId": {
            "type": "string"
        },
        "tags": {
            "items": {
                "properties": {
                    "confidence": {
                        "type": "number"
                    },
                    "name": {
                        "type": "string"
                    }
                },
                "required": [
                    "name",
                    "confidence"
                ],
                "type": "object"
            },
            "type": "array"
        }
    },
    "type": "object"
}
```

*   设定条件

![](img/daec6b6b33667fa0e6583ea1b503954b.png)

*   如果为真，则发送给客户视觉
*   发送到自定义视觉

![](img/d0cb23f94fb2edeba3bd393001a7cea5.png)

*   删除现有文件

![](img/5ff2a8e8090a3d91b48767166e13ff88.png)

*   将数据作为 json 保存到 blob

![](img/5d13a88ce673fa25beaaaedbebbebc2e.png)

原文—[samples 2021/computer vision 1 . MD at main balakreshnan/samples 2021(github.com)](https://github.com/balakreshnan/Samples2021/blob/main/AzureAI/computervision1.md)

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)