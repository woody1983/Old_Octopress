---
layout: post
title: "iDecide's logic"
date: 2012-08-11 23:11
comments: true
categories: IOS
---

####iDecide’s logic

Any controls you create need a method that Interface Builder can use to connect the control to behaviors specified in the implementation file.

The .xib file describes the button as you configured it in Interface Builder.
You’ll likely notice all the files’ names have “ViewController”
in them. Don’t sweat that for now, we’ll explain that in a bit.

iDecideViewController.h
This line declares a method called buttonPressed that Interface Builder will recognize as a possible callback.

iDecideViewController.m
You provide the method implementation in the
.m file. Here’s where you code up what should actually happen when the button is pressed.


####IBOutlet与IBAction
通过在变量前增加IBOutlet来说明该变量将与界面上的某个UI对象对应，在方法前增加IBAction来说明该方法将与界面上的事件对应.
<!-- more -->

Below is the code for when the button gets tapped. Add the bolded code to the iDecideViewController.h and iDecideViewController.m files. We are creating three things: the UILabel property, the IBAction to respond to the button press, and the IBOutlet to change the label when the button is pressed.

###iDecideViewController.h

`UILabel *decisionText_;`

We need to change the label text to provide our answer, so we need an IBOutlet to be able to get to the label control that the framework will build from our nib.

`@property (retain, nonatomic) IBOutlet UILabel *decisionText;`

We’ll talk more about properties later in the book.

`-(IBAction)buttonPressed:(id)sender;`

Here’s the action that will be called when the button is pressed.

###iDecideViewController.m
`@synthesize decisionText=decisionText_;`

The @synthesize tells the compiler to create a getter and setter for the property we declared in the header file. We’ll get into that more in Chapter 3.

`-(IBAction)buttonPressed:(id)sender`

This is the implementation of the method that gets called when the button is pressed.

`decisionText_.text = @”Go for it!”;`

We’ll use our reference to the label to change the text.


Click on the circle next to New Referencing Outlet and drag it to the @property statement for the Outlet in the .h file on the right. Now when the decisionText UILabel is generated, our decisionText property will reference the control, through the IBOutlet.

``` objective-c
//
//  iDecideViewController.h
//  iDecide
//
//  Created by Woody.Xu on 12-8-11.
//  Copyright (c) 2012年 Woody.Xu. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface iDecideViewController : UIViewController{
	    UILabel *decisionText;
}

@property (retain,nonatomic) IBOutlet UILabel *decisionText;

-(IBAction)buttonPressed:(id)sender;

-(IBAction)buttonPressed_redo:(id)sender;

-(IBAction)buttonPressed_green:(id)sender;

@end

```
and

``` objective-c
#import "iDecideViewController.h"

@interface iDecideViewController ()

@end

@implementation iDecideViewController
@synthesize decisionText=decisionText_;

-(IBAction)buttonPressed:(id)sender{
	decisionText_.text=@"Go for it!";
	decisionText_.textColor= [UIColor blackColor];
}


-(IBAction)buttonPressed_redo:(id)sender{
	decisionText_.text=@"You wang redo?";
	decisionText_.textColor= [UIColor redColor];
}

-(IBAction)buttonPressed_green:(id)sender{
	decisionText_.text=@"You choose Green";
	decisionText_.textColor= [UIColor greenColor];
}
```

