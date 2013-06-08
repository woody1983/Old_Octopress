---
layout: post
title: "Basic Arduino"
date: 2013-06-08 23:50
comments: true
categories: Arduino
---

###PIN脚电压
digitalWrite(led, HIGH);

使13腳位的電壓設定為相對應的值,HIGH為5V(3.3的板子上為3.3v),LOW為0V


* Digital Read Serial 串行数字阅读
* Analog Read Serial 串行模拟阅读
* resistor 电阻
* hook-up wire 面包板插线
* breadboard 面包板

留意 LED的+电源是有PIN脚给的~ 5V电源是直接供电给button 然后通过10k的电阻连接到GND~ 并在-电源上插了一根线连到一个PIN脚上 并pinMode设置为OUTPUT~获取pushbutton的电源情况~

{% img http://25.media.tumblr.com/fa5442720a0bb7c5977224b78bcde78f/tumblr_mnb2qw0yjf1sst44io1_400.jpg %}

<!-- more -->

###ADC & Analog Read Serial
arduino主控板上A0—-A5是指ADC输入，所谓ADC值模数变换器，将读入的模拟值进行处理，将模拟数转换为数字，便于用户对数据进行处理，因为atmea328ADC是一个10位的寄存器，2的10次方=1024，从1~1023所以当arduino读入值为默认的电压最大值5V时，analogRead读到的值就是1023.

模拟信号
模拟信号是指信息参数在给定范围内表现为连续的信号。 或在一段连续的时间间隔内，其代表信息的特征量可以在任意瞬间呈现为任意数值的信号。比如正弦函数、指数函数等。 从自然界感知的大部分物理量都是模拟性质的，如速度、压力、温度、声音、重量以及位置等都是最常见的物理量。

数字信号
数字信号指幅度的取值是离散的，幅值表示被限制在有限个数值之内。二进制码就是一种数字信号。二进制码受噪声的影响小，易于有数字电路进行处理，所以得到了广泛的应用。 通俗的说：数字传感器就产生0 1信号(此0与1是指高低电平形成的矩形波) 而模拟传感器是通过输出一个线性变换的电平信号(如通常的正弦波)


#杂记

###关于VGA 看来还是需要给Raspberry配个显示器
HDMI: High Definition Multimedia Interface, 高清晰度多媒体接口，其可同时传送音频和影音信号，最高数据传输速度为5Gbps

VGA: Video Graphics Array, 视频图形阵列,

普及一下知识，笔记本电脑上的VGA接口和HDMI接口的是输出作用的，就是说只能数据输出，没有接受数据的功能。

补记:远程也可以 但第一次配置WiFi账户密码什么的 还是需要显示的


###关于电位器
程序方面没有什么特别的地方 一样是 将他的Analog数值读出来用作你想使用的地方

int sensorValue = analogRead(A0);
实用的地方之一 就是可以用外界来控制电压的大小 来达到想要的效果.

将得到的analog数值按照模拟方式写给电子元件

analogWrite(2,sensorValue);
另外 蜂鸣器需要特殊的操作才能发出声音…噪音确切点


###我的第一个Arduino UNO R3
{% img http://24.media.tumblr.com/cd348854371f41a165a21b802d4b8cae/tumblr_mn8rm1rGDS1sst44io1_400.jpg %}
{% img http://25.media.tumblr.com/425af0b5217813e7227785ac1431ad3f/tumblr_mnd3ouoEB61sst44io1_400.jpg %}
{% img http://25.media.tumblr.com/a4e9ab26fd2e9a2f630db6476747192b/tumblr_mnejqm2t7l1sst44io1_400.jpg %}
{% img http://25.media.tumblr.com/0eb1b4af85107cd163413a5d8456edef/tumblr_mngqgrrtWg1sst44io1_400.jpg %}
{% img http://24.media.tumblr.com/19a7eebe4d1eed8da211d38ee1c75825/tumblr_mnikolbPlQ1sst44io1_400.jpg %}
{% img http://24.media.tumblr.com/4eecaf0a7307ada1595b2ceae1f0fd63/tumblr_mnimukqbd41sst44io1_400.jpg %}



###Arduino Sensor Shield V5.0
{% img http://24.media.tumblr.com/c61baa915d4c6b609c18ab8eb428ebd2/tumblr_mncx4xipVw1sst44io1_400.jpg %}

