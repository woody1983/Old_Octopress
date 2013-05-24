---
layout: post
title: "Arduino PWM Control Led"
date: 2013-05-24 11:54
comments: true
categories: Arduino
---

###几个概念

* LED长脚接(+) 
* Digital 的pin脚可以代替(+) 另一端需接GND地线
* resistor 要选择合适的值 否则电压会过小或过大
* 串联和并联的区别 我初中白念了
* analogWrite 模拟传值

<!-- more -->

``` c
const int redled_1=5; 
const int redled_2=4; 
const int greenled=3;

const int switchPins=2;

int brightness = 0; 
int fadeAmount = 5;

void setup()
{ 
// 把3,4,5腳定義為輸出 
  pinMode(redled_1,OUTPUT); 
  pinMode(redled_2,OUTPUT); 
  pinMode(greenled,OUTPUT); 
// 把第2腳定義為輸入 
  pinMode(switchPins,INPUT); 
}

void loop()
{ 
  int switchstate; 
  // 用 digitalRead(2) 來讀取第2腳開關的狀態 
  switchstate = digitalRead(switchPins);

  // 如果案件沒有按下，就只亮綠燈 
  if (switchstate == LOW) 
  { 
    //digitalWrite(3, HIGH); 
    // 將第3腳的綠燈點亮 
    analogWrite(3, brightness);
    brightness = brightness + fadeAmount; 

    if (brightness == 0 || brightness == 255) 
      { 
        fadeAmount = -fadeAmount ; 
      }
    delay(30);
    digitalWrite(4, LOW); 
    // 將第4腳的紅燈熄滅 
    digitalWrite(5, LOW); 
    // 將第5腳的紅燈熄滅 
   } 
    // 如果按鍵按下了，就會進入下面的程式碼 
   else 
     { 
       digitalWrite(3, LOW); 
       // 將第3腳的綠燈熄滅 
       digitalWrite(4, LOW); 
       // 將第4腳的紅燈熄滅 
       digitalWrite(5, HIGH); 
       // 將第5腳的紅燈點亮 
       // 等 250 mini-second 後再改變燈號 
       delay(250); 
       digitalWrite(4, HIGH); 
       // 將第4腳的紅燈點亮 
       digitalWrite(5, LOW); 
       // 將第5腳的紅燈熄滅 
       // 等 250 mini-second 後再改變燈號 
       delay(250); 
     } 
}

```
