# 反刍动物的生长模型

> 原文：<https://medium.com/mlearning-ai/growth-modelling-in-ruminants-1bc62435bd4d?source=collection_archive---------9----------------------->

## 使用混合模型和 R 的三分钟阅读

阅读会耗费你的时间，所以这里只要代码和情节。尽情享受吧！

```
rm(list = ls())
#### LIBRARIES ####
library(readr)
library(foreign)
library(lme4)
library(ggplot2)
library(rms)
library(plyr)
library(Hmisc)
library(reshape2)
library(boot)
library(sjPlot)
library(sjstats)
library(sjmisc)
library(readr)Datalong <- read_csv(".csv")
View(Datalong)
Datalong$...1<-NULL
colnames(Datalong)[4]<-"Baseline"
Datalong$Treat<-as.factor(Datalong$Treat)
Datalong$Block<-as.factor(Datalong$Block)
head(Datalong)
```

![](img/dc76b02b8ba45963988a1c01c68d8edf.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(7,14,21,42,49,56))
myy<-scale_y_continuous(breaks = seq(0, 400, by = 10))
ggplot(Datalong, aes(x=Time, y=BW, group=ID, colour=Treat))+
  myx+
  myy+
  geom_line() +
  stat_summary(aes(group=Treat), fun.y=mean, geom="point", size=3.5)+
  stat_summary(aes(group=Treat), fun.y=mean, geom="line", lwd=1.5)
```

![](img/7ebefd8b7e66102c31b3f4b131f9e29d.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(1,7,14,21,42,49,56))
myy<-scale_y_continuous(breaks = seq(0, 400, by = 10))
ggplot(Datalong, aes(Time, BW, shape=Treat, colour=Treat))+
  stat_summary(fun.data=mean_se, geom="pointrange", lwd=1)+
  myx+
  myy
```

![](img/f3bcf6918655a135dd498c58351ebbe4.png)

```
Grouped<-groupedData(BW~Time|Block/Treat,
                              data=Datalong,
                              FUN=mean)
fit<-lme(BW~ns(Time,4)+Treat, 
             data=Grouped,
             random=~ns(Time,2),
             method="ML", 
             control=lmeControl(opt="optim", maxIter=10000, msMaxIter = 1000))
summary(fit)
VarCorr(fit)
```

![](img/bd7919459b44f5bef2e138015c074555.png)

```
 plot(fit)
qqnorm(fit)
plot(augPred(fit,level=0:2))
plot(acf(resid(fit)))
bwplot(resid(fit)~Datalong$Block)
bwplot(resid(fit)~as.factor(Datalong$ID))
bwplot(resid(fit)~Datalong$Treat)
```

![](img/33384d616d7aa379d92b5523383c38ad.png)![](img/dbd8c26fd1ecd3c0535688c42361bb7f.png)![](img/cfe062054bce21c567f3eb949a63bbdd.png)![](img/1378a178862eba1ba92118f10422ced5.png)![](img/9c5982e50c7f093bdbe4fc996ff9a7c8.png)![](img/94be303e95349e8fb9ddbb0460e26a7b.png)![](img/76f9a60c5e395958eae295b777a70934.png)

```
fit<-lmer(BW~ns(Time,4)+Treat+(ns(Time,2)|Block/Treat), data=Datalong, REML = FALSE)
summary(fit)
ranef(fit)
```

![](img/e039b56672aa9095b049c07697f285f6.png)![](img/b134287feac78e40cdfbf846ef3fe45f.png)

```
plot(fit)
plot_model(fit, type="est")
plot_model(fit, type="re")
plot_model(fit, type="pred")
plot_model(fit, type="diag")
```

![](img/671bf005aba0ffec045b5f2f2e040954.png)![](img/f9b9e54f380c3f8129651067eadddfbd.png)![](img/31b11fc83e46207af6258760e424d8a4.png)![](img/1fa56e50491056ab1100fd412ebc58ae.png)![](img/179d3b81145ea87c7841f29fb7881636.png)![](img/29f702eaf0abeb8ccc1a13a8b4ed605a.png)![](img/9d699dfb769c2d15497b1d9e5917aec0.png)![](img/cf5e7768b8b2bf6bbd994ddce3704715.png)![](img/db22fe674d126703c8770a091e37f2b2.png)![](img/02d404af6b970250c862058df3de5d59.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(1,7,14,21,42,49,56))
myy<-scale_y_continuous(breaks = seq(0, 400, by = 10))
ggplot(Datalong, aes(Time, BW, shape=Treat, colour=Treat))+
  stat_summary(fun.data=mean_se, geom="pointrange", lwd=1, alpha=0.5)+
  stat_summary(aes(y=fitted(fit), linetype=Treat), 
               fun.y=mean, geom = "line", lwd=2)+
  myx+
  myy
```

![](img/e6a35339bcba7fc6725e6d5a9d1a35f5.png)[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)