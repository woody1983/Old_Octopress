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

@interface RadioStation : NSObject
{
    NSString *name;
    double frequency;
    NSUInteger band;
}

+(double)minAMFrequency;
+(double)maxAMFrequency;
+(double)minFMFrequency;
+(double)maxFMFrequency;

-(id)initWithName:(NSString *)newName atFrequency:(double)newFrequency;
-(NSString *)name;
-(void)setName:(NSString *)newName;
-(double)frequency;
-(void)setFrequency:(double)newFrequency;

@end
```

`RadioStation.m`

``` objective-c
#import "RadioStation.h"

@implementation RadioStation
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

-(id)initWithName:(NSString *)newName atFrequency:(double)newFrequency
{
    self = [super init];
    NSLog(@"initWithName is Running!\n newFrequency is %f , name is %@",newFrequency,newName);
    if (self != nil) {
        name = newName;
        frequency = newFrequency;
    }
    return self;
    
}

-(NSString *)name
{
    return name;
    NSLog(@"name is ok");
}
-(double)frequency{
    return frequency;
}

-(void)setName:(NSString *)newName{
    name = newName;
}


-(void)setFrequency:(double)newFrequency{
    frequency = newFrequency;
}

@end
```


`viewController.h`

``` objective-c
#import <UIKit/UIKit.h>
@class RadioStation;

@interface XYZViewController : UIViewController
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
#import "XYZViewController.h"
#import "RadioStation.h"
@interface XYZViewController ()

@end

@implementation XYZViewController

-(IBAction)buttonClick:(id)sender{
    [stationName setText:[myStation name]];
    [stationFrequency setText:[NSString stringWithFormat:@"%.1f",[myStation frequency]]];
    if (([myStation frequency] >= [RadioStation minFMFrequency]) &&
        ([myStation frequency] <= [RadioStation maxFMFrequency])) {
        [stationBand setText:@"FM"];
    }else{
        [stationBand setText:@"AM"];
    }
    NSLog(@"buttonClick frequency = %f", [myStation frequency]);
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    myStation = [[RadioStation alloc] initWithName:@"Woody Style" atFrequency:94.1];
    NSLog(@"App is Running");
  // Do any additional setup after loading the view, typically from a nib.
}

- (void)viewDidUnload
{
    [super viewDidUnload];
    // Release any retained subviews of the main view.
    
}

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    return (interfaceOrientation != UIInterfaceOrientationPortraitUpsideDown);
}

@end

```

{% img /images/img/oc_demo1.png  %}
