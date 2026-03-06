# 小猪的生长模型— 2

> 原文：<https://medium.com/mlearning-ai/growth-modeling-in-piglets-2-8f940505551c?source=collection_archive---------4----------------------->

R 中通过混合模型模拟增长的另一个例子

这个例子和我之前[发布的例子](/@marc.jacobs012/growth-modeling-in-piglets-534766dcad1f)有很多相似之处。所以我不会在这上面花太多的文字，让代码和图片自己说话。

因为这是商业数据，我不能与你分享数据集。

```
rm(list = ls())
#### LIBRARIES ####
library(lme4)
library(ggplot2)
library(rms)
library(plyr)
library(reshape2)
library(boot)
library(sjPlot)
library(sjstats)
library(sjmisc)
library(interval)
library(AICcmodavg)
require(parallel) 
library(gridExtra)
library(coefplot) 
library(coda)      
library(aods3)     
library(plotMCMC) 
library(bbmle)    
library(nlme)
library(merTools)
library(RLRsim) 
library(pbkrtest)
library(multcomp)
library(lsmeans)
library(multcompView)
library(lattice)
library(splines)
library(lmtest)
library(mgcv)
library(gamm4)
library(car)### DATA MANAGEMENT ####
library(readxl)
Resultstotalperiod <- read_excel("") 
DAT<-Resultstotalperiod[,c(1:11)]
rm(Resultstotalperiod)
head(DAT)
str(DAT)
summary(is.na(DAT))
```

![](img/d3786cbbb709cbf2c3ef59ba048118f6.png)![](img/104b34fde8ec9cad173722c7da1a8aea.png)

```
## Number of assessments per pig
Z<-!is.na(DAT[,6:10])
DAT[ ,"nr_asses"]<- as.numeric(rowSums(Z))## R code to determine when last pig was observed --> since there is no missing data in between, same results as previous code
last.observed <- function (x, time = NULL) {
  if (is.null (time)) {
    time <- 1:length (x)
  }
  dropout <- sapply (1:length(x),
                     function (i, y) all(y[i:length(y)]), is.na (x))
  ifelse (any (dropout), time[which(dropout)[1]-1], time[length(x)])
}
otime<-c(0,07,14,28,42)
DAT$last.observed <- apply(DAT[,6:10], 1, last.observed)# Missing DATA pattern
DAT$miss<-ifelse(DAT$last.observed==DAT$nr_asses, "Mono","NonMono")
table(DAT$miss) # all monotonic missing data
```

![](img/25e76dca172d07b12f117ce823ae63eb.png)

```
# Create long format
DAT<-as.data.frame(DAT)
DAT$nr_asses<-as.factor(DAT$nr_asses)
DAT$miss<-as.factor(DAT$miss)
IRTA<- reshape(DAT,
               varying  = c("iBW", "7dBW", "14dBW", "28dBW","42dBW"),
               v.names = "BW",
               timevar = "Time",
               times = as.numeric(c("0", "7", "14", "28", "42")),
               direction = "long")
IRTA$id<-NULL
IRTA<-IRTA[order(IRTA$Pig),]
row.names(IRTA)<-NULL
IRTA<-IRTA[!is.na(IRTA$BW),]
dim(IRTA)
```

![](img/bf5548fa4d844e3ecbecafded946cf0e.png)

```
IRTA$Trt<-as.factor(IRTA$Trt)
IRTA$Sexe<-as.factor(IRTA$Sexe)
IRTA$Bloc<-as.factor(IRTA$Bloc)
IRTA$Fate<-as.factor(IRTA$Fate)
IRTA$BW<-as.numeric(IRTA$BW)/1000
levels(IRTA$Fate) <- c("Dead", "Dead", "Slaughter")
IRTA$Fate<-as.character(IRTA$Fate)
IRTA$Fate<-ifelse(is.na(IRTA$Fate), "Finished", IRTA$Fate)
IRTA$Fate<-as.factor(IRTA$Fate)
IRTA$Time_center<-scale(IRTA$Time, center=TRUE, scale = FALSE)
```

![](img/af3cb34520172cfdd6e415514f8f8957.png)

```
## Create dataset with baseline value
IRTA_base<-reshape(DAT,
                   varying  = c("7dBW", "14dBW", "28dBW", "42dBW"),
                   v.names = "BW",
                   timevar = "Time",
                   times = as.numeric(c("7", "14", "28", "42")),
                   direction = "long")
IRTA_base$id<-NULL
IRTA_base<-IRTA_base[order(IRTA_base$Pig),]
row.names(IRTA_base)<-NULL
IRTA_base<-IRTA_base[!is.na(IRTA_base$BW),]
dim(IRTA_base)
IRTA_base$Trt<-as.factor(IRTA_base$Trt)
IRTA_base$Sexe<-as.factor(IRTA_base$Sexe)
IRTA_base$Bloc<-as.factor(IRTA_base$Bloc)
IRTA_base$Fate<-as.factor(IRTA_base$Fate)
IRTA_base$BW<-as.numeric(IRTA_base$BW)/1000
IRTA_base$iBW<-IRTA_base$iBW/1000
levels(IRTA_base$Fate) <- c("Dead", "Dead", "Slaughter")
IRTA_base$Fate<-as.character(IRTA_base$Fate)
IRTA_base$Fate<-ifelse(is.na(IRTA_base$Fate), "Finished", IRTA_base$Fate)
IRTA_base$Fate<-as.factor(IRTA_base$Fate)
IRTA_base$Time_center<-scale(IRTA_base$Time, center=TRUE, scale = FALSE)
```

![](img/2d67e5cc4dedffe79360c30fb269e9d1.png)

```
DAT2<-DAT[!DAT$Week==1, ]
A<-list(IRTA, DAT[, c("Pig", "iBW")])
IRTA<-join_all(A)
IRTA$iBW<-IRTA$iBW/1000TREATmelt<-ddply(IRTA, c("Time", "Trt"), summarise,
                 N = sum(!is.na(BW)),
                 Mis = sum(is.na(BW)),
                 Mean = round(mean(BW, na.rm=T),3),
                 Median = round(median(BW, na.rm=T),3),
                 SD = round(sd(BW, na.rm=T),3),
                 SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                 LCI = round(Mean - (2*SE),3), 
                 HCI = round(Mean + (2*SE),3))SEXmelt<-ddply(IRTA, c("Time", "Sexe"), summarise,
               N = sum(!is.na(BW)),
               Mis = sum(is.na(BW)),
               Mean = round(mean(BW, na.rm=T),3),
               Median = round(median(BW, na.rm=T),3),
               SD = round(sd(BW, na.rm=T),3),
               SE = round(sd(BW, na.rm=T) / sqrt(N),5),
               LCI = round(Mean - (2*SE),3), 
               HCI = round(Mean + (2*SE),3))
```

![](img/5b49c74404776821cb508d07605a9ddd.png)

```
SEXTREATmelt<-ddply(IRTA, c("Time", "Sexe", "Trt"), summarise,
                    N = sum(!is.na(BW)),
                    Mis = sum(is.na(BW)),
                    Mean = round(mean(BW, na.rm=T),3),
                    Median = round(median(BW, na.rm=T),3),
                    SD = round(sd(BW, na.rm=T),3),
                    SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                    LCI = round(Mean - (2*SE),3), 
                    HCI = round(Mean + (2*SE),3))
```

![](img/7008ceae190ef0a2dff02056dd9fefb9.png)

```
BLOCmelt<-ddply(IRTA, c("Time", "Bloc", "Trt"), summarise,
                N = sum(!is.na(BW)),
                Mis = sum(is.na(BW)),
                Mean = round(mean(BW, na.rm=T),3),
                Median = round(median(BW, na.rm=T),3),
                SD = round(sd(BW, na.rm=T),3),
                SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                LCI = round(Mean - (2*SE),3), 
                HCI = round(Mean + (2*SE),3))ASSESSmelt<-ddply(IRTA, c("Time","nr_asses"), summarise,
                  N = sum(!is.na(BW)),
                  Mis = sum(is.na(BW)),
                  Mean = round(mean(BW, na.rm=T),3),
                  Median = round(median(BW, na.rm=T),3),
                  SD = round(sd(BW, na.rm=T),3),
                  SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                  LCI = round(Mean - (2*SE),3), 
                  HCI = round(Mean + (2*SE),3))ASSESSBLOCmelt<-ddply(IRTA, c("Time", "Bloc","nr_asses"), summarise,
                      N = sum(!is.na(BW)),
                      Mis = sum(is.na(BW)),
                      Mean = round(mean(BW, na.rm=T),3),
                      Median = round(median(BW, na.rm=T),3),
                      SD = round(sd(BW, na.rm=T),3),
                      SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                      LCI = round(Mean - (2*SE),3), 
                      HCI = round(Mean + (2*SE),3))FATEmelt<-ddply(IRTA, c("Time", "Fate"), summarise,
                N = sum(!is.na(BW)),
                Mis = sum(is.na(BW)),
                Mean = round(mean(BW, na.rm=T),3),
                Median = round(median(BW, na.rm=T),3),
                SD = round(sd(BW, na.rm=T),3),
                SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                LCI = round(Mean - (2*SE),3), 
                HCI = round(Mean + (2*SE),3))FATETREATmelt<-ddply(IRTA, c("Time", "Fate", "Trt"), summarise,
                     N = sum(!is.na(BW)),
                     Mis = sum(is.na(BW)),
                     Mean = round(mean(BW, na.rm=T),3),
                     Median = round(median(BW, na.rm=T),3),
                     SD = round(sd(BW, na.rm=T),3),
                     SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                     LCI = round(Mean - (2*SE),3), 
                     HCI = round(Mean + (2*SE),3))xlabs <- paste(levels(IRTA$Time),"\n(N=",table(IRTA$Time), ")",sep="")
```

![](img/5fe70382379eb73d4c9682e99929e277.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42), labels=xlabs)
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(ASSESSmelt, aes(x=Time, y=Mean, colour=nr_asses))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_point()
```

![](img/bd2c47f7b2d43d3a9d0d61a46e7999dc.png)

BW over time as function of number of assessments

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42), labels=xlabs)
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(ASSESSBLOCmelt, aes(x=Time, y=Mean, colour=nr_asses))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_point()+
  facet_wrap(~Bloc)
```

![](img/3284c14e0ab26cf4bd6074e14dbb614d.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42), labels=xlabs)
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(FATEmelt, aes(x=Time, y=Mean, colour=Fate))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_point()theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(FATETREATmelt, aes(x=Time, y=Mean, colour=Fate))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_point()+
  facet_wrap(~Trt)
```

![](img/7640f698db7d3028c68a3f3f4a812dd3.png)![](img/7c65331c196467304b63733f5698db43.png)

```
droptime<-table(DAT$last.observed)
ndrop<-cumsum(droptime)
ndrop<-1-ndrop[-5]/ndrop[5]
ndrop<-as.vector(ndrop)
time<-c(0,7,14,28,42)
xyplot(ndrop~time[-1], 
       type = "o", col=2, lwd=2,
       main = "BW drop out pattern",
       ylab = "Proportion Still in Study",
       xlab = "Time (Days)",
       scales=list(x=list(at=c(0,7,14,28,42)), y=list(at=c(0,0.1,0.2,0.3,0.4,0.5,0.6,0.7,0.8,0.9,1), limits=c(0,1))))
```

![](img/b31a7f7af1a0ed99d9dc5a2b526e087d.png)

```
xyplot(log(BW)~ Time_center|Pig, data = IRTA, 
       strip = FALSE, aspect = "xy", pch = 16, cex=0.2, grid = TRUE,
       panel = function(x, y, ..., fit, subscripts) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         fm<-lm(y~ns(x,2))
         panel.lines(x, fitted(fm), lty=2, col.line="red")
         fm2<-lm(y~ns(x,3))
         panel.lines(x, fitted(fm2), lty=2, col.line="green")
         ypred.mixed<- fitted(fit.full.mixed_nodead6.3)[subscripts]
         panel.lines(x, ypred.mixed, col = "orange")
         ypred.mixed2<- fitted(fit_nlme)[subscripts]
         panel.lines(x, ypred.mixed2, col = "purple")
         panel.abline(0, 0)
       } )
```

![](img/83cf4f8f03730f2038e246467fe17f03.png)

A two-knot spline is probably best when on log scale, which is strange , not sure if natural

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(TREATmelt, aes(x=Time, y=Mean, colour=Trt))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Trt), alpha=0.1)+
  facet_wrap(~Trt)
```

![](img/61c438defc900c41a7017c6600e39a5b.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(SEXmelt, aes(x=Time, y=Mean, colour=Sexe))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Sexe), alpha=0.1)+
  facet_wrap(~Sexe)theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(SEXTREATmelt, aes(x=Time, y=Mean, colour=Sexe))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Sexe), alpha=0.1)+
  facet_wrap(~Trt)theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(BLOCmelt, aes(x=Time, y=Mean, colour=Bloc))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Bloc), alpha=0.1)+
  facet_wrap(~Trt)theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(BLOCmelt, aes(x=Time, y=Mean, colour=Trt))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Trt), alpha=0.1)+
  facet_wrap(~Bloc)
```

![](img/178b4cdaa84b92c245e396c067b34e22.png)![](img/fdfec71a9ce4459c335954c41adb16bf.png)![](img/31f9a92c1ebd8f0696910e62ebd59a29.png)![](img/bfbea062294f6e8bfc2de39410824488.png)

```
sjp.poly(IRTA$BW,IRTA$Time, 1)
sjp.poly(IRTA$BW,IRTA$Time, 2) # seems enough
sjp.poly(IRTA$BW,IRTA$Time, 3)
```

![](img/4918d2b0bb2b869ae64c6a550005cbb1.png)![](img/5bd644356d11224aabc5031551c0386d.png)![](img/bd6306f0954e45cfd53edfb1f8c84e3d.png)

How many polynomial terms do I need for raw BW data?

```
sjp.poly(log(IRTA$BW),IRTA$Time, 1)
sjp.poly(log(IRTA$BW),IRTA$Time, 2) 
sjp.poly(log(IRTA$BW),IRTA$Time, 3)# seems enough
sjp.poly(log(IRTA$BW),IRTA$Time, 4)
```

![](img/502cca9d2002b45e9784a6d3b62d3210.png)![](img/0838d4e4a70004114b1fb81a93a9f87e.png)![](img/63f9435bee0cd781a9ed589b10d3531a.png)![](img/0b520ee89e2dbe30723683bb5968b7de.png)

How many polynomial terms do I need for log BW data?

```
densityplot(~IRTA_nodead$BC_BW|IRTA_nodead$Trt*IRTA_nodead$Time)
densityplot(~IRTA_nodead$BC_BW|IRTA_nodead$Sexe*IRTA_nodead$Time)
densityplot(~IRTA_nodead$BC_BW|IRTA_nodead$Bloc*IRTA_nodead$Time)
densityplot(~IRTA_nodead$BC_BW|IRTA_nodead$Lot*IRTA_nodead$Time) 
densityplot(~IRTA_nodead$BC_BW|IRTA_nodead$Time) 
densityplot(IRTA_nodead$BC_BW)
densityplot(IRTA_nodead$BW) 
densityplot(~IRTA_nodead$BW|IRTA_nodead$Time)
```

![](img/6dda41fe835359dfaf656e208c1c6474.png)![](img/d865433d4e21faa08a9d2c17f9299014.png)![](img/d5e93e9e307735291d9a8211d796c3ef.png)![](img/62053ee6630350e511ffb44f5792e52e.png)![](img/da82563dfb3fc539332aabe68a60c982.png)![](img/bc9f98ff8f292912734f49d4ba154e07.png)![](img/2c09b37ff45fc5257a750a7f13285493.png)

```
IRTA<-IRTA[complete.cases(IRTA), ]
## Pooled
fit.pooled<-lm(BW~Time, data=IRTA)
summary(fit.pooled)
IRTA$fit.pooled<-fitted(fit.pooled)
## Unpooled
fit.unpooled<-lm(BW~Time+Bloc-1, data=IRTA)
summary(fit.unpooled)
IRTA$fit.unpooled<-fitted(fit.unpooled)
## Mixed model 
fit.mixed<-lmer(BW~Time+(1|Bloc), data=IRTA)
summary(fit.mixed)
fixef(fit.mixed)
ranef(fit.mixed)
IRTA$fit.mixed<-fitted(fit.mixed)
```

![](img/4898e3de4172790f88fdde5364e4434e.png)

```
fit.pooled<-augment(fit.pooled)
fit.unpooled<-augment(fit.unpooled)
fit.mixed<-augment(fit.mixed)theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 20, by=5))
ggplot(data=IRTA, aes(x=Time, y=fit.pooled, group=Bloc))+
  stat_summary(aes(colour="fit.pooled"), fun.y="mean", geom="point", alpha=0.75)+
  stat_summary(aes(colour="fit.pooled"), fun.y="mean", geom="line", alpha=0.75)+  stat_summary(aes(y=fit.unpooled,colour="fit.unpooled"),fun.y="mean", geom="line", alpha=0.75)+
  stat_summary(aes(y=fit.unpooled, colour="fit.unpooled"),fun.y="mean", geom="line", alpha=0.75)+
  stat_summary(aes(y=fit.mixed, colour="fit.mixed"), fun.y="mean", geom="line",alpha=0.75)+
  stat_summary(aes(y=fit.mixed, colour="fit.mixed"),fun.y="mean", geom="line", alpha=0.75)+
  facet_wrap(~Bloc)+
  myy+
  myx+
  labs(title = "Difference in fitted values", y="Fitted values", x="Time (Days)")+
  scale_colour_manual(name="Fit",values=c(fit.pooled="blue", fit.unpooled="red", fit.mixed="green"))
```

![](img/bf6b3095099efca022348a9e49012054.png)

Comparing pooled, unpooled, and mixed model → not so big apart

```
## Pooled
fit.full.pooled<-lm(BW~ns(Time_center,2)*Trt+Sexe, data=IRTA)
IRTA$fit.full.pooled<-fitted(fit.full.pooled)
AIC(fit.full.pooled)## Unpooled
fit.full.unpooled<-lm(BW~ns(Time_center,2)*Trt+Sexe+Bloc-1+Pig-1, data=IRTA)
IRTA$fit.full.unpooled<-fitted(fit.full.unpooled)
AIC(fit.full.unpooled)## Mixed model
fit.full.mixed<-lmer(BW~ns(Time_center,2)*Trt+Sexe+(Time_center|Bloc/Pig), data=IRTA) # failed to converge
IRTA$fit.full.mixed<-fitted(fit.full.mixed)fit.full.mixed2<-lmer(BW~ns(Time_center,2)*Trt+Sexe+(1|Bloc/Pig), data=IRTA) # failed to converge
anova(fit.full.mixed2, fit.full.mixed) 
```

![](img/df315ae801c0bcb0de34945bdaba4ef4.png)

```
fit.full.mixed3<-lmer(BW~ns(Time_center,2)*Trt+Sexe+(Time_center|Bloc:Pig), data=IRTA) # failed to converge
anova(fit.full.mixed3, fit.full.mixed2, fit.full.mixed) # 
```

![](img/743c289ebc410bcd30d90d3d6f7001ad.png)

**Fit.full.mixed** is best, but correlation is almost one between Time and Slope.

```
bwplot(resid(fit.full.mixed)~IRTA$Bloc)
bwplot(resid(fit.full.mixed)~IRTA$Trt)
bwplot(resid(fit.full.mixed)~IRTA$Trt|IRTA$Bloc)
```

![](img/12dfe93e3e8fe41e1c076f9d8116676b.png)![](img/d6c149139f40bfc5f39d70b8fc8a536f.png)![](img/f1c1199eb999919bb7b331f29d5cb723.png)

```
fit.full.mixed.base<-lmer(BW~iBW+ns(Time_center,2)*Trt+Sexe+(Time_center|Bloc/Pig), data=IRTA) # failed to converge
IRTA$fit.full.mixed.base<-fitted(fit.full.mixed.base)
plot(fit.full.mixed.base)
```

![](img/ff0061b60e7d786e5632a37539d2aef3.png)

Mixed model with baseline covariate

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 20, by=5))
ggplot(IRTA, aes(x=Time, y=fit.full.pooled))+
  myy+
  myx+
  stat_summary(aes(colour="fit.pooled", fun.y="mean", geom="point", alpha=0.75))+
  stat_summary(aes(colour="fit.pooled", fun.y="mean", geom="line", alpha=0.75))+
  stat_summary(aes(y=fit.full.unpooled,colour="fit.unpooled"),fun.y="mean", geom="line", alpha=0.75)+
  stat_summary(aes(y=fit.full.unpooled, colour="fit.unpooled"),fun.y="mean", geom="line", alpha=0.75)+
  stat_summary(aes(y=fit.full.mixed.base, colour="fit.mixed"), fun.y="mean", geom="line",alpha=0.75)+
  stat_summary(aes(y=fit.full.mixed.base, colour="fit.mixed"),fun.y="mean", geom="line", alpha=0.75)+
  stat_summary(aes(y=fit.full.mixed, colour="fit.mixed"), fun.y="mean", geom="line",alpha=0.75)+
  stat_summary(aes(y=fit.full.mixed, colour="fit.mixed"),fun.y="mean", geom="line", alpha=0.75)+
  facet_wrap(~Bloc)+
  labs(title = "Difference in fitted values", y="Fitted values", x="Time (Days)")+
  scale_colour_manual(name="Fit",values=c(fit.pooled="blue",fit.unpooled="red",fit.mixed="green",fit.mixed.baseline="orange"))
```

![](img/3dc12cde981db6a43a818903acd77435.png)

Comparison of technique by fitted values.

```
lattice.options(
  layout.heights=list(bottom.padding=list(x=0), top.padding=list(x=0)),
  layout.widths=list(left.padding=list(x=0), right.padding=list(x=0))
)
xyplot(BW ~ Time|Pig, data = IRTA,
       strip = FALSE, aspect = "xy", pch = 16, cex=0.5, grid = FALSE,
       panel = function(x, y, ..., fit, subscripts) 
       {
         panel.xyplot(x, y, ...)
         ypred.pooled <- fitted(fit.full.pooled)[subscripts]
         panel.lines(x, ypred.pooled, col = "black")
         ypred.unpooled <- fitted(fit.full.unpooled)[subscripts]
         panel.lines(x, ypred.unpooled, col = "red")
         ypred.mixed <- fitted(fit.full.mixed)[subscripts]
         panel.lines(x, ypred.mixed, col = "green")
         ypred.mixed.base <- fitted(fit.full.mixed.base)[subscripts]
         panel.lines(x, ypred.mixed.base , col = "orange")
       },
       xlab = "Time",  ylab = "BW (kg)",key=list(space="top",
                                                 lines=list(col=c("black","red", "green","orange"), lwd=6),
                                                 text=list(c("LM Pooled", "LM Unpooled", "Mixed model","Mixed model with baseline covariate"), columns=1))
)
```

![](img/15865c8852d6e9c2d2506e86b0da87a4.png)

Model fit, but only for Bloc

```
IRTA_nodead<-IRTA[IRTA$Fate=="Finished"|IRTA$Fate=="Slaughter",]
TREATmelt_nodead<-ddply(IRTA_nodead, c("Time", "Trt"), summarise,
                        N = sum(!is.na(BW)),
                        Mis = sum(is.na(BW)),
                        Mean = round(mean(BW, na.rm=T),3),
                        Median = round(median(BW, na.rm=T),3),
                        SD = round(sd(BW, na.rm=T),3),
                        SE = round(sd(BW, na.rm=T) / sqrt(N),5),
                        LCI = round(Mean - (2*SE),3), 
                        HCI = round(Mean + (2*SE),3))theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,14,28,42))
myy<-scale_y_continuous(breaks = seq(0, 35, by = 1))
ggplot(TREATmelt_nodead, aes(x=Time, y=Mean, colour=Trt))+
  myx+
  myy+
  geom_line(lwd=1)+
  geom_ribbon(aes(ymin=LCI, ymax=HCI, fill=Trt), alpha=0.1)
```

![](img/ce66a1b1d0272fe8146c19c70493e675.png)

Asses data excluding pigs that died

```
IRTA_nodead$Time_center<-scale(IRTA_nodead$Time, center=TRUE, scale=FALSE)
fit.full.mixed_nodead<-lmer(BW~ns(Time_center,2)*Trt+Sexe+
                              (ns(Time_center,2)|Bloc/Pig), 
                            data=IRTA_nodead)jtools::plot_summs(fit.full.mixed, 
                   fit.full.mixed_nodead,
                   plot.distributions = TRUE,
                   rescale.distributions = TRUE,
                   model.names = c("fit.full.mixed", "fit.full.mixed_nodead"))
```

![](img/a5ffb29706df593768fa1b51215926e5.png)

Box 和 Cox (1964)提出了一系列变换，旨在减少线性模型中误差的非正态性。事实证明，这样做通常也会降低非线性。为了减少右偏，求根、对数或倒数(根是最弱的)。这是实践中最常见的问题。为了减少左偏斜，采取正方形或立方体或更高的权力。

常见的 Box-Cox 变换—λ标准变换
#-2→1/y2
#-1→1/y1
#-0.5→1/(sqrt(y))
# 0→log(y)
# 0.5→sqrt(y)
# 1→y
# 2→y^2

```
fit<-lm(BW~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead)
bc1<-boxcox(BW~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead)
boxcox(fit,lambda=seq(-0.5,0,by=0.05),plotit=T)
which.max(bc1$y)
LV<-bc1[1]$x[as.numeric(which.max(bc1$y))]
LV
fitbc<-lm((BW^LV-1/LV)~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead)
fitreciproc<-lm((1/BW)~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead) # reciprocal
fitreciprocsqrt<-lm((1/sqrt(BW))~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead) # reciprocal square root 
fitlog<-lm(log(BW)~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead)
fitsqrt<-lm(sqrt(BW)~ns(Time_center,3)*Trt+Sexe, data=IRTA_nodead)par(mfrow=c(2,3))
plot(fit, which=2, main="BW")
plot(fitbc, which=2, main="BW_bc") 
plot(fitlog, which=2, main="BW_log") 
plot(fitsqrt, which=2, main="BW_sqrt")
plot(fitreciproc, which=2, main="BW_reciprocal") 
plot(fitreciprocsqrt, which=2, main="BW_reciprocalsqrt")
```

![](img/4a6a5616d5fe836b576c6e1a90e355d8.png)![](img/45ad8a2537ce1f3d38bd9e2d186dcc90.png)![](img/bf9194f584c383042338c9c7afc06698.png)

Plot several QQ plots based on transformations

```
histogram(~BW|Time,data=IRTA_nodead,type="density", breaks=30)
histogram(~log(BW)|Time,data=IRTA_nodead,type="density", breaks=30)
histogram(~BC_BW|Time,data=IRTA_nodead,type="density", breaks=30)
histogram(~BW^LV|Time,data=IRTA_nodead,type="density", breaks=30)
```

![](img/6ed5faa699535bf94735df69a8e94502.png)![](img/10a2623cdbdb05415dd4a40376a045ba.png)![](img/332cdda845c7bdf686e0698d29c8f155.png)

```
fit.full.mixed_nodead<-lmer(BW~ns(Time_center,2)*Trt+Sexe+
                              (ns(Time_center,2)|Bloc/Pig), 
                            data=IRTA_nodead)
fit.full.mixed_nodead2<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                               (ns(Time_center,2)|Bloc/Pig), 
                             data=IRTA_nodead)
fit.full.mixed_nodead3<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                               (ns(Time_center,2)|Bloc)+
                               (ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)
fit.full.mixed_nodead4<-lmer(BC_BW~ns(Time_center,2)*Trt+Sexe+
                               (ns(Time_center,2)|Bloc)+
                               (ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)
fit.full.mixed_nodead5<-lmer(BC_BW~ns(Time_center,2)*Trt+Sexe+
                               (1|Bloc)+
                               (1|Pig)+
                               (0+ns(Time_center,2)|Bloc)+
                               (0+ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)
fit.full.mixed_nodead6<-lmer(BC_BW~ns(Time_center,2)*Trt+Sexe+
                               (1|Bloc)+
                               (ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)
fit.full.mixed_nodead6.1<-lmer(BC_BW~ns(Time_center,3)*Trt+Sexe+
                                 (1|Bloc)+
                                 (ns(Time_center,3)|Pig), 
                               data=IRTA_nodead)
fit.full.mixed_nodead8<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                               (1|Bloc)+
                               (ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)
anova(fit.full.mixed_nodead4, 
      fit.full.mixed_nodead5, 
      fit.full.mixed_nodead6,
      fit.full.mixed_nodead6.1) # 6.1 provides best fit
```

![](img/dfbb65c7c4959a594a26819bbfeb5221.png)

```
fit.full.mixed_nodead6.2<-lmer(log(BW)~ns(Time_center,3)*Trt+Sexe+
                                 (1|Bloc)+
                                 (ns(Time_center,3)|Pig), 
                               data=IRTA_nodead)
fit.full.mixed_nodead6.2.1<-lmer((1/BW)~ns(Time_center,3)*Trt+Sexe+
                                   (1|Bloc)+
                                   (ns(Time_center,3)|Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.2<-lmer((1/sqrt(BW))~ns(Time_center,3)*Trt+Sexe+
                                   (1|Bloc)+
                                   (ns(Time_center,3)|Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.3<-lmer((1/sqrt(BW))~ns(Time_center,3)*Trt+Sexe+Fate+
                                   (1|Bloc)+
                                   (ns(Time_center,3)|Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.4<-lmer(log(BW)~ns(Time_center,2)*Trt*Sexe+Fate+
                                   (1|Bloc)+
                                   (ns(Time_center,2)|Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.5<-lmer(log(BW)~ns(Time_center,2)*Trt*Sexe+Fate+
                                   (ns(Time_center,2)|Bloc/Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.6<-lmer(log(BW)~ns(Time_center,2)+Trt+Sexe+Fate+
                                   (ns(Time_center,2)|Bloc/Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.2.7<-lmer(log(BW)~ns(Time_center,2)+Trt+Sexe+nr_asses+
                                   (1|Bloc)+
                                   (1|Lot)+
                                   (ns(Time_center,2)|Pig), 
                                 data=IRTA_nodead)
fit.full.mixed_nodead6.3<-lmer(log(BW)~ns(Time_center,3)*Trt+Sexe+
                                 (1|Bloc)+
                                 (1|Pig)+
                                 (0+ns(Time_center,3)|Pig), 
                               data=IRTA_nodead)
anova(fit.full.mixed_nodead6.2,
      fit.full.mixed_nodead6.3, 
      fit.full.mixed_nodead6.2.4, 
      fit.full.mixed_nodead6.2.5,
      fit.full.mixed_nodead6.2.6,
      fit.full.mixed_nodead6.2.7) # 6.2 best for log (BW)
```

![](img/d4901c9edaff60619f1a75773b169d94.png)

```
fit.full.mixed_nodead10<-glmer(BW~ns(Time_center,2)*Trt+Sexe+
                                 (1|Bloc)+
                                 (0+ns(Time_center,2)|Pig), 
                               family=inverse.gaussian(link="1/mu^2"), 
                               control = glmerControl(optimizer = "bobyqa", nAGQ = 1),
                               data=IRTA_nodead)RMSE.merMod(fit.full.mixed_nodead6.2)
RMSE.merMod(fit.full.mixed_nodead6.3)
RMSE.merMod(fit.full.mixed_nodead6.1) 
```

![](img/65d350f55d96d83147db335efe8a8c67.png)

```
## What if I only kept the finishers
IRTA_finished<-IRTA[IRTA$Fate=="Finished",]
fit.full.mixed_finished<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                                (ns(Time_center,2)|Bloc/Pig), 
                              data=IRTA_finished) 
plot(fit.full.mixed_finished)
plot_model(fit.full.mixed_finished, type="est") 
```

![](img/e87b3f427dd7d00e9df974fb7e64cd41.png)

```
jtools::plot_summs(fit.full.mixed_nodead6.2,
                   fit.full.mixed_nodead6.3, 
                   fit.full.mixed_nodead6.2.4, 
                   fit.full.mixed_nodead6.2.5,
                   fit.full.mixed_nodead6.2.6,
                   fit.full.mixed_nodead6.2.7, 
                   robust = "HC3", 
                   model.names = c("6.2",
                                   "6.3", 
                                   "6.2.4", 
                                   "6.2.5",
                                   "6.2.6",
                                   "6.2.7"))
```

![](img/a526b603b7c42c03ce1ca040ebf7cd64.png)

```
Grouped<-groupedData(BW~Time_center|Bloc/Pig,
                     data=IRTA_nodead,
                     FUN=mean)
fit_nlme<-lme(BW~ns(Time_center,2)*Trt+
                Sexe, 
              data=IRTA_nodead_deleted,
              random=~ns(Time_center,2)|Bloc/Pig,
              method="ML", 
              control=lmeControl(opt="optim", maxIter=1000000, msMaxIter = 10000))
plot(fit_nlme)
```

![](img/9e640f2ec45db0742d4b0fee953186d3.png)

Homogeneity of variances indicates normal distrbution of errors

```
fit_nlme_log<-lme(log(BW)~ns(Time_center,2)*Trt+
                    Sexe, 
                  data=IRTA_nodead,
                  random=~ns(Time_center,2)|Bloc/Pig,
                  method="ML", 
                  control=lmeControl(opt="optim", maxIter=1000000, msMaxIter = 10000))
plot(fit_nlme_log)
plot(fit.full.mixed_nodead2)
plot(fit_nlme_log, resid(., type = "p") ~ Time_center|Bloc,abline=0) 
plot(fit_nlme_log, resid(., type = "p") ~ Time_center|Sexe,abline=0) 
plot(fit_nlme_log, resid(., type = "p") ~ Time_center|Trt,abline=0) 
plot(fit_nlme_log, resid(., type = "p") ~ Time_center|Fate,abline=0)
```

![](img/89c85a664b235f36218e4d44d84bc60f.png)![](img/ded96ab61d700084fe55eda3487f0e43.png)![](img/ce962c4a4a55207baff58023c650967c.png)![](img/776501508e8d67dbfd1d073840d2ae1d.png)

```
fit.mixed.pigs<-lmer(BC_BW~ns(Time_center,2)*Trt+Sexe+
                       (ns(Time_center,2)|Pig), 
                     data=IRTA_nodead)inf.Pig<-influence.ME::influence(fit.mixed.pigs, group="Pig")
plot(inf.Pig, which="cook", cutoff=4/368)
cooks.distance(inf.Pig, sort=TRUE)
```

![](img/3b479b7d9a85caf6ee49d4096ac426a5.png)

INFLUENCE STATISTICS

```
IRTA_nodead_deleted<-IRTA_nodead[!IRTA_nodead$Pig %in% c(187, 398, 174,167), ] 
dim(IRTA_nodead);dim(IRTA_nodead_deleted) # 20 rows missingfit.full.mixed_nodead6<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                               (1|Bloc)+
                               (ns(Time_center,2)|Pig), 
                             data=IRTA_nodead)fit.full.mixed_nodead7<-lmer(log(BW)~ns(Time_center,2)*Trt+Sexe+
                               (1|Bloc)+
                               (0+ns(Time_center,2)|Pig), 
                             data=IRTA_deleted) 
ICtab(fit.full.mixed_nodead6,fit.full.mixed_nodead7)
```

![](img/b72a13892ae474a71192aea9b5a0cdc4.png)

Analysis with and without deleted pigs

```
Anova(fit.full.mixed_nodead7, type=3, test.statistic = "F")
```

![](img/3c9ee79cbe6478915a06f57698ae5dad.png)

```
plot(fit.full.mixed_nodead7)
plot(fit.full.mixed_nodead6)
par(mfrow = c(3, 1))
qqp(resid(fit.full.mixed_nodead7))
qqp(resid(fit.full.mixed_nodead7), "lnorm")
qqp(IRTA_nodead$BW, "lnorm") # not a good fit
```

![](img/bb6b34efa4da6c2f59935886b3465dcb.png)![](img/2f49f94bf744ad2409a051c26665eb6e.png)![](img/1eb9cfd37e7ff52bcec8520a47f7dec8.png)

```
IRTAcomplete<-IRTA_nodead_deleted
final.model<-lmer(1/(sqrt(BW))~ns(Time,3)*Trt+Sexe+Fate+
                    (ns(Time,3)|Bloc/Pig),data=IRTAcomplete)
VarCorr(final.model)
fixef(final.model)
ranef(final.model)
```

![](img/7e5ebf000f955705b1840ff08bf75f87.png)

```
summary(final.model)
plot(final.model)
```

![](img/408b0d320c4228c2a58692c83ae4f176.png)

```
RMSE.merMod(final.model)
[1] 0.004179619qqp(resid(final.model))
```

![](img/157a84c7275884ca43a12037424d7c85.png)

```
plot_model(final.model)
plot_model(final.model, type="re")
plot_model(final.model, type="diag")
```

![](img/79f19ea47617eaadfa4ea0b22cbdb7ac.png)![](img/80e551fb5eaa8d40723629217db59bc3.png)![](img/8252da02945e72f312d242c65455bca2.png)![](img/39061cf889247458d3598d554b2cb9c5.png)![](img/dfd637ca6f47f730328a4af337260796.png)![](img/40c3406a4d771980557e81c08af51b22.png)![](img/efda2e6ddf11d8ec2845c89dfd09fe64.png)![](img/eecdd305774cc5915cc309c97c5003fe.png)

```
 Anova(final.model, type=3, test.statistic = "F") 
```

![](img/8b3e7350ea3dbab9586f863def3ec90c.png)

```
hnp::hnp(final.model)
```

![](img/579564500635e88016ffa2fed9829b78.png)

```
library(orcutt)
fit.pooled<-lm((BW^LV-1/LV)~ns(Time,3)*Trt+Sexe, data=IRTAcomplete)
fit.pooled2<-lm((BW^LV-1/LV)~ns(Time,3)*Trt+Sexe+as.factor(Fate)+as.factor(Bloc), data=IRTAcomplete) # closest to full model 
rho<-orcutt::cochrane.orcutt(fit.pooled)
rho2<-orcutt::cochrane.orcutt(fit.pooled2)
rho$rho # [1] 0.7202645
rho2$rho # [1] 0.635619plot(final.model, resid(.) ~ Time, abline = 0 ) 
bwplot(resid(final.model)~IRTA_nodead$Bloc)
bwplot(resid(final.model)~IRTA_nodead$Trt)
bwplot(resid(final.model)~IRTA_nodead$Sexe)
bwplot(resid(final.model)~IRTA_nodead$Fate)
bwplot(IRTA_nodead$Time~resid(final.model))
bwplot(resid(final.model)~IRTA_nodead$Pig)
```

![](img/ce5982e53becc745d431d7c46648aacd.png)![](img/13ce797a60c3cb4d50cbe83cae2f7172.png)![](img/34c50003e933aa491a5b64e0b5f8369c.png)![](img/36659d0367f4bbe0fa5b47adf2f45393.png)![](img/1342c88c9fab9ad53d69b56a095060f3.png)![](img/0f7c01562ed8d06d1750c5fcc151d74d.png)![](img/6e8029f980f1b095809d9eec458478fb.png)

最后的方法，看看是否 lme4 和 nlme 的预测纳入自相关更好。

```
final.model<-lmer(1/(sqrt(BW))~ns(Time,3)*Trt+Sexe+Fate+
                    (ns(Time,3)|Bloc/Pig),data=IRTAcomplete)
final.model_nlme<-lme(1/(sqrt(BW))~ns(Time,3)*Trt+Sexe+Fate, 
                      data=IRTAcomplete,
                      random=~ns(Time,3)|Bloc/Pig,
                      correlation=corAR1(0.7, form=~1|Bloc/Pig),
                      method="ML", 
                      control=lmeControl(opt="optim", maxIter=1000000, msMaxIter = 10000))
AICtab(final.model, final.model_nlme) # nlme model way better
```

![](img/61faa736a8316507deaf9091dbb66261.png)

```
RMSE.merMod(final.model)
rmse(final.model_nlme) 
```

![](img/0269e2f289cef7328440958d5e202935.png)

```
qqp(resid(final.model))
qqp(resid(final.model_nlme))
```

![](img/fc2755ab1774fee34c1c1e8942fd65d6.png)![](img/e0e9193ffcb7dcd56c533b3112f85c54.png)

```
IRTAcomplete$fit2<-1/(fitted(final.model_nlme)^2)
IRTAcomplete$fit-IRTAcomplete$fit2 
VarCorr(final.model)
VarCorr(final.model_nlme)
```

![](img/2c8bb305c18eb74cd8bb174de9339d95.png)

```
xyplot(BW~Time|Pig, data = IRTAcomplete, 
       strip = FALSE, aspect = "xy", pch = 16, cex=0.2, grid = TRUE,
       panel = function(x, y, ..., fit, subscripts) {
         panel.grid()
         panel.xyplot(x, y)
         panel.lmline(x, y, lty = 2, col="black")
         ypred<-IRTAcomplete$fit[subscripts]
         panel.lines(x, ypred, col = "red")
         ypred2<-IRTAcomplete$fit2[subscripts]
         panel.lines(x, ypred2, col = "orange")
         panel.abline(0, 0)
       }
```

![](img/c359e471b978743551f81385bed1d408.png)

```
IRTAcomplete$diff<-IRTAcomplete$fit-IRTAcomplete$BW
theme_set(theme_bw())
myx<-scale_x_continuous(breaks= seq(0, 35, by = 5))
myy<-scale_y_continuous(breaks= seq(0, 35, by = 5))
ggplot(IRTAcomplete,aes(x=BW, y=fit, colour=diff))+
  geom_abline(intercept=0, slope=1, lty=2, col="black", lwd=1)+
  geom_point()+
  myx+
  myy+
  scale_colour_gradient2(low="red",mid="blue",high="red")+
  facet_wrap(~Trt)+
  labs(title = "Residuals by treatment", y="Predicted", x="Observed")
ggsave(filename="Observed vs. Predicted BW.png", width=16, height=8, dpi=600)
```

![](img/60ee2a8c6475143e39d5694e9dadfafd.png)

```
theme_set(theme_bw())
myx<-scale_x_continuous(breaks=c(0,7,12,28,42))
myy<-scale_y_continuous(breaks = seq(-2, 2, by = 0.5))
ggplot(IRTAcomplete, aes(x=Time, y=diff, colour=Trt, group=Time))+
  geom_hline(yintercept=c(-2,-1,0,1,2), lty=2)+
  geom_boxplot()+
  myx+
  myy+
  facet_wrap(~Trt) 
```

![](img/57b0a8e5622e379cc2f1faff09ca17ff.png)

通过扩展数据帧来创建新的预测数据帧，从而为第 0 天和第 42 天之间的时间点创建预测，并扩展到第 100 天。

```
pdata<-subset(IRTAcomplete[,c(1:11)], Time==0)
dim(pdata) # 346 rows of 11 variables, meaning 364 pigs included in dataset
pdata<-as.data.frame(sapply(pdata, rep.int, times=43))
pdata$id<-NULL
pdata<-pdata[order(pdata$Pig),]
row.names(pdata)<-NULL
pdata$Time<-rep(c(0:42),364)
pdata<-as.data.frame(pdata)
pdata$Trt<-as.factor(pdata$Trt)
pdata$Sexe<-ifelse(pdata$Sexe==1, "M", "F")
pdata$Sexe<-as.factor(pdata$Sexe)
pdata$Fate<-ifelse(pdata$Fate==2, "Finished", "Slaughter")
pdata$Fate<-as.factor(pdata$Fate)
pdata$Trt<-as.factor(pdata$Trt)### BOOTSTRAP
mySumm <- function(.) {
  predict(., pdata,re.form=NULL)
}
nc <- detectCores()
cl <- makeCluster(rep("localhost", nc))pred<-bootMer(final.model, 
              mySumm, 
              nsim=10, # 7 warnings, not such a stable model and will take a huge amount of tiem to reach 5000 simulations
              use.u=FALSE, 
              type="parametric")
attr(pred,"boot.fail.msgs")### Save bootstraps in dataframe 
bootstrapped<-as.data.frame(1/(pred$t^2))
bootstrapped<-as.data.frame(t(bootstrapped))
bootstrapped<-cbind(bootstrapped,pdata[,c(5,10)])
dim(bootstrapped)
str(bootstrapped)
colnames(bootstrapped)### CREATE RESULTS for AVERAGE BW DIFFERENCES BETWEEN TREATMENTS 
T1.Mean.boot=matrix(ncol=43, nrow=10)
T2.Mean.boot=matrix(ncol=43, nrow=10)
T3.Mean.boot=matrix(ncol=43, nrow=10)
T4.Mean.boot=matrix(ncol=43, nrow=10)
T5.Mean.boot=matrix(ncol=43, nrow=10)
T6.Mean.boot=matrix(ncol=43, nrow=10)for (j in 0:42){
  for (i in 1:10){
    T1.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==1&bootstrapped$Time==j])
    T2.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==2&bootstrapped$Time==j])
    T3.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==3&bootstrapped$Time==j])
    T4.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==4&bootstrapped$Time==j])
    T5.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==5&bootstrapped$Time==j])
    T6.Mean.boot[i,j]<-mean(bootstrapped[,i][bootstrapped$Trt==6&bootstrapped$Time==j])
  }
}
T1.Mean.boot<-as.data.frame(T1.Mean.boot) # means for treatment 1 for each bootstrap sample and each timepoint
T2.Mean.boot<-as.data.frame(T2.Mean.boot) 
T3.Mean.boot<-as.data.frame(T3.Mean.boot)
T4.Mean.boot<-as.data.frame(T4.Mean.boot)
T5.Mean.boot<-as.data.frame(T5.Mean.boot)
T6.Mean.boot<-as.data.frame(T6.Mean.boot)
names(T1.Mean.boot) <- substring(names(T1.Mean.boot), 2)
names(T2.Mean.boot) <- substring(names(T2.Mean.boot), 2)
names(T3.Mean.boot) <- substring(names(T3.Mean.boot), 2)
names(T4.Mean.boot) <- substring(names(T4.Mean.boot), 2)
names(T5.Mean.boot) <- substring(names(T5.Mean.boot), 2)
names(T6.Mean.boot) <- substring(names(T6.Mean.boot), 2)
diffMEAN.T2T1.boot<-as.data.frame(T2.Mean.boot-T1.Mean.boot) 
diffMEAN.T3T1.boot<-as.data.frame(T3.Mean.boot-T1.Mean.boot)
diffMEAN.T4T1.boot<-as.data.frame(T4.Mean.boot-T1.Mean.boot)
diffMEAN.T5T1.boot<-as.data.frame(T5.Mean.boot-T1.Mean.boot)
diffMEAN.T6T1.boot<-as.data.frame(T6.Mean.boot-T1.Mean.boot)### Create dataframe to plot differences per time-point
## T2T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffMEAN.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffMEAN.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffMEAN.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffMEAN.T2T1.dist<-sumBoot(diffMEAN.T2T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffMEAN.T2T1.dist<-setDT(diffMEAN.T2T1.dist, keep.rownames = TRUE)[]
diffMEAN.T2T1.dist<-as.data.frame(diffMEAN.T2T1.dist)
colnames(diffMEAN.T2T1.dist)[1]<-"Day"
colnames(diffMEAN.T2T1.dist)[2]<-"Difference BW T2T1"
colnames(diffMEAN.T2T1.dist)[3]<-"LCI T2T1 BW"
colnames(diffMEAN.T2T1.dist)[4]<-"HCI T2T1 BW"
diffMEAN.T2T1.dist$Day<-as.numeric(diffMEAN.T2T1.dist$Day)
diffMEAN.T2T1.dist$`Difference BW T2T1`<-as.numeric(diffMEAN.T2T1.dist$`Difference BW T2T1`)
diffMEAN.T2T1.dist$`LCI T2T1 BW`<-as.numeric(diffMEAN.T2T1.dist$`LCI T2T1 BW`)
diffMEAN.T2T1.dist$`HCI T2T1 BW`<-as.numeric(diffMEAN.T2T1.dist$`HCI T2T1 BW`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T2T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffMEAN.T2T1.boot[,j],breaks=5,freq=FALSE, xlab="BW difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T2T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T2T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffMEAN.T2T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffMEAN.T2T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffMEAN.T2T1.boot[,j]),max(diffMEAN.T2T1.boot[,j]),length=length(diffMEAN.T2T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffMEAN.T2T1.boot[,j]),sd=sd(diffMEAN.T2T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T3T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffMEAN.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffMEAN.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffMEAN.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffMEAN.T3T1.dist<-sumBoot(diffMEAN.T3T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffMEAN.T3T1.dist<-setDT(diffMEAN.T3T1.dist, keep.rownames = TRUE)[]
diffMEAN.T3T1.dist<-as.data.frame(diffMEAN.T3T1.dist)
colnames(diffMEAN.T3T1.dist)[1]<-"Day"
colnames(diffMEAN.T3T1.dist)[2]<-"Difference BW T3T1"
colnames(diffMEAN.T3T1.dist)[3]<-"LCI T3T1 BW"
colnames(diffMEAN.T3T1.dist)[4]<-"HCI T3T1 BW"
diffMEAN.T3T1.dist$Day<-as.numeric(diffMEAN.T3T1.dist$Day)
diffMEAN.T3T1.dist$`Difference BW T3T1`<-as.numeric(diffMEAN.T3T1.dist$`Difference BW T3T1`)
diffMEAN.T3T1.dist$`LCI T3T1 BW`<-as.numeric(diffMEAN.T3T1.dist$`LCI T3T1 BW`)
diffMEAN.T3T1.dist$`HCI T3T1 BW`<-as.numeric(diffMEAN.T3T1.dist$`HCI T3T1 BW`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T3T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffMEAN.T3T1.boot[,j],breaks=5,freq=FALSE, xlab="BW difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T3T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T3T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffMEAN.T3T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffMEAN.T3T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffMEAN.T3T1.boot[,j]),max(diffMEAN.T3T1.boot[,j]),length=length(diffMEAN.T3T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffMEAN.T3T1.boot[,j]),sd=sd(diffMEAN.T3T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T4T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffMEAN.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffMEAN.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffMEAN.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffMEAN.T4T1.dist<-sumBoot(diffMEAN.T4T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffMEAN.T4T1.dist<-setDT(diffMEAN.T4T1.dist, keep.rownames = TRUE)[]
diffMEAN.T4T1.dist<-as.data.frame(diffMEAN.T4T1.dist)
colnames(diffMEAN.T4T1.dist)[1]<-"Day"
colnames(diffMEAN.T4T1.dist)[2]<-"Difference BW T4T1"
colnames(diffMEAN.T4T1.dist)[3]<-"LCI T4T1 BW"
colnames(diffMEAN.T4T1.dist)[4]<-"HCI T4T1 BW"
diffMEAN.T4T1.dist$Day<-as.numeric(diffMEAN.T4T1.dist$Day)
diffMEAN.T4T1.dist$`Difference BW T4T1`<-as.numeric(diffMEAN.T4T1.dist$`Difference BW T4T1`)
diffMEAN.T4T1.dist$`LCI T4T1 BW`<-as.numeric(diffMEAN.T4T1.dist$`LCI T4T1 BW`)
diffMEAN.T4T1.dist$`HCI T4T1 BW`<-as.numeric(diffMEAN.T4T1.dist$`HCI T4T1 BW`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T4T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffMEAN.T4T1.boot[,j],breaks=5,freq=FALSE, xlab="BW difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T4T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T4T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffMEAN.T4T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffMEAN.T4T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffMEAN.T4T1.boot[,j]),max(diffMEAN.T2T1.boot[,j]),length=length(diffMEAN.T4T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffMEAN.T4T1.boot[,j]),sd=sd(diffMEAN.T4T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T5T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffMEAN.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffMEAN.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffMEAN.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffMEAN.T5T1.dist<-sumBoot(diffMEAN.T5T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffMEAN.T5T1.dist<-setDT(diffMEAN.T5T1.dist, keep.rownames = TRUE)[]
diffMEAN.T5T1.dist<-as.data.frame(diffMEAN.T5T1.dist)
colnames(diffMEAN.T5T1.dist)[1]<-"Day"
colnames(diffMEAN.T5T1.dist)[2]<-"Difference BW T5T1"
colnames(diffMEAN.T5T1.dist)[3]<-"LCI T5T1 BW"
colnames(diffMEAN.T5T1.dist)[4]<-"HCI T5T1 BW"
diffMEAN.T5T1.dist$Day<-as.numeric(diffMEAN.T5T1.dist$Day)
diffMEAN.T5T1.dist$`Difference BW T5T1`<-as.numeric(diffMEAN.T5T1.dist$`Difference BW T5T1`)
diffMEAN.T5T1.dist$`LCI T5T1 BW`<-as.numeric(diffMEAN.T5T1.dist$`LCI T5T1 BW`)
diffMEAN.T5T1.dist$`HCI T5T1 BW`<-as.numeric(diffMEAN.T5T1.dist$`HCI T5T1 BW`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T5T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffMEAN.T5T1.boot[,j],breaks=5,freq=FALSE, xlab="BW difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T5T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T5T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffMEAN.T5T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffMEAN.T5T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffMEAN.T5T1.boot[,j]),max(diffMEAN.T5T1.boot[,j]),length=length(diffMEAN.T5T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffMEAN.T5T1.boot[,j]),sd=sd(diffMEAN.T5T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T6T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffMEAN.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffMEAN.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffMEAN.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffMEAN.T6T1.dist<-sumBoot(diffMEAN.T6T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffMEAN.T6T1.dist<-setDT(diffMEAN.T6T1.dist, keep.rownames = TRUE)[]
diffMEAN.T6T1.dist<-as.data.frame(diffMEAN.T6T1.dist)
colnames(diffMEAN.T6T1.dist)[1]<-"Day"
colnames(diffMEAN.T6T1.dist)[2]<-"Difference BW T6T1"
colnames(diffMEAN.T6T1.dist)[3]<-"LCI T6T1 BW"
colnames(diffMEAN.T6T1.dist)[4]<-"HCI T6T1 BW"
diffMEAN.T6T1.dist$Day<-as.numeric(diffMEAN.T6T1.dist$Day)
diffMEAN.T6T1.dist$`Difference BW T6T1`<-as.numeric(diffMEAN.T6T1.dist$`Difference BW T6T1`)
diffMEAN.T6T1.dist$`LCI T6T1 BW`<-as.numeric(diffMEAN.T6T1.dist$`LCI T6T1 BW`)
diffMEAN.T6T1.dist$`HCI T6T1 BW`<-as.numeric(diffMEAN.T6T1.dist$`HCI T6T1 BW`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T6T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffMEAN.T6T1.boot[,j],breaks=5,freq=FALSE, xlab="BW difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T6T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffMEAN.T6T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffMEAN.T6T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffMEAN.T6T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffMEAN.T6T1.boot[,j]),max(diffMEAN.T6T1.boot[,j]),length=length(diffMEAN.T6T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffMEAN.T6T1.boot[,j]),sd=sd(diffMEAN.T6T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}# Plot differences in BW between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gBW1<-ggplot(diffMEAN.T2T1.dist, aes(x=Day, y=`Difference BW T2T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T2T1 BW`, ymax=`HCI T2T1 BW`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in BW between Treatment 3 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gBW2<-ggplot(diffMEAN.T3T1.dist, aes(x=Day, y=`Difference BW T3T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T3T1 BW`, ymax=`HCI T3T1 BW`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in BW between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gBW3<-ggplot(diffMEAN.T4T1.dist, aes(x=Day, y=`Difference BW T4T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T4T1 BW`, ymax=`HCI T4T1 BW`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in BW between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gBW4<-ggplot(diffMEAN.T5T1.dist, aes(x=Day, y=`Difference BW T5T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T5T1 BW`, ymax=`HCI T5T1 BW`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in BW between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gBW5<-ggplot(diffMEAN.T6T1.dist, aes(x=Day, y=`Difference BW T6T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T6T1 BW`, ymax=`HCI T6T1 BW`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)gridBW<-grid.arrange(gBW1,gBW2,gBW3,gBW4,gBW5)
ggsave("TrtvsControl_BW.png", gridBW, width=16, height=8, dpi=600)
dev.off()
```

![](img/98e2bba35a01d4072e17c1218656383b.png)

```
### CREATE RESULTS for SD DIFFERENCES BETWEEN TREATMENTS 
T1.SD.boot=matrix(ncol=43, nrow=10)
T2.SD.boot=matrix(ncol=43, nrow=10)
T3.SD.boot=matrix(ncol=43, nrow=10)
T4.SD.boot=matrix(ncol=43, nrow=10)
T5.SD.boot=matrix(ncol=43, nrow=10)
T6.SD.boot=matrix(ncol=43, nrow=10)for (j in 0:42){
  for (i in 1:10){
    T1.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==1&bootstrapped$Time==j])
    T2.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==2&bootstrapped$Time==j])
    T3.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==3&bootstrapped$Time==j])
    T4.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==4&bootstrapped$Time==j])
    T5.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==5&bootstrapped$Time==j])
    T6.SD.boot[i,j]<-sd(bootstrapped[,i][bootstrapped$Trt==6&bootstrapped$Time==j])
  }
}
T1.SD.boot<-as.data.frame(T1.SD.boot) 
T2.SD.boot<-as.data.frame(T2.SD.boot) 
T3.SD.boot<-as.data.frame(T3.SD.boot)
T4.SD.boot<-as.data.frame(T4.SD.boot)
T5.SD.boot<-as.data.frame(T5.SD.boot)
T6.SD.boot<-as.data.frame(T6.SD.boot)
names(T1.SD.boot) <- substring(names(T1.SD.boot), 2)
names(T2.SD.boot) <- substring(names(T2.SD.boot), 2)
names(T3.SD.boot) <- substring(names(T3.SD.boot), 2)
names(T4.SD.boot) <- substring(names(T4.SD.boot), 2)
names(T5.SD.boot) <- substring(names(T5.SD.boot), 2)
names(T6.SD.boot) <- substring(names(T6.SD.boot), 2)
diffSD.T2T1.boot<-as.data.frame(T2.SD.boot-T1.SD.boot) 
diffSD.T3T1.boot<-as.data.frame(T3.SD.boot-T1.SD.boot)
diffSD.T4T1.boot<-as.data.frame(T4.SD.boot-T1.SD.boot)
diffSD.T5T1.boot<-as.data.frame(T5.SD.boot-T1.SD.boot)
diffSD.T6T1.boot<-as.data.frame(T6.SD.boot-T1.SD.boot)### Create dataframe to plot differences per time-point
## T2T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffSD.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffSD.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffSD.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffSD.T2T1.dist<-sumBoot(diffSD.T2T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffSD.T2T1.dist<-setDT(diffSD.T2T1.dist, keep.rownames = TRUE)[]
diffSD.T2T1.dist<-as.data.frame(diffSD.T2T1.dist)
colnames(diffSD.T2T1.dist)[1]<-"Day"
colnames(diffSD.T2T1.dist)[2]<-"Difference SD T2T1"
colnames(diffSD.T2T1.dist)[3]<-"LCI T2T1 SD"
colnames(diffSD.T2T1.dist)[4]<-"HCI T2T1 SD"
diffSD.T2T1.dist$Day<-as.numeric(diffSD.T2T1.dist$Day)
diffSD.T2T1.dist$`Difference SD T2T1`<-as.numeric(diffSD.T2T1.dist$`Difference SD T2T1`)
diffSD.T2T1.dist$`LCI T2T1 SD`<-as.numeric(diffSD.T2T1.dist$`LCI T2T1 SD`)
diffSD.T2T1.dist$`HCI T2T1 SD`<-as.numeric(diffSD.T2T1.dist$`HCI T2T1 SD`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffSD.T2T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffSD.T2T1.boot[,j],breaks=5,freq=FALSE, xlab="SD difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T2T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T2T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffSD.T2T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffSD.T2T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffSD.T2T1.boot[,j]),max(diffSD.T2T1.boot[,j]),length=length(diffSD.T2T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffSD.T2T1.boot[,j]),sd=sd(diffSD.T2T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T3T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffSD.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffSD.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffSD.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffSD.T3T1.dist<-sumBoot(diffSD.T3T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffSD.T3T1.dist<-setDT(diffSD.T3T1.dist, keep.rownames = TRUE)[]
diffSD.T3T1.dist<-as.data.frame(diffSD.T3T1.dist)
colnames(diffSD.T3T1.dist)[1]<-"Day"
colnames(diffSD.T3T1.dist)[2]<-"Difference SD T3T1"
colnames(diffSD.T3T1.dist)[3]<-"LCI T3T1 SD"
colnames(diffSD.T3T1.dist)[4]<-"HCI T3T1 SD"
diffSD.T3T1.dist$Day<-as.numeric(diffSD.T3T1.dist$Day)
diffSD.T3T1.dist$`Difference SD T3T1`<-as.numeric(diffSD.T3T1.dist$`Difference SD T3T1`)
diffSD.T3T1.dist$`LCI T3T1 SD`<-as.numeric(diffSD.T3T1.dist$`LCI T3T1 SD`)
diffSD.T3T1.dist$`HCI T3T1 SD`<-as.numeric(diffSD.T3T1.dist$`HCI T3T1 SD`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffSD.T3T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffSD.T3T1.boot[,j],breaks=5,freq=FALSE, xlab="SD difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T3T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T3T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffSD.T3T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffSD.T3T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffSD.T3T1.boot[,j]),max(diffSD.T3T1.boot[,j]),length=length(diffSD.T3T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffSD.T3T1.boot[,j]),sd=sd(diffSD.T3T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T4T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffSD.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffSD.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffSD.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffSD.T4T1.dist<-sumBoot(diffSD.T4T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffSD.T4T1.dist<-setDT(diffSD.T4T1.dist, keep.rownames = TRUE)[]
diffSD.T4T1.dist<-as.data.frame(diffSD.T4T1.dist)
colnames(diffSD.T4T1.dist)[1]<-"Day"
colnames(diffSD.T4T1.dist)[2]<-"Difference SD T4T1"
colnames(diffSD.T4T1.dist)[3]<-"LCI T4T1 SD"
colnames(diffSD.T4T1.dist)[4]<-"HCI T4T1 SD"
diffSD.T4T1.dist$Day<-as.numeric(diffSD.T4T1.dist$Day)
diffSD.T4T1.dist$`Difference SD T4T1`<-as.numeric(diffSD.T4T1.dist$`Difference SD T4T1`)
diffSD.T4T1.dist$`LCI T4T1 SD`<-as.numeric(diffSD.T4T1.dist$`LCI T4T1 SD`)
diffSD.T4T1.dist$`HCI T4T1 SD`<-as.numeric(diffSD.T4T1.dist$`HCI T4T1 SD`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffSD.T4T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffSD.T4T1.boot[,j],breaks=5,freq=FALSE, xlab="SD difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T4T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T4T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffSD.T4T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffSD.T4T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffSD.T4T1.boot[,j]),max(diffSD.T2T1.boot[,j]),length=length(diffSD.T4T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffSD.T4T1.boot[,j]),sd=sd(diffSD.T4T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T5T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffSD.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffSD.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffSD.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffSD.T5T1.dist<-sumBoot(diffSD.T5T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffSD.T5T1.dist<-setDT(diffSD.T5T1.dist, keep.rownames = TRUE)[]
diffSD.T5T1.dist<-as.data.frame(diffSD.T5T1.dist)
colnames(diffSD.T5T1.dist)[1]<-"Day"
colnames(diffSD.T5T1.dist)[2]<-"Difference SD T5T1"
colnames(diffSD.T5T1.dist)[3]<-"LCI T5T1 SD"
colnames(diffSD.T5T1.dist)[4]<-"HCI T5T1 SD"
diffSD.T5T1.dist$Day<-as.numeric(diffSD.T5T1.dist$Day)
diffSD.T5T1.dist$`Difference SD T5T1`<-as.numeric(diffSD.T5T1.dist$`Difference SD T5T1`)
diffSD.T5T1.dist$`LCI T5T1 SD`<-as.numeric(diffSD.T5T1.dist$`LCI T5T1 SD`)
diffSD.T5T1.dist$`HCI T5T1 SD`<-as.numeric(diffSD.T5T1.dist$`HCI T5T1 SD`)## Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffSD.T5T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffSD.T5T1.boot[,j],breaks=5,freq=FALSE, xlab="SD difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T5T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T5T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffSD.T5T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffSD.T5T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffSD.T5T1.boot[,j]),max(diffSD.T5T1.boot[,j]),length=length(diffSD.T5T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffSD.T5T1.boot[,j]),sd=sd(diffSD.T5T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T6T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffSD.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffSD.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffSD.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffSD.T6T1.dist<-sumBoot(diffSD.T6T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffSD.T6T1.dist<-setDT(diffSD.T6T1.dist, keep.rownames = TRUE)[]
diffSD.T6T1.dist<-as.data.frame(diffSD.T6T1.dist)
colnames(diffSD.T6T1.dist)[1]<-"Day"
colnames(diffSD.T6T1.dist)[2]<-"Difference SD T6T1"
colnames(diffSD.T6T1.dist)[3]<-"LCI T6T1 SD"
colnames(diffSD.T6T1.dist)[4]<-"HCI T6T1 SD"
diffSD.T6T1.dist$Day<-as.numeric(diffSD.T6T1.dist$Day)
diffSD.T6T1.dist$`Difference SD T6T1`<-as.numeric(diffSD.T6T1.dist$`Difference SD T6T1`)
diffSD.T6T1.dist$`LCI T6T1 SD`<-as.numeric(diffSD.T6T1.dist$`LCI T6T1 SD`)
diffSD.T6T1.dist$`HCI T6T1 SD`<-as.numeric(diffSD.T6T1.dist$`HCI T6T1 SD`)## Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffSD.T6T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffSD.T6T1.boot[,j],breaks=5,freq=FALSE, xlab="SD difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T6T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffSD.T6T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffSD.T6T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffSD.T6T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffSD.T6T1.boot[,j]),max(diffSD.T6T1.boot[,j]),length=length(diffSD.T6T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffSD.T6T1.boot[,j]),sd=sd(diffSD.T6T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## Plot differences in SD between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-2,2,0.1))
gSD1<-ggplot(diffSD.T2T1.dist, aes(x=Day, y=`Difference SD T2T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T2T1 SD`, ymax=`HCI T2T1 SD`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)## Plot differences in SD between Treatment 3 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-2,2,0.1))
gSD2<-ggplot(diffSD.T3T1.dist, aes(x=Day, y=`Difference SD T3T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T3T1 SD`, ymax=`HCI T3T1 SD`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)## Plot differences in SD between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-2,2,0.1))
gSD3<-ggplot(diffSD.T4T1.dist, aes(x=Day, y=`Difference SD T4T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T4T1 SD`, ymax=`HCI T4T1 SD`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)## Plot differences in SD between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-2,2,0.1))
gSD4<-ggplot(diffSD.T5T1.dist, aes(x=Day, y=`Difference SD T5T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T5T1 SD`, ymax=`HCI T5T1 SD`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)## Plot differences in SD between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-2,2,0.1))
gSD5<-ggplot(diffSD.T6T1.dist, aes(x=Day, y=`Difference SD T6T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T6T1 SD`, ymax=`HCI T6T1 SD`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)gridSD<-grid.arrange(gSD1,gSD2,gSD3,gSD4,gSD5)
ggsave("TrtvsControl_SD.png", gridSD, width=16, height=8, dpi=600)
dev.off()
```

![](img/b8888004baccbf9bfac4a3f5164b07a8.png)

```
T1.CV.boot<-100*(T1.SD.boot/T1.Mean.boot)
T2.CV.boot<-100*(T2.SD.boot/T2.Mean.boot)
T3.CV.boot<-100*(T3.SD.boot/T3.Mean.boot)
T4.CV.boot<-100*(T4.SD.boot/T4.Mean.boot)
T5.CV.boot<-100*(T5.SD.boot/T5.Mean.boot)
T6.CV.boot<-100*(T6.SD.boot/T6.Mean.boot)
T1.CV.boot<-as.data.frame(T1.CV.boot) 
T2.CV.boot<-as.data.frame(T2.CV.boot) 
T3.CV.boot<-as.data.frame(T3.CV.boot)
T4.CV.boot<-as.data.frame(T4.CV.boot)
T5.CV.boot<-as.data.frame(T5.CV.boot)
T6.CV.boot<-as.data.frame(T6.CV.boot)
names(T1.CV.boot) <- substring(names(T1.CV.boot), 2)
names(T2.CV.boot) <- substring(names(T2.CV.boot), 2)
names(T3.CV.boot) <- substring(names(T3.CV.boot), 2)
names(T4.CV.boot) <- substring(names(T4.CV.boot), 2)
names(T5.CV.boot) <- substring(names(T5.CV.boot), 2)
names(T6.CV.boot) <- substring(names(T6.CV.boot), 2)
diffCV.T2T1.boot<-as.data.frame(T2.CV.boot-T1.CV.boot) 
diffCV.T3T1.boot<-as.data.frame(T3.CV.boot-T1.CV.boot)
diffCV.T4T1.boot<-as.data.frame(T4.CV.boot-T1.CV.boot)
diffCV.T5T1.boot<-as.data.frame(T5.CV.boot-T1.CV.boot)
diffCV.T6T1.boot<-as.data.frame(T6.CV.boot-T1.CV.boot)### Create dataframe to plot differences per time-point
## T2T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffCV.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffCV.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffCV.T2T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffCV.T2T1.dist<-sumBoot(diffCV.T2T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffCV.T2T1.dist<-setDT(diffCV.T2T1.dist, keep.rownames = TRUE)[]
diffCV.T2T1.dist<-as.data.frame(diffCV.T2T1.dist)
colnames(diffCV.T2T1.dist)[1]<-"Day"
colnames(diffCV.T2T1.dist)[2]<-"Difference CV T2T1"
colnames(diffCV.T2T1.dist)[3]<-"LCI T2T1 CV"
colnames(diffCV.T2T1.dist)[4]<-"HCI T2T1 CV"
diffCV.T2T1.dist$Day<-as.numeric(diffCV.T2T1.dist$Day)
diffCV.T2T1.dist$`Difference CV T2T1`<-as.numeric(diffCV.T2T1.dist$`Difference CV T2T1`)
diffCV.T2T1.dist$`LCI T2T1 CV`<-as.numeric(diffCV.T2T1.dist$`LCI T2T1 CV`)
diffCV.T2T1.dist$`HCI T2T1 CV`<-as.numeric(diffCV.T2T1.dist$`HCI T2T1 CV`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diffCV.T2T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffCV.T2T1.boot[,j],breaks=5,freq=FALSE, xlab="CV difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T2T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T2T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffCV.T2T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffCV.T2T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffCV.T2T1.boot[,j]),max(diffCV.T2T1.boot[,j]),length=length(diffCV.T2T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffCV.T2T1.boot[,j]),sd=sd(diffCV.T2T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T3T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffCV.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffCV.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffCV.T3T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffCV.T3T1.dist<-sumBoot(diffCV.T3T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffCV.T3T1.dist<-setDT(diffCV.T3T1.dist, keep.rownames = TRUE)[]
diffCV.T3T1.dist<-as.data.frame(diffCV.T3T1.dist)
colnames(diffCV.T3T1.dist)[1]<-"Day"
colnames(diffCV.T3T1.dist)[2]<-"Difference CV T3T1"
colnames(diffCV.T3T1.dist)[3]<-"LCI T3T1 CV"
colnames(diffCV.T3T1.dist)[4]<-"HCI T3T1 CV"
diffCV.T3T1.dist$Day<-as.numeric(diffCV.T3T1.dist$Day)
diffCV.T3T1.dist$`Difference CV T3T1`<-as.numeric(diffCV.T3T1.dist$`Difference CV T3T1`)
diffCV.T3T1.dist$`LCI T3T1 CV`<-as.numeric(diffCV.T3T1.dist$`LCI T3T1 CV`)
diffCV.T3T1.dist$`HCI T3T1 CV`<-as.numeric(diffCV.T3T1.dist$`HCI T3T1 CV`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T3T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffCV.T3T1.boot[,j],breaks=5,freq=FALSE, xlab="CV difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T3T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T3T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffCV.T3T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffCV.T3T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffCV.T3T1.boot[,j]),max(diffCV.T3T1.boot[,j]),length=length(diffCV.T3T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffCV.T3T1.boot[,j]),sd=sd(diffCV.T3T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T4T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffCV.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffCV.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffCV.T4T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffCV.T4T1.dist<-sumBoot(diffCV.T4T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffCV.T4T1.dist<-setDT(diffCV.T4T1.dist, keep.rownames = TRUE)[]
diffCV.T4T1.dist<-as.data.frame(diffCV.T4T1.dist)
colnames(diffCV.T4T1.dist)[1]<-"Day"
colnames(diffCV.T4T1.dist)[2]<-"Difference CV T4T1"
colnames(diffCV.T4T1.dist)[3]<-"LCI T4T1 CV"
colnames(diffCV.T4T1.dist)[4]<-"HCI T4T1 CV"
diffCV.T4T1.dist$Day<-as.numeric(diffCV.T4T1.dist$Day)
diffCV.T4T1.dist$`Difference CV T4T1`<-as.numeric(diffCV.T4T1.dist$`Difference CV T4T1`)
diffCV.T4T1.dist$`LCI T4T1 CV`<-as.numeric(diffCV.T4T1.dist$`LCI T4T1 CV`)
diffCV.T4T1.dist$`HCI T4T1 CV`<-as.numeric(diffCV.T4T1.dist$`HCI T4T1 CV`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T4T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffCV.T4T1.boot[,j],breaks=5,freq=FALSE, xlab="CV difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T4T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T4T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffCV.T4T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffCV.T4T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffCV.T4T1.boot[,j]),max(diffCV.T2T1.boot[,j]),length=length(diffCV.T4T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffCV.T4T1.boot[,j]),sd=sd(diffCV.T4T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T5T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffCV.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffCV.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffCV.T5T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffCV.T5T1.dist<-sumBoot(diffCV.T5T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffCV.T5T1.dist<-setDT(diffCV.T5T1.dist, keep.rownames = TRUE)[]
diffCV.T5T1.dist<-as.data.frame(diffCV.T5T1.dist)
colnames(diffCV.T5T1.dist)[1]<-"Day"
colnames(diffCV.T5T1.dist)[2]<-"Difference CV T5T1"
colnames(diffCV.T5T1.dist)[3]<-"LCI T5T1 CV"
colnames(diffCV.T5T1.dist)[4]<-"HCI T5T1 CV"
diffCV.T5T1.dist$Day<-as.numeric(diffCV.T5T1.dist$Day)
diffCV.T5T1.dist$`Difference CV T5T1`<-as.numeric(diffCV.T5T1.dist$`Difference CV T5T1`)
diffCV.T5T1.dist$`LCI T5T1 CV`<-as.numeric(diffCV.T5T1.dist$`LCI T5T1 CV`)
diffCV.T5T1.dist$`HCI T5T1 CV`<-as.numeric(diffCV.T5T1.dist$`HCI T5T1 CV`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T5T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffCV.T5T1.boot[,j],breaks=5,freq=FALSE, xlab="CV difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T5T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T5T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffCV.T5T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffCV.T5T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffCV.T5T1.boot[,j]),max(diffCV.T5T1.boot[,j]),length=length(diffCV.T5T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffCV.T5T1.boot[,j]),sd=sd(diffCV.T5T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}## T6T1
sumBoot <- function(merBoot) {
  return(
    data.frame(fit = apply(diffCV.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.5, na.rm=TRUE))),
               lwr = apply(diffCV.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.025, na.rm=TRUE))),
               upr = apply(diffCV.T6T1.boot, 2, function(x) as.numeric(quantile(x, probs=.975, na.rm=TRUE)))))}
diffCV.T6T1.dist<-sumBoot(diffCV.T6T1.boot) # diff.dist$fit[1] equals median(diff.boot$1)
library(data.table)
diffCV.T6T1.dist<-setDT(diffCV.T6T1.dist, keep.rownames = TRUE)[]
diffCV.T6T1.dist<-as.data.frame(diffCV.T6T1.dist)
colnames(diffCV.T6T1.dist)[1]<-"Day"
colnames(diffCV.T6T1.dist)[2]<-"Difference CV T6T1"
colnames(diffCV.T6T1.dist)[3]<-"LCI T6T1 CV"
colnames(diffCV.T6T1.dist)[4]<-"HCI T6T1 CV"
diffCV.T6T1.dist$Day<-as.numeric(diffCV.T6T1.dist$Day)
diffCV.T6T1.dist$`Difference CV T6T1`<-as.numeric(diffCV.T6T1.dist$`Difference CV T6T1`)
diffCV.T6T1.dist$`LCI T6T1 CV`<-as.numeric(diffCV.T6T1.dist$`LCI T6T1 CV`)
diffCV.T6T1.dist$`HCI T6T1 CV`<-as.numeric(diffCV.T6T1.dist$`HCI T6T1 CV`)# Histograms to diagnose bootstrapped samples
par (mfrow=c(2,2))
bootstraps<-as.numeric(sample(colnames(diff.T6T1.boot),4))
for (j in bootstraps){
  #for (i in 1:length(bootstraps)){
  h=hist(diffCV.T6T1.boot[,j],breaks=5,freq=FALSE, xlab="CV difference", main="", ylab="Probability") 
  abline(v=0, col="orange", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T6T1.boot[,j], probs=.5, na.rm=TRUE)),col="red", lwd=2)
  abline(v=as.numeric(quantile(diffCV.T6T1.boot[,j], probs=.025, na.rm=TRUE)),col="red", lwd=2) # Lower bootstrapped percentile interval
  abline(v=as.numeric(quantile(diffCV.T6T1.boot[,j], probs=.975, na.rm=TRUE)),col="red", lwd=2) # Upper bootstrapped percentile intervals
  lines(density(diffCV.T6T1.boot[,j]), col="blue", lwd=2)
  xfit<-as.numeric(seq(min(diffCV.T6T1.boot[,j]),max(diffCV.T6T1.boot[,j]),length=length(diffCV.T6T1.boot[,j])))  
  yfit<-as.numeric(dnorm(xfit,mean=mean(diffCV.T6T1.boot[,j]),sd=sd(diffCV.T6T1.boot[,j]))) # correct if i use probability
  lines(xfit, yfit, col="green", lwd=2)}# Plot differences in CV between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gCV1<-ggplot(diffCV.T2T1.dist, aes(x=Day, y=`Difference CV T2T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T2T1 CV`, ymax=`HCI T2T1 CV`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in CV between Treatment 3 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gCV2<-ggplot(diffCV.T3T1.dist, aes(x=Day, y=`Difference CV T3T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T3T1 CV`, ymax=`HCI T3T1 CV`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in CV between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gCV3<-ggplot(diffCV.T4T1.dist, aes(x=Day, y=`Difference CV T4T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T4T1 CV`, ymax=`HCI T4T1 CV`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in CV between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gCV4<-ggplot(diffCV.T5T1.dist, aes(x=Day, y=`Difference CV T5T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T5T1 CV`, ymax=`HCI T5T1 CV`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)# Plot differences in CV between Treatment 2 & Treatment 1 for each day 
theme_set(theme_bw())
myx<-scale_x_continuous(breaks = seq(0, 42, by = 1))
myy<-scale_y_continuous(breaks = seq(-10,10,0.5))
gCV5<-ggplot(diffCV.T6T1.dist, aes(x=Day, y=`Difference CV T6T1`))+
  myx+
  myy+
  geom_line(lwd=1.5)+
  geom_ribbon(aes(ymin=`LCI T6T1 CV`, ymax=`HCI T6T1 CV`), alpha=0.3, fill="orange")+
  geom_hline(yintercept=0,colour="red", lwd=1, lty=2)gridCV<-grid.arrange(gCV1,gCV2,gCV3,gCV4,gCV5)
ggsave("TrtvsControl_CV.png", gridCV, width=16, height=8, dpi=600)
```

![](img/ff5fe784f9b2373e737b48712fed5668.png)

希望你喜欢它！如有疑问，请联系我！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)