---
layout: post
title: "Fast enumeration"
date: 2013-08-14 23:50
comments: true
categories: IOS
---

###能多快速？

>Objective-C is good at making decisions, but it’s great at being lazy.

#### 笨方法循环打印数组内容
``` objective-c
NSArray *funnyWords = @[@"Schadenfreude", @"Portmanteau", @"Penultimate"];

NSLog(@"%@ is a funny word", funnyWords[0]);
NSLog(@"%@ is a funny word", funnyWords[1]);
NSLog(@"%@ is a funny word", funnyWords[2]);
``` 

#### 更简洁的方法是

``` objective-c
NSArray *funnyWords = @[@"Schadenfreude", @"Portmanteau", @"Penultimate"];

for (NSString *word in funnyWords) {
  NSLog(@"%@ is a funny word", word);
}
``` 

###练习
``` objective-c
NSArray *newHats = @[@"Cowboy", @"Conductor", @"Baseball"];
NSString *hat; //记得声明
for (hat in newHats) {

  NSLog(@"Trying on new %@ hat", hat);

  if([mrHiggie tryOnHat:hat]) {
    NSLog(@"Mr. Higgie loves it");
  } else {
    NSLog(@"Mr. Higgie hates it");
  }
}
```