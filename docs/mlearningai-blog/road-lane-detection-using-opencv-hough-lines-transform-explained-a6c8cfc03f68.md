# 道路车道检测使用 OpenCV(霍夫线变换解释)

> 原文：<https://medium.com/mlearning-ai/road-lane-detection-using-opencv-hough-lines-transform-explained-a6c8cfc03f68?source=collection_archive---------0----------------------->

## 送餐机器人可以用这个

在我以前的一篇文章中，我提到我们为我们的顶点项目建造了一个送餐机器人，集成了一些简单的 OpenCV 和 OCR 技巧，使机器人能够检测病人。它的导航完全依靠红外传感器来指引前进的道路。本周，我尝试使用 OpenCV 实现车道检测，以提高其导航能力。使用 OpenCV 的车道检测的很大一部分涉及霍夫线变换，因此我将做我的…