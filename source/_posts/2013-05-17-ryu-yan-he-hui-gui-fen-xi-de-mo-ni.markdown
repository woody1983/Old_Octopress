---
layout: post
title: "R语言和回归分析的模拟"
date: 2013-05-17 15:35
comments: true
categories: R
---

###导入数据

employees <- read.csv("http://www.headfirstlabs.com/books/hfda/hfda_ch09_employees.csv",header=TRUE)

* X id
* received 加薪幅度
* negotiated 是否提出加薪需求
* gender 性别
* year 加薪时间？
<!-- more -->
###Create Hist 
```
hist(employees$received, breaks=50)
```
Y轴 Frequency 是频数 此处为人数的意思
X轴 employees$received 加薪幅度
多数人的加薪幅度为5%左右

确认加薪幅度的标准差为多少
```
sd(employees$received)
#-- [1] 2.432138
```

###汇总统计
```
summary(employees$received)
#  Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#-1.800   4.600   5.500   6.028   6.700  25.900 
```

###单独看一下2007年的加薪情况
```
hist(employees$received[employees$year == 2007], breaks=50)
hist(employees$received[employees$year == 2008], breaks=50)
```
###女性的加薪情况
```
hist(employees$received[employees$gender == "F"], breaks=50)
```
###男性的加薪情况
```
hist(employees$received[employees$gender == "M"], breaks=50)
```

###多角度去看
```
hist(employees$received[employees$year == 2007 & 
     employees$gender == "M"], breaks=50)
hist(employees$received[employees$year == 2007 & 
     employees$gender == "F" & employees$negotiated == "TRUE"], breaks=50)
hist(employees$received[employees$year == 2007 & 
     employees$gender == "F" & employees$negotiated == "FALSE"], breaks=50)
```

####对是否提出过的加薪要求和没有提出要求的分别统计
```
summary(employees$received[employees$negotiated == TRUE])
#   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#-1.800   6.300   7.600   8.096   9.000  25.900 
summary(employees$received[employees$negotiated == FALSE])
#   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#1.700   4.300   5.000   4.994   5.700   8.100 
```
>很明显  提出加薪要求的人 得到的加薪幅度最大



##预测部分
比较 两个变量：要求加薪幅度和实际加薪幅度
```
employees <- read.csv("http://www.headfirstlabs.com/books/hfda/hfda_ch10_employees.csv",header=TRUE)

head(employees)
plot(employees$requested[employees$negotiated == TRUE],
     employees$received[employees$negotiated == TRUE])
```

### 相关系数  
`[1] 0.6656481`

```
cor(employees$requested[employees$negotiated == TRUE],
     employees$received[employees$negotiated == TRUE])
```


####预测公式

> Y轴实际加薪值received（y）= Y轴截距(a) + 斜率(b) x X轴要求加薪值requested(x)

###创建一个线性模型 指出回归线的系数
```
myLm <- lm(received[negotiated == TRUE] ~ requested[negotiated == TRUE],data=employees)
```

###回归线系数
```
myLm$coefficients
#                  (Intercept) requested[negotiated == TRUE] 
#                   2.3121277                     0.7250664 
```

截距 `2.3`     斜率`0.7`

如果你要求加薪8% 用这个公式就是 y = 2.3+0.7*8 实际可能加薪7.9%
如果预期是5% ，y = 2.3+0.7*5 # 5.8% 则会超过预期

##有关合理误差

!!! 小心外插法:用回归方程预测数据范围以外的数值成为外插法
内插法对数据范围内的点进行预测 正式回归法的本来目的。
千万要对模型假设保持戒心。
>观察他人的模型时，一定要想一想他们的假设有何道理，以及他们是否忘记了某种假设。不合适的假设会使模型完全失效---这还算是最好的结果；最坏的结果是具有危险的欺骗性。

限定预测范围 避免外插法
 x的范围在 `0%到22%`

###添加一条回归线  有lm运算得出  在回归线上放的人得到的比预期的多  下方同理
`abline(myLm)`

任何点在距离回归线的距离叫做“残差”    预测总是会与机会误差同在  所以指出误差所在是必要的。
指出误差可以让分析方：`1 切实的预期 2 了解更多 3 更好的决策`

####找出均方根误差

summary(myLm)`#Summary函数里的可以显示Residual standard error: 2.298 on 998 degrees of freedom`
也可以使用函数 sigma直接显示
summary(myLm)$sigma `#[1] 2.297743`

结果 加减2.3% 把机会误差的范围也计算进去

###分割的根本目的是管理误差
以要求加薪10%为标准  划分两个结果 因为10%以下的误差最小
 两个独立的模型  两条回归线 
 优秀的回归分析兼具解释功能和预测功能

###创建新模型
##激进谈判者专用
```
myLmBig <- lm(received[negotiated == TRUE & requested > 10] ~ requested[negotiated == TRUE & requested > 10],
              data=employees)
#胆小谈判者专用
myLmSmall <- lm(received[negotiated == TRUE & requested <= 10] ~ requested[negotiated == TRUE & requested <= 10],
              data=employees)
```

###汇总分析两个新模型
```
summary(myLmSmall)$coefficients
#                                                 Estimate Std. Error   t value      Pr(>|t|)
#(Intercept)                                     0.7933468 0.22472009  3.530378  4.378156e-04
#requested[negotiated == TRUE & requested <= 10] 0.9424946 0.03151835 29.903041 6.588020e-134
summary(myLmSmall)$sigma #[1] 1.374526

#--/ 方根误差更小 1.4左右

summary(myLmBig)$coefficients
#                                               Estimate Std. Error  t value     Pr(>|t|)
#(Intercept)                                    7.813403  1.8760371 4.164845 4.997597e-05
#requested[negotiated == TRUE & requested > 10] 0.302609  0.1420151 2.130824 3.457618e-02
summary(myLmBig)$sigma #[1] 4.544424
#--/ 方根误差在4.5
```

Summary
##如果要求小于或等于10%的时候  Y = 0.8 + 0.9X  ±1.4%
##如果要求大于10%的时候 Y = 7.8 + 0.3X ±4.5%
