---
layout: post
title: "Objective-C Note:Class init alloc"
date: 2013-03-08 16:58
comments: true
categories: IOS 
---


``` objective-c
#import <Foundation/Foundation.h>

@interface BNRItem : NSObject
{
  NSString *itemName;
  NSString *serialNumber;
  int valueInDollars;
  NSDate *dateCreated;
}
@end
```
### 对象会直接保存非指针类型的实例变量，BNRItem 对象并没有直接保存其他对象，而只拥有三个指针，分别指向另外三个对象。这些指针都是BNRItem的实例变量。
### 也就是说 BNRItem通过一个itemName的指针 指向一个NSString对象。

{% img /images/img/nsobject.jpg %}
<!-- more -->

#实例变量的存取

##存方法
###修改实例的变量，指向另一个字符串，该BNRItem对象会将新的NSString对象命名为itemName
``` objective-c
-(void)setItemName:(NSString *)newItemName
{
  itemName = newItemName;
}
```

##取方法
###返回BNRItem名为itemName的实例变量 一个指向NSString对象的指针
``` objective-c
-(NSString *)itemName
{
  return itemName;
}
```
##调用
对BNRItem对象使用setItemName和itemName方法即可实现设置和读取该变量的动作。

##例子
``` objective-c
//声明
BNRItem *p = [[BNRItem alloc] init];
//设置
[p setItemName:@"Red Sofa"];
//读取
NSString *str = [p itemName];
//输出
NSLog(@"%@", str);
```
>但在Objective-C中，取方法的方法名就是实例变量的变量名。

##覆盖某种方法 直接定义就可以了
##关于实例说明
Description是NSObject类的方法，是一个介绍该类的方法。因为这个类已经在父类NSObject中声明过了，因此只要在定义文件中重新覆盖即可。
``` objective-c
-(NSString *)description
{
  NSString *descriptionString = [[NSString alloc] initWithFormat:@"%@ (%@): Worth $%d, recorded on %@",
                                                   itemName,serialNumber,valueInDollars,dateCreated];
  return descriptionString;
}

>向某个BNRItem实例发送description消息就会得到描述该实例的NSString对象，一般情况下格式化说明%@ 程序就先调用实参的description方法。

NSLog(@"%@",p);
```

* alloc 向某个类发送这个信息 是创建该类的实例并得到指向新实例的指针
* init 为实例变量设置初始值 编写更复杂的类时 需要创建更多的初始化方法 类似init 但会带参数


##原先的代码
``` objective-c
#import <Foundation/Foundation.h> 
@interface BNRItem : NSObject 
{ 
NSString *itemName; 
NSString *serialNumber; 
int valueInDollars; 
NSDate *dateCreated; 
} 
- (void)setItemName:(NSString *)str; 
- (NSString *)itemName; 

- (void)setSerialNumber:(NSString *)str; 
- (NSString *)serialNumber;

- (void)setValueInDollars:(int)i; 
- (int)valueInDollars; 

- (NSDate *)dateCreated; 
@end 
```
### 要新增的方法

``` objective-c
- (id)initWithItemName:(NSString *)name 
valueInDollars:(int)value 
serialNumber:(NSString *)sNumber;
```

>这些参数都有类型和参数名。声明参数时，要先写标签，然后写类型（写在圆括号里），
最后写参数名。以initWithItemName:valueInDollars:serialNumber:为例， 和
initWithItemName:标签对应的参数是指向NSString对象的指针。在该方法的程序段中，
可以用name来引用该指针所指向的NSString对象。

这个方法返回的是一个id类型，OC中ID可以指向任意对象的指针。

#实现指定初始化方法
``` objective-c
@implementation BNRItem 
- (id)initWithItemName:(NSString *)name 
valueInDollars:(int)value 
serialNumber:(NSString *)sNumber 
{ 
// 调用父类的指定初始化方法
  self = [super init]; 
// 为实例变量设定初始值
  [self setItemName:name]; 
  [self setSerialNumber:sNumber]; 
  [self setValueInDollars:value]; 
  dateCreated = [[NSDate alloc] init]; 
// 返回初始化后的对象的地址
  return self; 
} 
```
##self
`self`存在于方法中，是一个隐式（implicit）局部变量。编写方法时不需要声明self，并且程序会自动为self赋值，通常情况下，self会用来向对象自己发送消息。

>初始化方法的最后一行代码必须返回初始化后的对象。

##Super
覆盖某个类的某个方法时 需要保留该方法在父类中的实现，在其基础上扩充子类的实现。

``` objective-c
- (void)someMethod 
{ 
[self doMoreStuff]; 
[super someMethod]; 
} 
```
>以BNRItem的指定初始化方法为例，向super发送init消息会调用NSObject的init。
``` objective-c
 self = [super init]; 
```
这一步其实是先调用父类的初始化方法后 先将得到的返回值赋给self变量 如果变量是不是nil，如果不是就继续。
 
##一个类可以有多个初始化方法
``` objective-c
- (id)initWithItemName:(NSString *)name; 
//和之前initWithItemName的声明看起来貌似只少了几个参数而已
```

>实现initWithItemName:方法时，不需要将指定初始化方法中的代码搬过来再重写一遍。它只需要调用指定初始化方法，将得到的实参作为itemName传入，而其他实参则使用某个默认值传入，代码如下：
``` objective-c
- (id)initWithItemName:(NSString *)name 
{ 
return [self initWithItemName:name 
valueInDollars:0 
serialNumber:@""]; 
} 
```
>这种串联（chain）使用初始化方法的机制可以降低出错的概率，也更容易维护代码。在创建拥有多个初始化方法的类时，需要先将其中的某个初始化方法确定为“指定初始化方法”。然后只在指定初始化方法中编写初始化的核心代码，其他初始化方法只需要调用该核心代码并输入默认值即可。

##初始化方法的调用方向

{% img /images/img/nsobject2.jpg %}

####覆盖BNRItem.m中的init方法，调用指定初始化方法，并使用默认值输入所有的实参，代码如下：
``` objective-c
- (id)init { 
return [self initWithItemName:@"Item" 
valueInDollars:0 
serialNumber:@""]; 
} 
```
#使用初始化方法

main.m会向BNRItem实例发送init消息，因为BNRItem类实现了上述两个初始化方法，所以程序会调用该类所实现的init方法，而该方法又会调用initWithItemName:valueInDollars：serialNumber:并输入默认值

测试:main.m中加入一行代码
``` objective-c
BNRItem *p = [[BNRItem alloc] init]; 
NSLog(@"%@", p);
// 创建一个新的NSString对象“Red Sofa”，并传给BNRItem对象
[p setItemName:@"Red Sofa"]; 
```
main.h

``` objective-c
#import <Foundation/Foundation.h> 
#import "BNRItem.h" 
int main (int argc, const char * argv[]) 
{ 
@autoreleasepool { 
NSMutableArray *items = [[NSMutableArray alloc] init]; 
BNRItem *p = [[BNRItem alloc] initWithItemName:@"Red Sofa" 
valueInDollars:100 
serialNumber:@"A1B2C"]; 
NSLog(@"%@", p); 
items = nil; 
} 
return 0; 
} 
```
