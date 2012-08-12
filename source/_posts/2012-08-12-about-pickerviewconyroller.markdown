---
layout: post
title: "About PickerViewController"
date: 2012-08-12 12:44
comments: true
categories: IOS
---

####Pickers get their data from a datasource and tell their delegates when something happens

`UIPickerViewDatasource.` and `UIPickerViewDelegate.`

Controls have their own specific datasources and delegates
<!-- more-->
####First, declare that the controller conforms to both protocols

``` objective-c
@interface InstaEmailViewController : UIViewController
<UIPickerViewDataSource, UIPickerViewDelegate> {
	NSArray* activities_;
	NSArray* feelings_;
} 
@end
```

然后在InstaEmailViewController.m文件中去定义array的具体内容

``` objective-c InstaEmailViewController.m
- (void)viewDidLoad
{
	[super viewDidLoad];
	activities_ = [[NSArray alloc ] initWithObjects:@"sleeping",@"eating",@"working",@"thinking", nil];
	feelings_ =[[NSArray alloc ]initWithObjects:@"awesome",@"sad",@"happy",@"ambivalent",@"nauseous", nil];
		    	// Do any additional setup after loading the view, typically from a nib.
}
```
记得要Release出去

``` objective-c
-(void)dealloc{
	[emailPicker_ release];
	[activities_ release];
	[feelings_ release];
	[super dealloc];
}
```

###The datasource protocol has two required methods
``` objective-c
-(NSInteger)numberOfComponentsInPickerView:(UIPickerView *) pickerView {
	    return 2;
} //要返回有多少个components(组件)

-(NSInteger)pickerView:(UIPickerView *)pickerView numberOfRowsInComponent:(NSInteger)component {
	if (component == 0) {
		return [activities_ count];
	}
	else {
		return [feelings_ count];
	}
}
```

这一步做完以后 将`Outlets`下的Datasource link到`File's Owner`上

###There’s just one method for the delegate protocol

``` objective-c
- (NSString *)pickerView:(UIPickerView *)pickerView titleForRow:(NSInteger)row forComponent:(NSInteger)component {
	if (component == 0) {
			return [activities_ objectAtIndex:row];
	} 
	else { 
			return [feelings_ objectAtIndex:row];
	}
		return nil; 
}

```
一样 也是link到Outlets上。




