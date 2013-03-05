---
layout: post
title: "Objective-C Demo Note"
date: 2013-03-05 16:38
comments: true
categories: IOS
---

#Project:Single View Application
##MyFirstApp

`viewController.h`

``` objective-c
#import <UIKit/UIKit.h>

@interface ViewController : UIViewController{
  IBOutlet UILabel *nameLabel //新增一个数据成员
}
-(IBAction)showName:(id)sender; //新增一个方法showName
@end
```


`viewController.m`

``` objective-c
#import "viewController.h"

@implementation ViewController
-(void)didReceiveMemoryWaring
{
  [super didReceiveMemoryWaring];
  // 自带的？ Release any cache data, images, etc that aren't in use.
}

-(IBAction)showName:(id)sender
{
  [nameLabel setText:@"My name is Johnny!"];
}
```

* @interface 是声明一个类 大括号里面定义的是这个类的数据成员
* 在@end 之前定义的都是类方法  只定义 实现在@implementation部分实现
* didReceiveMemoryWaring这个方法好像是自定义出来的

<!-- more -->

#Project RadioStation

###添加一个Objective-C Class : RadioStation

`RadioStation.h`

``` objective-c
#import <Foundation/Foundation.h>

@interface RadioStation :NSObject
{
  NSString *name;
  double frequency;
  NSUInteger band;
}
+(double)minAMFrequency;
+(double)maxAMFrequency;
+(double)minFMFrequency;
+(double)maxFMFrequency;

-(id)initWithName:(NSString *)newName atFrenquency:(double)newFrequency;
-(NSString *)name;
-(void)setName:(NSString *)newName;
-(double)frequency;
-(double)setFrequency:(double)newFrequency;

@end
```

`RadioStation.m`

``` objective-c
#import "RadioStation.h"

@implementation 
+(double)minAMFrequency
{
  return 520.0;
}

+(double)maxAMFrequency
{
  return 1610.0;
}

+(double)minFMFrequency
{
  return 88.3;
}

+(double)maxFMFrequency
{
  return 107.9;
}

-(id)initWithName:(NSString *)newName atFrenquency:(double)newFrequency {
  self = [super init];
  if (self != nil){
    name = newName;
	frequency = newFrequency;
  }
  return self;
}

-(NSString *)name{
  return name;
}

-(void)setName:(NSString *)newName{
  name = newName;
}

-(double)frequency{
  return frequency;
}

-(double)setFrequency:(double)newFrequency{
  frequency = newFrequency;
}

@end
```


`viewController.h`

``` objective-c
#import <UIKit/UIKit.h>
@class RadioStation;

@interface ViewController : UIViewController
{
  RadioStation *myStation;
  IBOutlet UILabel* stationName;
  IBOutlet UILabel* stationFrequency;
  IBOutlet UILabel* stationBand;
}
@end
```

`viewController.m`

``` objective-c
#import "RadioStation.h"
@implementation ViewController
-(void)didReceiveMemoryWaring
{
  [super didReceiveMemoryWaring];
  // 自带的？ Release any cache data, images, etc that aren't in use.
}

-(void)viewDidLoad
{
  [super viewDidLoad];
  //code in here...
  myStation = [[RadioStation alloc] initWithName:@"STAR 94" atFrenquency:94.1];
}

```
