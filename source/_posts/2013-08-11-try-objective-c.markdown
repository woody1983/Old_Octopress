---
layout: post
title: "Try Objective-C"
date: 2013-08-11 23:30
comments: true
categories: IOS
---

###Code School 上的免费教程，为什么要着急先把这个免费的学掉 我也不知道具体是为什么。一个月学费20$ 总是出现在我最忙的时候。月底还有考试，真TMD服了。

<!-- more -->

##常规的一些语法 变量 输出

``` objective-c

NSLog(@"Hello,Mr.Johnny");
NSString *firstName = @"Johnny Hsu";
NSLog(firstName); //Logging a variable
NSLog(@"Hello there, %@.", firstName );
NSLog(@"%@ %@", firstName, firstName);

NSString *lastName = @"Hsu";

NSLog(@"%@ %@", firstName, lastName);
NSNumber *age = @30; //Creating a number object
/*
NSNumber *ipodBirthdayYear = @2001;
NSNumber *iphoneBirthdayYear = @2007;
NSNumber *ipadBirthdayYear = @2010;
*/

//Log a number object
NSLog(@"%@ is %@ years old", firstName, age);

//Creating an array variable

NSArray *apps = @[@"AngryFowl",@"Lettertouch",@"Tweetrobot"];

//Accessing values in an array
NSLog(@"%@", apps[1]); //challenge[11]: Lettertouch

//Changing an array
apps = @[@"AngryFowl",@"Lettertouch",@"Tweetrobot",@"Instacanvas"];

## Dictionary还是蛮实用的。

//Creating a dictionary
NSDictionary *person = @{@"First Name": @"Johnny"};
NSDictionary *appRatings = @{@"AngryFowl": @3, @"Lettertouch": @5};

//Accessing values in a dictionary
NSLog(@"Lettertouch has a rating of %@.", appRatings[@"Lettertouch"]);

```

#基础部分结束


#Objective-C 有一个特性就是调用一个方法 在这里称之为传递一个信息 不过这么看倒反而形象了。

``` objective-c
//Sending a message
[tryobjc completeThisChallenge];

//-------------------------Sending the description message
/*
Try sending the description message to the foods array inside of NSLog() to see what it returns.
*/
NSArray *foods = @[@"tacos", @"burgers"];

NSLog(@"%@", [foods description]);
//--------------------------Storing the result of a message
//NSString *result = [myArrayObject description];

NSArray *foods = @[@"tacos", @"burgers"];             
NSString *result = [foods description];

NSLog(@"%@", result);

//--------------------------Trying to log an NSUInteger
NSString *city = @"Ice World";
NSUInteger cityLength = [city length];

NSLog(@"City has %lu characters", cityLength);
//If you want to log an NSUInteger, you can use the %lu placeholder instead of %@.
``` 

##Operating on NSNumbers

``` objective-c
/*
NSNumber *higgiesAge = @6;
NSNumber *phoneLives = @3;

NSUInteger higgiesAgeInt = [higgiesAge unsignedIntegerValue];
NSUInteger phoneLivesInt = [phoneLives unsignedIntegerValue];

NSUInteger higgiesRealAge = higgiesAgeInt * phoneLivesInt;
*/
NSNumber *higgiesAge = @6;                                                                   
NSNumber *phoneLives = @3;

NSUInteger higgiesAgeInt = [higgiesAge unsignedIntegerValue];
NSUInteger phoneLivesInt = [phoneLives unsignedIntegerValue];

NSUInteger product = higgiesAgeInt * phoneLivesInt;
NSLog(@"Higgie is actually %lu years old.", product);


//----------------------Appending 2 strings
NSString *firstName = @"Johnny Hsu";
NSString *lastName = @"Hsu";

NSString *fullName = [firstName stringByAppendingString:lastName];
NSLog(@"%@", fullName);
/*
Well, it turns out that we can. There is a method on NSString called stringByAppendingString: 
that takes a single NSString argument and appends it to the NSString object that received the 
stringByAppendingString: message, returning the full string. So we can rewrite the above code like so:
*/
```  

#Nesting messages

``` objective-c
NSString *firstName = @"Johnny Hsu";
NSString *lastName = @"Hsu";

NSString *fullName = [[firstName stringByAppendingString:@" "] stringByAppendingString:lastName ];
 /*//Append firstName and lastName with a space between//*/;

NSLog(@"%@", fullName);

//---------------------Working with long message names
NSString *firstName = @"Johnny";
NSString *lastName = @"Hsu";

NSString *fullName = [[firstName stringByAppendingString:@" "]
                        stringByAppendingString:lastName];

NSString *replaced = [fullName stringByReplacingOccurrencesOfString:firstName
                                                         withString:lastName];

NSLog(@"%@", fullName);
NSLog(@"%@", replaced);
/*
challenge[9]: Johnny Hsu
challenge[10]: Hsu Hsu
*/

//-------------------------Creating an NSString with a message
NSString *firstName = @"Johnny Hsu";

NSString *copy = [NSString stringWithString:firstName];

NSLog(@"%@ is a copy of %@", copy, firstName);
//challenge[5]: Johnny Hsu is a copy of Johnny Hsu

//-------------------------Creating an NSString with alloc/init
/*
NSString *emptyString = [[NSString alloc] init];
NSArray *emptyArray = [[NSArray alloc] init];
NSDictionary *emptyDictionary = [[NSDictionary alloc] init];
*/
NSString *firstName = @"Johnny Hsu";

NSString *copy = [[NSString alloc] initWithString:firstName];

NSLog(@"%@ is a copy of %@", copy, firstName);
//challenge[5]: Johnny Hsu is a copy of Johnny Hsu

//-------------------------Refactoring string combination
NSString *firstName = @"Johnny Hsu";
NSString *lastName = @"Hsu";

NSString *fullName = [NSString stringWithFormat:@"%@ %@",firstName,lastName];

NSLog(@"%@", fullName);

``` 

#判断啥的 登场吧

``` objective-c

//----------------------A simple if
BOOL mrHiggieIsMean = YES;

if (mrHiggieIsMean) {
  NSLog(@"Confirmed: he is super mean");
}
//----------------------if - else
BOOL mrHiggieIsMean = [mrHiggie areYouMean];

if (mrHiggieIsMean) {
  NSLog(@"Confirmed: he is super mean");
}else{
  NSLog(@"Confirmed: he isn't super mean");
}
//----------------------if - else if - else
/*
+----------------+-----------------------------------------+
| Meanness Range |               Log String                |
+----------------+-----------------------------------------+
| 0-3            | Mr. Higgie is on the nice side          |
| 4-7            | Mr. Higgie is sorta nice but not really |
| 8-10           | Mr. Higgie is definitely mean           |
+----------------+-----------------------------------------+
*/
NSNumber *meannessScale = [mrHiggie meannessScale];

if([meannessScale intValue] < 4) {
  NSLog(@"Mr. Higgie is on the nice side");
} else if([meannessScale intValue] < 8 && [meannessScale intValue] >3) {
  NSLog(@"Mr. Higgie is sorta nice but not really");
} else {
  NSLog(@"Mr. Higgie is definitely mean");
}

//----------------------Equal strings
NSString *hat = [mrHiggie currentHat];

if([hat isEqualToString:@"Sombrero"]) {
  NSLog(@"Ese es un muy buen sombrero");
} else if ([hat isEqualToString:@"Fedora"]) {
  NSLog(@"Mr. Higgie was an iPhone before there was iPhone");
} else if ([hat isEqualToString:@"AstronautHelmet"]) {
  NSLog(@"Mr. Higgie was an AstronautHelmet");
} else {
  NSLog(@"Mr. Higgie is currently hatless");
}

//--------------------Switch it up
//Mission:He has requested that we please update the switch to let him wear his AstronautHelmet hat on the weekends.
NSInteger day = getDayOfWeek();

switch (day) {
  case 1:
  case 2:
  case 3:
  case 4: {
    [mrHiggie setCurrentHat:@"Fedora"];
    break;
  }
  case 5: {
    [mrHiggie setCurrentHat:@"Sombrero"];
    break;
  }
  case 6:{
    [mrHiggie setCurrentHat:@"AstronautHelmet"];
    break;
  }
  case 7: {
    [mrHiggie setCurrentHat:@"AstronautHelmet"];
    break;
  }
}

NSLog(@"Mr. Higgie is wearing: %@", [mrHiggie currentHat]);

//-------------------------Switch on enums
typedef NS_ENUM(NSInteger, DayOfWeek) {
    DayOfWeekMonday = 1,
    DayOfWeekTuesday = 2,
    DayOfWeekWednesday = 3,
    DayOfWeekThursday = 4,
    DayOfWeekFriday = 5,
    DayOfWeekSaturday = 6,
    DayOfWeekSunday = 7
};

//Mission:Finish converting the rest of the cases to use the enum values.
DayOfWeek day = getDayOfWeek();

switch (day) {
    case DayOfWeekMonday:
    case DayOfWeekTuesday:
    case DayOfWeekWednesday:
    case DayOfWeekThursday: {
        [mrHiggie setCurrentHat:@"Fedora"];
        break;
    }
    case DayOfWeekFriday: {
        [mrHiggie setCurrentHat:@"Sombrero"];
        break;
    }
    case DayOfWeekSaturday:
    case DayOfWeekSunday: {
        [mrHiggie setCurrentHat:@"AstronautHelmet"];
        break;
    }
}

NSLog(@"Mr. Higgie is wearing: %@", [mrHiggie currentHat]);


``` 

#Obviously 还没写完 不能对一个拖延症患者要求太高不是。












