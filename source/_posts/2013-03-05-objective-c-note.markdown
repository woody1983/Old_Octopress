---
layout: post
title: "Objective-C Note"
date: 2013-03-05 14:18
comments: true
categories: IOS
---

``` objective-c
#import <Foundation/Foundation.h>
@interface HelloWorld : NSObject
{
}
-(void)printGreeting;
@end

@implementation HelloWorld

-(void)printGreeting
{
		NSLog(@"Hello World!");
}

@end

#import "HelloWorld.h"

int main(void)
{
  HelloWorld* myObject = [[HelloWorld alloc] init];
  [myObject printGreeting];
  
  [myObject release];
  return 0;
}
```

<!-- more -->

###还有上午其他的例子

``` objective-c
//@"这里的内容应该作为Cocoa的NSString元素来处理"

// 定义函数
BOOL areIntsDifferent(int thing1, int thing2)
{
  if (thing1 == thing2) {
    return (NO);
  }
  else {
    return (YES);
  }
}

// 返回一个BOOL类型的值  接受2个int类型的参数

NSString *boolString (BOOL yesNo)
{
  if (yesNo == NO){
    return (@"NO");
  }
  else
  {
    return (@"YES");
  }
}

返回类型 函数名(参数类型 参数,参数类型1 参数1)
{
// ...  code in here
}

main函数里调用的时候 先声明一个BOOL类型的变量

BOOL areTheyDifferent;
areTheyDifferent = areIntsDifferent(5,5);

NSLog(@"are %d and %d different? %@", 5,5, boolString(areTheyDifferent));// 直接用返回值做参数

//定义一个类  @interface 将该类的数据成员和特性告诉给编译器 

@interface Circle : NSObject  // 类名 来自NSObject类
{
  ShapeColor fillColor;
  ShapeRect bounds;
} // 告诉编译器circle对象需要的数据成员
//下面是方法的声明 
-(void) draw; //方法需要参数的时候才需要一个冒号 否则就不需要
-(void) setFillColor : (ShapeColor) fillColor; //参数的类型在圆括号里定义!?
-(void) setBounds : (ShapeRect) bounds;

// void表示无返回值  -符号表示是类方法 区别于一般的函数原型和方法声明
// setFillColor 需要一个颜色参数
@end

// @implementation 中定义了类的实现部分

@implementation Circle

-(void) setFillColor: (ShapeColor) c //这个c是参数的重命名!!! @implementation 和 @interface的参数名不同是正确的。如果一致则会报错。
{
  fillColor = c;
}

-(void) setBounds: (ShapeRect) b
{
  bounds = b;
}

-(void) draw
{
  NSLog (@"drawing a circle at (%d %d %d %d) in %@",
    bounds.x, bounds.y,
	bounds.width, bounds.height,
	colorName(fillColor)
  );
}

@end
```
