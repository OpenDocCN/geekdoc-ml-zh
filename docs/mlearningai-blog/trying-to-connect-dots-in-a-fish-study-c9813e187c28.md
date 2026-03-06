# 试图在鱼类研究中把点联系起来

> 原文：<https://medium.com/mlearning-ai/trying-to-connect-dots-in-a-fish-study-c9813e187c28?source=collection_archive---------4----------------------->

## 为什么真实数据从来没有你看到的技术例子那么干净

在这篇文章中，我想向你介绍我是如何分析鱼类领域的一个特定数据集的。这来自一家商业公司的 R&D 工厂，所以我不能与你分享数据，但至少你可以看到我是如何处理的。

每当我写博客的时候，我再次清楚地意识到，我分析的数据集与图书馆文档中使用的数据集有着本质的不同。它们不太清晰、不太直观，而且通常你只能从这些数据集中得到一小部分。好了，这就是你的数据分析。它不干净。这并不简单，分析一个数据集意味着在想法、启示和碰壁之间来回穿梭，并意识到你想看到或相信你能看到的东西并不在那个特定的数据集中。也许它不会出现在随后的任何数据集中，但是由于研究很少被重复，所以你只能使用那个特定的数据集。

所以，我们又来了，向你们展示我如何分析一个商业数据集。很肯定，无论是谁读了这篇文章，都会发现更多可能已经走过的路，可能已经接近的技术，但可惜我不能与你分享这些数据。然后，它可以帮助你获得一些创造性的果汁去振兴你自己的项目。如果是这样的话，这篇博文就成功了。

我们开始吧！

```
remove(list = ls())#### LIBRARIES #####
library(dplyr)
library(skimr)
library(DataExplorer)
library(ggpubr)
library(lsmeans)
library(multcomp)
library(car)
library(emmeans)
library(afex)
library(lme4)
library(lmerTest)
library(caret)
library(mltools)
library(data.table)
library(corrplot)
library(tidyverse)
library(drc)
library(rsm)
library(mice)
library(VIM)#### DATA WRANGLING ####
## the combine file is saved as /pores_2ndRUN
MCT_all <- read.delim("PORE_run2")
MCT_all#convert to factors
mydata_all <- as.data.frame(MCT_all)
str (mydata_all)
attach(mydata_all)## Sumarize the info
group_by(mydata_all, GROUP) %>%
  summarise (
    mean = mean (TV, na.rm = TRUE),
    sd = sd(TV, na.rm=TRUE),
    median = median (TV, na.rm = TRUE),
    IQR = IQR (TV,na.rm = TRUE)
  )glimpse (mydata_all)
glimpse (MCT_all)
skim(MCT_all)
```

![](img/33f7377ecc7f78705b9241ef4a72fd98.png)

A glimpse of the data.

```
mydata_all_B<-mydata_all%>%
  dplyr::select(-c("Number", "Dataset"))%>%
group_by(GROUP) %>% 
  summarise_at(vars(TV:SD.St.Sp.), mean, na.rm = TRUE)
mydata_all$ID <- seq.int(nrow(mydata_all))
mydata_all_B$ID <- seq.int(nrow(mydata_all_B))
```

![](img/41cfe4c53607da2efee7819bafd9bd2d.png)

And the data much cleaner. There should be enough information included.

```
plot_intro(mydata_all_B)
plot_missing(mydata_all_B)
plot_boxplot(mydata_all_B, by="GROUP")
plot_correlation(mydata_all_B)
plot_density(mydata_all_B, geom_density_args = list("fill" = "black", "alpha" = 0.6))
plot_histogram(mydata_all_B)
plot_qq(mydata_all_B)
plot_qq(mydata_all_B,by="GROUP")
plot_scatterplot(mydata_all_B,by="GROUP")
plot_scatterplot(mydata_all_B,by="TV")
```

![](img/ba2311299276c5ee7b0a1bb8f7af0682.png)![](img/bf531e9d830d8330b1c34565177ed3ee.png)![](img/21a929605077dee90431f5c069518893.png)![](img/7ff13fa90b8f1fac1f83d4ecb2256d9a.png)![](img/dbcef8de852865bf1ba1c2e3658d6871.png)![](img/981b7909f21d6ed76dcca6d1eb4c6157.png)![](img/c8df05c82d67191e477c08dc2d21f0d8.png)

The [**DataExplorer**](https://cran.r-project.org/web/packages/DataExplorer/vignettes/dataexplorer-intro.html) library is a gem when it comes to exploring data.

```
bc <- preProcess(as.data.frame(mydata_all_B), method="BoxCox")
BoxCoxData <- predict(bc, as.data.frame(mydata_all_B))
skim(mydata_all_B)
skim(BoxCoxData)
plot_density(BoxCoxData, 
             geom_density_args = list("fill" = "black", "alpha" = 0.6))
```

![](img/c1db1e4ff573ba00d920c26e14db72af.png)

From the original glimpse made of the dataset you could already see that the variables are not normally distributed. They do not have to be! However, applying [Box-Cox transformations](https://www.statology.org/box-cox-transformation-in-r/) does make life easier when trying to converge models. However, I would always prefer to run a GLM model, since the transformations are done inside the model, via the link function, and so the back-transformation is much more straightforward.

让我们运行第一个模型，这是一个线性回归，但可以作为方差分析。它们是同一枚硬币的两面。在这里，我正在寻找作为治疗函数的感兴趣的结果之间的差异。

```
model1 <- lm(TV~GROUP, 
             data = mydata_all)
summary(model1)
par(mfrow = c(2, 2))
plot(model1)
anova(model1)
aov.model1<-aov(TV ~GROUP, data = mydata_all)
aov.model1
coef(aov.model1)
fit_emmeans <- emmeans(model1, "GROUP")
cld(fit_emmeans, Letter="abcdefg")
plot(cld(fit_emmeans, Letter="abcdefg"))
```

![](img/f8accc04596a3a27b62d5c87c04b7b96.png)![](img/4d96f4a93f7e6714f3362fb565394555.png)![](img/374f97b1ff9540415a84a7c84fec2c89.png)![](img/b42c71a9cb9e88e27c72af9849f3fe3a.png)![](img/3db17dc980b5058127b7bef51598596b.png)![](img/c78d3a5204f21635ddda46c92aadfdb6.png)

The most important plot. Differences of which the confidence interval does not include zero are significant differences. The great number of comparisons made warrant the application of a multiple-testing safeguard. Otherwise, the family type-2 error rate will no longer be 0.05 ([and that is saying that we actually like to approach data in such a way, since it is riddles with issues](/mlearning-ai/inference-estimates-p-values-and-confidence-limits-a-frequentist-approach-acdd45d94bd5)).

我们还可以在测试和训练数据集中拆分数据，添加虚拟变量，并进行交叉验证，以赋予模型更多的外部有效性(通过使用内部有效性)。

```
inTraining <- createDataPartition(mydata_all$GROUP, p = .75, list = FALSE)
training <- mydata_all_B[ inTraining,]
testing  <- mydata_all_B[-inTraining,]
skim(training)
## One Hot Encoding
training <- one_hot(as.data.table(training))
testing <- one_hot(as.data.table(testing))
# Control Resampling Methods 
tr <- trainControl(method = "repeatedcv",
                   number = 20,
                   repeats = 10)
tr1 <- trainControl(method = "boot632",
                    number =1000)# Regression Model with all predictors included - high multcollinearity
set.seed(1123)
lm1 <- train(TV~. -ID -Obj.N - Obj.V -Obj.V.1 -Obj.V.TV -Obj.S.TV -Obj.S.Obj.V -Obj.Dm -GROUP -Po.V.tot., 
             data=training,
             preProcess=c("center", "scale"),
             method = "BstLm",
             trControl=tr1, 
             na.action = na.omit)
lm1
summary(lm1)
plot(lm1)
lm1$finalModel 
model1 <- lm(TV ~GROUP + i.S + T.Ar + Po.V.cl.,SD.St.Sp., data = mydata_all)
summary(model1)
fit_emmeans <- emmeans(model1, "GROUP")
cld(fit_emmeans, Letter="abcdefg") # including confounders changes the ANOVA
comp1<-glht(model1, mcp(GROUP="Tukey")) # Comparison of diets
summary(comp1)
plot(comp1)
```

![](img/93b8aabf64eaf7f68bd19e671c540006.png)![](img/add6183529094d14e9407e405114919f.png)

The type of model used here (**BstLm** which stands for Boosted Linear Model)will set the coefficient of non-important variables to zero.

我可以使用上面的选择来运行一个新模型，然后寻找*组*的差异，包括一些协变量。

![](img/b6d4a07c7bd9ccafa1852fce8bc83222.png)![](img/2ebd0b0f4f91e0d22b19f0a95f7a9cda.png)![](img/c363ebdf4a6a2b2edc3c563069ac9ff5.png)

Model information to the left. To the right, we have the differences between treatments. None of the differences are now significant. I guess that including some covariances made it a bit more difficult. This is good, since you do not want to make false inferences. The side of the confidence intervals is saying something as well. They are huge! That gives you something to ponder about the model itself and its usability.

我们可以尝试和做的是将我们的分析集中在特定的饮食上，从而针对特定的人群。对于任何统计分析，您不需要这样做。事实上，分析数据的最好方法是尽可能全面地分析数据。但是，让我们来看看。

```
## Extruded vs pelleting
My_list2 <- split(mydata_all, f = list(mydata_all$GROUP))  
analysis1 <- rbind(My_list2$Diet_F, My_list2$Diet_J)
model1 <- lm(TV ~GROUP, data = analysis1)
summary(model1)
par(mfrow = c(2, 2))
plot(model1)
fit_emmeans <- emmeans(model1, "GROUP")
cld(fit_emmeans, Letter="ab") # not different in univariate analysis 
comp1<-glht(model1, mcp(GROUP="Tukey")) # Comparison of diets
summary(comp1)
plot(comp1)
```

![](img/ef866fcb5c0869a9bda7027f2b2875e8.png)![](img/8e07b6364ee999567b26eb6fdda8653c.png)![](img/ce615502a69674e5e68dd44bfc7cf98d.png)![](img/02d762eeb1353ae853138cda60c84ad3.png)

Model does really have a lot of datapoints, and although the difference is significant, I would not attach to much meaning to it. Who knows what a new dataset could bring to the table?

```
model1 <- lm(Obj.V/TV ~GROUP, data = analysis1)
summary(model1)
par(mfrow = c(2, 2))
plot(model1)
fit_emmeans <- emmeans(model1, "GROUP")
cld(fit_emmeans, Letter="ab") # not different in univariate analysis 
comp1<-glht(model1, mcp(GROUP="Tukey")) # Comparison of diets
summary(comp1)
plot(comp1)
```

![](img/eb98f3f57c4ade78ab7a82042d3f0a10.png)

As a ratio, the difference between groups is still significant, but the differences is starting to approach zero. Whenever you find a significant difference, you always need to ask yourself how clinically relevant that difference is. It may be significant according to frequentist theory, but economically, or clinically, it does not add anything to the table. Thats why power calculations, and pre-experiment simulations are so important.

```
analysis5 <- rbind(My_list2$Diet_F, My_list2$Diet_G,My_list2$Diet_H, My_list2$Diet_D )
anova1 <- aov(TV ~ GROUP, data = analysis5)
summary(anova1)
par(mfrow=c(2,2))
plot (anova1)
model1 <- lm(TV ~GROUP, data = analysis5)
summary(model1)
fit_emmeans <- emmeans(model1, "GROUP")
cld(fit_emmeans, Letter="abcd") 
comp1<-glht(model1, mcp(GROUP="Tukey")) # Comparison of diets
summary(comp1)
dev.off()
plot(comp1)
```

![](img/659df56a5a56155a700f7560adb7a814.png)![](img/c3764510778d1e016ee7c9cd6a4caf94.png)

And the same again, as above, but now more data included. For some reason, the plot function does not show the treatment comparisons as a whole, which is annoying, but the plot to the right is the graphical depiction of the stats to the left.

```
c1 <- TukeyHSD(anova1)
c1a <- as.data.frame(TukeyHSD(anova1)$GROUP)
c1a$pair = rownames (c1a)c1b <- ggplot (c1a, aes(colour=cut (`p adj`, c(0,0.001, 0.01, 1),
                                   label= c("p<0.001", "p<0.01", "non-Sig"))))+
  geom_hline(yintercept=0, lty="11", colour= "grey30") +
  geom_errorbar(aes(pair, ymin=lwr, ymax=upr), width=0.2) +
  geom_point(aes(pair, diff)) +
  labs (x= " ", y= " ")+
  labs(colour="")+
  ggtitle("TV")+
  theme_bw()c1b
```

![](img/5b5db15cc363d839d16e59b65bfeda33.png)

A much more beatiful way of showing the differences between the treatments for the outcome TV (NOT television ;-) ).

```
par(mfrow=c(1,3))
pelExt_control <- rbind (My_list2$Diet_F, My_list2$Diet_J)p1 <- ggplot(pelExt_control, aes(x = GROUP, y = Po.Dm, fill = GROUP))+
  geom_boxplot(outlier.shape = NA, colour = "black") + 
  scale_fill_manual(values = c("#F8766D","#00BFC4"))+
  geom_signif(comparisons = list(c("Diet_F", "Diet_J")), step_increase = 0.1)+
  theme(legend.position = "")+
  labs (x= " ", y= "Pore diammeter (nm3)")+
  scale_x_discrete(labels = c("Extruded", "Pelleted"))+
  ylim(10,50)+
  theme(axis.title.y = element_text(size = 12, face = "bold"))+theme_bw()p2 <- ggplot(pelExt_control, aes(x = GROUP, y = Po.tot., fill = GROUP))+
  geom_boxplot(outlier.shape = NA, colour = "black") + 
  scale_fill_manual(values = c("#F8766D","#00BFC4"))+
  geom_signif(comparisons = list(c("Diet_F", "Diet_J")), step_increase = 0.1)+
  theme(legend.position = "none")+
  labs (x= " ", y= "Total porosity (%)")+
  scale_x_discrete(labels = c("Extruded", "Pelleted"))+
  ylim(3,20)+
  theme(axis.title.y = element_text(size = 12, face = "bold"))+theme_bw()p3 <- ggplot(pelExt_control, aes(x = GROUP, y = Po.op., fill = GROUP))+
  geom_boxplot(outlier.shape = NA, colour = "black") + 
  scale_fill_manual(values = c("#F8766D","#00BFC4"))+
  geom_signif(comparisons = list(c("Diet_F", "Diet_J")), step_increase = 0.5)+
  theme(legend.position = "none")+
  labs (x= " ", y= "Open porosity (%)")+
  scale_x_discrete(labels = c("Extruded", "Pelleted"))+
  ylim(0,20)+
  theme(axis.title.y = element_text(size = 12, face = "bold"))+theme_bw()p4 <- ggplot(pelExt_control, aes(x = GROUP, y = Po.cl., fill = GROUP))+
  geom_boxplot(outlier.shape = NA, colour = "black") + 
  scale_fill_manual(values = c("#F8766D","#00BFC4"))+
  geom_signif(comparisons = list(c("Diet_F", "Diet_J")), step_increase = 0.1)+
  theme(legend.position = "none")+
  labs (x= " ", y= "Close porosity (%)")+
  scale_x_discrete(labels = c("Extruded", "Pelleted"))+
  ylim(0,8)+
  theme(axis.title.y = element_text(size = 12, face = "bold"))+theme_bw()tt1 <- ggarrange(p1,p2,p3,p4, ncol=2, nrow=2)
tt1
```

![](img/58ee63c22eef2632b31fb0771131977b.png)

And another beautiful way of plotting differences. I take no credit for these codes btw. They were included in the dataset that I analized and augmented. It is always great to learn from somebody else, and of course so do I.

顺便说一句，你在下面看到的是使用线性模型进行多元分析的一种巧妙方式。通过使用 *cbind* 选项，您可以让 R 对多个结果进行分组比较。这叫做马诺娃。

```
model1<-lm(cbind(Po.Dm,Po.tot.,Po.op.,Po.cl.)~GROUP,data=pelExt_control)
summary(model1)
vcov(model1)
Anova(model1) #MANOVA TEST STATISTIC
fit_emmeans <- emmeans(model1, "GROUP")
plot(cld(fit_emmeans, Letter="abcd")) +theme_bw()
```

![](img/ea7a64e44bd8ef0805157aa940b60ee7.png)![](img/c3d8fc05a447767c4eb7b9dc92442a3b.png)

```
MCT_all
My_list2zeolite <- rbind (My_list2$Diet_A, My_list2$Diet_B, My_list2$Diet_C, My_list2$Diet_D,My_list2$Diet_E,My_list2$Diet_F )
valid.names <- names(zeolite)[names(zeolite) != "GROUP" & names(zeolite)!="Dataset" & names(zeolite)!="Number"]
zeolite$GROUP<-as.character(zeolite$GROUP)mapping <- c("Diet_A" = 10, 
             "Diet_B" = 8, 
             "Diet_C" = 6, 
             "Diet_D" = 4,
             "Diet_E" = 2,
             "Diet_F" = 0)
zeolite$concentration <- mapping[zeolite$GROUP]ggplot(zeolite,aes(x=Conn,y=TV,colour=GROUP)) + 
  geom_point() +
  geom_smooth(method = lm, formula = y ~ splines::bs(x, 3), se = FALSE) +
  theme_bw()ggplot(zeolite,aes(x=concentration,y=TV,colour=GROUP)) + 
  geom_point() +
  geom_smooth(method = lm, formula = y ~ splines::bs(x, 3), se = FALSE) +
  theme_bw()
```

![](img/e258e9b403488a8b41dd85d9acc7c505.png)![](img/3aa8c58a4c7d9795c9ac1cb7e5a8b0f9.png)

I cannot say the graph to the left is helping. Splines have a tendency to connect points, no matter what, building trajectories that make no sense whatsoever.

下面是一个聪明简单的方法，根据你的数据运行一个剂量反应模型。

```
model<- drm(TV~concentration, data=zeolite,
            fct=LL.4(names = c("Slope", "Lower Limit", "Upper Limit", "ED50")))
plot(model, type="all")
summary(model)
```

![](img/34e9fa9f4e2fdba0a04d8a86dbe4513a.png)![](img/11c18955d23c5bd4dfc1fa3ce16f374c.png)

Looks like this, which looks good to me.

和一个[响应面模型](/mlearning-ai/surface-analysis-of-feed-data-in-fish-5e426464d2f4)，运行单阶连接。

```
model1<-rsm(TV~SO(concentration), data=zeolite)
plot(model)
summary(model1)
```

![](img/6fa72b5b45eb1b096554777482f6e5de.png)

```
model2<-rsm(TV~SO(concentration, TS, Po.Dm), data=zeolite)
summary(model2)
par(mfrow = c(1, 3))
contour(model2, ~concentration+TS+Po.Dm, image=TRUE)model3<-rsm(TV~SO(concentration, TS, Po.Dm, T.Ar), data=zeolite)
summary(model3)
par(mfrow = c(3, 1))
contour(model3, ~concentration+TS+Po.Dm, image=TRUE)
```

![](img/8f6061dc819ec5236f8cf6c8fd8c0c27.png)![](img/5945d478b4211505fcaeb9fa24418d67.png)

Actually looks very interesting. Not sure if the design of the study was intended to fit a response surface, but I really like what I see. A clear maximum to the left. But, also a clear sadle, and a funny looking connection which I wonder if it is not an artefact.

下面是一个不同的数据集，仍然来自 fish，展示了一个输入数据的尝试。现在，数据插补本身就是一项完整的建模工作，插补本身有很大的影响。这不是你能做的事！你做的方式会影响后续的分析。因此，尽管意在减少不确定性，你还是通过测试和整合新的假设来尝试。

```
library(readxl)
trial<- read_excel("50194 - Statistical analysis_FFA.xlsx", sheet = "Data2")
trial<-as.data.frame(trial)
trial <- trial %>% mutate(imp = if_else(is.na(tank) == TRUE, 'yes', 'no'))  
trial$imp<-as.factor(trial$imp)
## DATA EXPLORATION
plot_intro(trial)
plot_missing(trial)
plot_boxplot(trial, by="DIET")
plot_correlation(trial)
plot_density(trial, geom_density_args = list("fill" = "black", "alpha" = 0.6))
plot_histogram(trial)
plot_qq(trial)
plot_scatterplot(trial,by="DIET")
plot_scatterplot(trial,by="tank")
```

![](img/ba9975851aff5e1874523cfbb390477d.png)![](img/ec53bb99e6d3d0524d2fd65273a9486c.png)![](img/f6d9e7af7dcb6936a2687ddcca51128d.png)![](img/3726bce9b205bef0296a272e6cadb512.png)![](img/cda299a859f60e471d8287ddd9ce2614.png)![](img/b9319755f6c6e89cf19912a32a11ec9d.png)![](img/80a3dd149de9769952d85b3c7e34b5bb.png)![](img/4a8040657f6e1601a9529fffadd4a774.png)![](img/0d81c467ce80996e9fad8664c7c926b3.png)![](img/78b91a090dbaccc9d76b301b55c2c95f.png)![](img/2b5c35a3bd937aa3c33d7b053b6a5da2.png)![](img/19b196d552917b3a2eab7f5db2b58c06.png)

Again, explore your data!

```
fit<-lmer(FCR1~oPorosity + (1|TRT), data=trial) # I estimate different variances per Treatment 
summary(fit)
anova(fit)
coef(fit)
plot(fit) # here is where you check assumptions of normality and homogenity of variance 
require("sjPlot");plot_model(fit, type="diag")
```

![](img/d1c81a12e21b59b349edec7654377119.png)![](img/6c78b1e4c9395cb95a3df14162b302cc.png)![](img/b0f6d6e2aa5dc6cd0f65f8ce86950de3.png)![](img/f20239ab29b9d748047482e2e5255f0d.png)![](img/a1199006a366b0c6b2d6808b1af86062.png)![](img/6e35d51da0eb3b8fb91ca0b6a9b03a5a.png)

Mixed Models looks good given the sparse data included.

有两个主要的包装，可以用来填补空白。一个是 [**脱字符**](https://topepo.github.io/caret/) **包**这是一个构建预测模型的标准。下面你可以看到一个我的例子，包括一个 K-最近邻插补算法，然后检查它是否有意义。似乎是这样的，但是，使之有意义并不是插补模型的最大目标。这实际上是危险的，因为你试图去填充那些你永远无法证实它是否真实发生过的东西。

```
fit2<-lm(FCR1~oPorosity, data=trial) 
summary(fit2)
require("jtools");plot_summs(fit, fit2, scale = TRUE, plot.distributions = TRUE)
## Quick way to do imputation
knn_imp <- preProcess(trial, method="knnImpute", k=2)
trial_imp <- predict(knn_imp, trial)
skim(trial)
skim(trial_imp)
plot_boxplot(trial_imp,by="imp") # check imputation
ggplot(trial_imp, aes(x=FCR1, y=FCR2, colour=imp))+
  geom_point()+
  facet_grid(cols = vars(TRT))+
  theme_bw()
```

![](img/cefd6d08a80bef23eb67a132b6ad9155.png)![](img/1ced85e0d710ddbb25058300bda8361f.png)

To the left, you can see the difference of a mixed model and a standard linear model. The mixed model is much less uncertain, probably because including a variance component reduced the residual component. To the right, you see the imputations per group for the outcomes of interest. Looks good! BUT, as a said, looking good does not automatically mean it is good.

现在，就多重插补而言，我所知道的最大的软件包是[鼠标软件包](https://cran.r-project.org/web/packages/mice/mice.pdf)。它已经存在很多年了，但是它很坚固。这里的例子是一个非常小的例子，我可以花至少 10 篇博客文章在鼠标上。

```
md.pattern(trial)
aggr_plot <- aggr(trial, col=c('navyblue','red'), 
                  numbers=TRUE, sortVars=TRUE, labels=names(data), cex.axis=.7, 
                  gap=3, ylab=c("Histogram of missing data","Pattern"))
marginplot(trial[c(40,41)]) # Not informative because they have missing at the same place 
histogram(~ oPorosity | is.na(FCR2), data=trial) # oPorosity for missing and non-missing FCR2
histogram(~ cPorosity | is.na(FCR2), data=trial) 
histogram(~ tPorosity | is.na(FCR2), data=trial)# impute the data - once again, all missing on the same place 
trialMICE <- mice(trial,
                 m=5,
                 maxit=50,
                 meth='pmm',
                 seed=500) #pmm is a fast good approximation for sparse data
summary(trialMICE)
plot(trialMICE)
trialMICE$meth 
xyplot(trialMICE,
       oPorosity ~ FCR1+FBW+SGR+WG,
       pch=18,cex=1,
       na.group = imp)
densityplot(trialMICE, ~FAT1 |.imp)
stripplot(trialMICE, FAT1 + ASH1 + MOISTURE1 ~ .imp, layout = c(3, 1))
completedData <- complete(trialMICE,1)
ggplot(completedData , aes(x=FCR1, y=FCR2, colour=imp))+ # makes the same mistakes as knnIMpute
  geom_point()+
  facet_grid(cols = vars(TRT))+
  theme_bw()
```

![](img/feed2d34ad18c12a8aff836adbf6f56b.png)

What I ask MICE to do is to fill in the missing data by looking, just like KNN, at datapoints nearest to the missing data. Since MICE uses all the viable information available, it will look at the specific column of interest and neighbour columns. That is also why it uses chains, like Bayesian Analysis, to find a solution. It needs to constantly shift between a past, present, and future state.

![](img/b8c1247d1514fe8ec16c6f6b39b33e2a.png)

Missing data pattern.

![](img/214347c13a1abc594b92c2b75341ad5b.png)

Looking at points missing for one of the outcomes of interest. Often, when trying to connect dots between two or more variables, you see a difference in one variable if the other one is missing and vice versa. If that IS the case, that variable becomes important information to imputing the missing data at the other variable.

![](img/5a5ec574fad4f363c09432e0443cc2f6.png)![](img/6ad70f5904234b4b79c49a9ec3d2da7b.png)![](img/a4fd17bfe49e985d5bf6a2661347cd82.png)

The same as above.

![](img/50a13dabcaf6e7efbb4864fbb7bf8352.png)

Observed and imputed data. Only really visible for WG, since they have a slightly lower color.

![](img/6d517550adba4d85b39ed0ff05b4238b.png)![](img/9bffc51ff6ee695da238fceb3d30ea20.png)![](img/4521e03ba2fe7e841b7193be35ed29b0.png)

Convergence of the chains. Like always with chains you want to see NO pattern.

![](img/69a2a4a0020ec1430cf0cbb6c3231a01.png)![](img/5b21ea3d66d30b6460d148159e6ab3be.png)

Distribution of observed and imputed datapoints. When using PMM (Predictive Mean Matching), you will always end up with datapoints that are close in proximity to other datapoints. This is what makes it a swift algorithm to use, with components that seem to make sense, but which is not always the best approach. It could very well be that your missing points come from a completely different procedure, which you actively need to model.

所以，这就是我这篇博文的结尾。就像我说的，没有确定的答案，更多的是尝试处理数据集的不同方法的集合。可以做的，或者应该做的要多得多，但这意味着要写得更多。我相信，作为一名数据分析师，你理解有时放手的痛苦。改天再来。

再说一次，如果有什么不对劲，请让我知道！

[](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb) [## Mlearning.ai 提交建议

### 如何成为 Mlearning.ai 上的作家

medium.com](/mlearning-ai/mlearning-ai-submission-suggestions-b51e2b130bfb)