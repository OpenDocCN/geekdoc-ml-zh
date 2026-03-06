# AWS Deep Racer 活动主办—现场比赛

> 原文：<https://medium.com/mlearning-ai/aws-deep-racer-event-hosting-in-person-racing-98c25bc9d070?source=collection_archive---------4----------------------->

![](img/2b4d3a1002565b75eee974be5eb4b3ad.png)

Our event set-up in the Atos London offices

这是我的 AWS Deep Racer 系列的第三部分，也是最后一部分。如果你已经直接到达这里，这个系列的第一部分可以在这里查看，主要包括使用多用户模式在单个 AWS 帐户中设置 AWS Deep Racer。第二部分可以在这里看到[，主要是跑一个虚拟的在线比赛。我建议你先读这些，然后再回头读关于赛车的内容。举办现场比赛需要考虑很多因素，所以让我们开始吧…](https://markrosscloud.medium.com/aws-deep-racer-event-hosting-virtual-racing-5a5cd312d90d)

你举办活动的房间需要有一个合适的尺寸。我们印刷的 re:Invent 2018 曲目尺寸为 7.9 米 x 5.2m 米，一些较新的曲目甚至更长！我强烈建议以正确的尺寸打印它，以匹配在虚拟世界中训练的内容，给你的模型最好的机会。你需要考虑你将轨道铺在什么表面上，以及如何保持它伸展。我们以前在瓷砖地板上铺设过我们的赛道，非常棒，这次我们把它铺设在地毯瓷砖地板上，因为它有一点点屈服，我们确实经历了一些褶皱，随着使用的增加，随着人们跟在车的周围，这些褶皱增加了。也有其他选择，例如，你可以在现有的深色地板上用白色胶带制作你的轨道，以保持你的成本最低，但印刷轨道肯定看起来更专业。如果你需要帮助来决定构建什么以及如何构建，那么 AWS 提供了一个很好的指南[这里](https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-build-your-track.html)。

照明和房间里的物品是另一个重要的考虑因素。尝试移除或覆盖汽车可能能够看到的白色物体是一个明智的想法，否则你可能会发现你的模型与他们在虚拟世界中的行为不同。我们不得不覆盖一些白色的支柱，并关闭一些白色背景的屏幕，这些屏幕似乎会干扰汽车。另一项技术是建造栅栏，这项技术有额外的好处，可以在汽车偏离轨道时阻止它们离得太远。我们的障碍是膝盖高，这在逻辑上比你在 AWS re:Invent 和峰会上看到的腰部高的障碍容易得多，但我在观看车载摄像机拍摄的视频时注意到，它可以在某些点上看到障碍。试着让灯光尽可能的一致，避免大量的阴影，或与其他场景相比的特别亮点，我们使用了我们的标准房间内部照明，为了一致性，拉下了百叶窗，因为房间的一半墙壁是落地玻璃。

说到汽车，我强烈建议你拥有多种可用的东西，毕竟就像沃纳·威格尔说的那样“任何东西总是会出故障”!拥有多辆赛车可以在出现故障时提供弹性，所以你可以在排除任何问题的同时继续比赛。它还允许你让生产线运转起来，所以当一辆车在赛道上比赛时，下一辆车可以装载一个模型，以避免赛道上没有车比赛的大量时间，并保持观众的参与。在每辆车的内部增加额外的电池也是一个明智的选择，所以电池可以在赛车比赛时充电。电池和其他备件的详细信息可在[这里](https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-vehicle-chassis-parts.html)找到。关于如何[设置](https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-set-up-vehicle.html)和[校准](https://docs.aws.amazon.com/deepracer/latest/developerguide/deepracer-calibrate-vehicle.html)汽车的详细信息可以在 AWS 网站上找到，你会希望最初校准汽车，然后如果你在一天中开始有糟糕的模型性能，可能需要重新校准，因为严重撞击障碍物可能会影响校准。

在你的设置中增加一些其他有用的东西，比如为每一圈计时的装置和无线网络。从简单的手动计时器(如电话/秒表)，到在起点/终点使用压力传感器的自动计时机制，以及更专业的工作。一个单独的无线路由器也很有用，因为你可以预先配置网络中的所有东西，所以如果你把它们带到不同的办公室，你不必重新配置，所有较高带宽的活动都可以留在本地。

一旦一切就绪，我强烈建议你测试一切。这让你可以测试你的车队，这样每个人都知道他们在做什么，这也让你可以测试环境，以防任何事情对赛车产生不利影响。

我们的面对面比赛是来自虚拟资格赛的前 30 名参与者。从排位赛结束到比赛当天，我们给了他们额外的训练时间来进一步改变和改进他们的模型。我们有大约 20 名来自英国的合格人员可以到我们的伦敦办事处，而另外 10 名合格人员来自更远的地方(美国、危地马拉、丹麦、法国、罗马尼亚和德国)。为了让我们的远程同事参与进来，我们设置了几个网络摄像头从不同的角度捕捉活动，在一些同事的帮助下，网络摄像头和现场领先板流入元宇宙，完成了赛道的展示！

![](img/d0de5377fcdc0920f696c5079f3edaac.png)

AWS Deep Racer goes into the metaverse!

我们给了每位参赛者一段时间来测试他们的模型。一些模型很好地转移到了现实世界，而其他在虚拟世界表现良好的模型则有些挣扎。在机器学习中，这个问题被称为“过度适应”，一些在虚拟世界中表现非常好的模型变得过度依赖训练数据(虚拟环境)，并且不太擅长概括和克服他们以前没有见过的东西(例如，轨道上的褶皱、阴影等)。).对于参与者来说，这是一个很好的学习点，并且确实可以转移到现实世界中，道路不是一个无菌的环境，它可能有垃圾，坑洞或褪色的标记。一旦每个人都有一个回合，我们就邀请前 10 名进行进一步的射击，看看他们的时间是否可以改进。

顶级赛车手的成绩令人印象深刻，有趣的是，现场比赛的领奖台与虚拟资格赛完全不同，前 3 名的个人速度都比他们在虚拟资格赛中的速度快。

![](img/d3f30e99e1073da4583d779f694dfd19.png)

Atos and Cloudreach in-person leaderboard top 12

我们的获胜者，马可，有一个优秀的模型。它不仅是最快的，而且在赛道上始终如一，在他的第一次比赛中，何润跑出了低于 9 秒的成绩，然后在前 10 名的比赛中，他成功地将时间缩短到了令人难以置信的 7.939 秒

Fastest lap from our live finals

在这个过程中，出乎所有人意料的是，他获得了一次由 Atos OneCloud 领导团队成员 Santi Ribas 赠送的免费 re:Invent 之旅！

![](img/71e01554e555dd69eae006d5ba8dd880.png)

Presenter Santi Ribas (left) with our podium left to right (Nickson, Marco, Simon)

![](img/cac750a5d220704f659d6e7c9fb51456.png)

Participants and AWS colleagues who helped to run the event.

最后，我要感谢那些帮助组织和运作这次活动的人。我的 Atos 同事 Matt Knight 和 Neil Clark 在虚拟和物理比赛中提供了帮助，我的同事 Vrushali Malankar 和 Kshitij Bhatnagar 创建了元宇宙。我还要感谢我的 AWS 同事 Sathya Paduchuri、Bharath Sridharan、Rajan Patel、Stuart Lupton、Pete Moles 和 Jenny Vega。

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)