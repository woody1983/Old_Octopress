---
layout: post
title: "Objective-C Note2"
date: 2013-03-06 16:12
comments: true
categories: IOS
---


>今天的笔记 Objective-C感觉是一种很细腻的语言。

``` objective-c
@interface MyObject : NSObject {
[instance variables]
}

[method declarations]
@end

@implementation MyObject
[method implementations]
@end
```

###Method declarations in Objective-C look like this:
``` objective-c
-(void) launchPlane;
```
###Here, for example, is a method that takes a single parameters:
``` objective-c
-(void) launchPlane:(NSString *)planeName;
```
###two parameters:
``` objective-c
-(void) launchPlane:(NSString *)planeName fuelCapacity:(int)litresOfFuel;
```

###实现的时候
``` objective-c
[planeLauncher launchPlane];

[planeLauncher launchPlane:@"Boeing 747-300" fuelCapacity:183380];
```
<!-- more -->
>The method declarations for a class are kept in its interface. Here’s an example of what the interface for a class that defines some methods looks like:
``` objective-c
@interface SomeObject: NSObject
- (void) launchPlane:(NSString*)planeName;
- (int) numberOfPlanesInTheAir;
@end
```

##Properties
>In object-oriented programming, it’s considered bad practice for one object to directly access another object’s data. Doing so breaks encapsulation, because it means that one object’s code is now dependent on the data stored in another.In order to access and change another object’s variables, you use a pair of instance methods known as a setterand getter. The getter method returns the current value of the variable, and the setter method changes the value.
In Objective-C, setter and getter method names must follow an established pattern. For example, given an instance variable named  `planeName`, the setter method would be named `setPlaneName:`and the getter method would be named `planeName`.

###Declaring a property in an Objective-C class looks like this:
``` objective-c
@interface SomeClass: NSObject
@property (strong, nonatomic) NSObject* myProperty;
@end
```
#关于Property
###strong
This property is a strong (owning) reference to an object; see  “Object Graphs in
Objective-C”(page  32). Using  strong  and  weakproperties controls whether the ob‐
ject referred to by the property stays in memory or not.
###weak
This property is a weak (nonowning) reference to an object. When the object re‐
ferred to by this property is deallocated, this property is automatically set to nil.
###assign
This property’s setter method simply assigns the property’s variable to whatever is
passed in, and performs no memory management.
###copy
This property’s setter copies any object passed to it, creating a duplicate object.
###readwrite
This property generates both getter and setter methods. (This attribute is set by
default—you need to explicitly use it only when overriding a superclass’s property.)
###readonly
This property does not generate a setter method, rendering the property read-only
by other classes. (Your class’s implementation code can still modify the property’s
variable, however.)
###nonatomic
This property’s setter and getter do not attempt to get a lock before making changes
to the variable, rendering it thread-safe.

``` objective-c
@implementation MyClass
@synthesize myProperty = _myCustomVariableName;
// the rest of the class code goes here
@end
```

##或者不用@synthesize  而使用@dynamic 来实现getter and setter methods
``` objective-c
@implementation MyClass
@dynamic myProperty;
- (int) myProperty {
// this is the getter method for this property
return 123;
}
- (void) setMyProperty:(int)newValue {
// this is the setter method for this property
}
@end
```

##Protocols
>A protocolis a list of methods that your class promises to implement. Protocols are used to mark classes as having certain capabilities, like the ability to be copied, to be serialized and deserialized, or to act as a data source for some other class.
``` objective-c
@protocol SomeProtocol
[ method declarations ]
@end
//用处
You can mark a class as conformingto a protocol by declaring so in the class’s interface:
@interface SomeObject: NSObject <SomeProtocol>
@end
```

###alloc只是给对象保留内存空间，如果要使用必须准备初始化动作。
``` objective-c
SomeClass* anObject = [[SomeClass alloc] init];
```

>Some classes use a different designated initializer, or have multiple initializers you can use. For example, the NSStringclass has several—here are a few:
一些类使用不同的多个初始化方法或多个初始化 
``` objective-c
NSString* myString  = [[NSString alloc] initWithFormat:@"here's a number: %i", 123];
NSString* anotherString = [[NSString alloc] initWithData:anNSDataObject encoding:NSUTF8Encoding];
NSString* oneMoreString = [[NSString alloc] initWithContentsOfFile:@"path to a file" encoding:NSUTF8Encoding error：someErrorPointer];
```

#Foundation

##String

Strings are stored in the  NSStringclass, which makes them Objective-C objects just like
everything else. You can create an empty string with this code

``` objective-c
NSString* aString = [[NSString alloc] init];
//Doing this isn’t terribly useful, because the NSString class is immutable
NSString* aString = @"Hello, world!";

@表示一个NSString对象 可以接受信息，和其他对象互动。
//it can receive messages and generally interact other objects in the application. 

NSInteger sizeOfString = [@"Hello, world!" length];
//将这个NSString对象的长度提取出来

还有一些内置方法 比如 大小 小写 首字母大写之类的

NSString* originalString  = @"This is An EXAMPLE";
// "THIS IS AN EXAMPLE"
NSString* uppercaseString  = [originalString uppercaseString];
// "this is an example"
NSString* lowerCaseString  = [originalString lowercaseString];
// "This Is An Example"
NSString* capitalizedString = [originalString capitalizedString];
//To get the first five characters in a string, you do this:
NSString* startSubstring = [originalString substringToIndex:5]; // "This "
//To get everything past the first five characters:
NSString* endSubstring = [originalString substringFromIndex:5]; // "is An EXAMPLE"
//To get a substring of a range of characters, you first create an NSRangestructure, which
//defines the start point and length of the range. For example, to create an NSRangethat 
//starts at the third character and is five characters long, you do this:
NSRange theRange = NSMakeRange(2,5);
NSString* substring = [originalString substringWithRange:theRange]; // "is is"
```

###Structures and Objects
``` objective-c
struct CGPoint {
float x;
float y;
};


CGPoint somePoint;
somePoint.x = 123;
somePoint.y = 456;
// This is a variable that contains a pointer to an NSString
NSString* someString;
// This is a variable that contains a CGPoint
CGPoint somePoint;
```
###判断NSString
``` objective-c
if (firstString == secondString) {
// Do something
}
if ([firstString isEqualToString:secondString]) {
// Do something
}
```
##Searching Strings
``` objective-c
NSString* sourceString = @"Four score and seven years ago";
NSRange range = [sourceString rangeOfString:@"seven"];
if (range.location == NSNotFound) {
// the string was not found
} else {
// the string was found; 'range' variable contains info on where it is
}
```

##还有一些选项
``` objective-c
NSString* sourceString = @"Four score and seven years ago";
NSRange range = [sourceString rangeOfString:@"SEVEN"
options:NSCaseInsensitiveSearch];
```

##Arrays
###Declaration:
``` objective-c
NSArray* myArray = @[@"one", @"two", @"three"];
###You can also retrieve objects from an array, using syntax like this:
NSString* oneString = myArray[0];
NSString* twoString = myArray[1];

//The syntax for accessing elements in an NSArray doesn’t work on iOS 5 and below.
//Instead, you need to use the slightly wordier method objectAtIndex:, like so:
NSString* oneString = [myArray objectAtIndex:0];

int count = myArray.count;
// count now equals 3

NSArray* myArray = @[@"one", @"two", @"three"];
int index = [myArray indexOfObject:@"two"];  // should be equal to 1
if (index == NSNotFound) {
NSLog(@"Couldn't find the object!");
}

//Here’s an example of creating a subarray from an existing array:
NSArray* myArray = @[@"one", @"two", @"three"];
NSRange subArrayRange = NSMakeRange(1,2);
NSArray* subArray = [myArray subArrayWithRange:subArrayRange];
// subArray now contains "two", "three"

###Fast Enumeration
To loop over an array, you do this:

NSArray* myArray = @[@"one", @"two", @"three"];
for (NSString* string in myArray) {
// this code is repeated 3 times, one for each item in the array
}

###Mutable Arrays
NSMutableArray* myArray = [NSMutableArray arrayWithArray:@[@"One", @"Two"]];
// Add "Three" to the end
[myArray addObject:@"Three"];
// Add "Zero" to the start
[myArray insertObject:@"Zero" atIndex:0];
// The array now contains "Zero", "One", "Two", "Three"

NSMutableArray* myArray = [NSMutableArray arrayWithArray:@[@"One", @"Two", @"Three"]];
[myArray removeObject:@"One"];  // removes "One"
[myArray removeObjectAtIndex:1]; // removes "Three", the second
// item in the array at this point
// The array now contains just "Two"

NSMutableArray* myArray = [NSMutableArray arrayWithArray:@[@"One", @"Two",@"Three"]];
[myArray replaceObjectAtIndex:1 withObject:@"Bananas"];
// myArray is now "One", "Bananas", "Three"
You can also ask the mutable array to set an object at a given index:
myArray[0] = @"Null";
```

##Dictionaries
/*
Table 3-1. Contact information
Key      Value
Name     Cave Johnson
Company  Aperture Science
Likes    Science
Dislikes Lemons
*/
``` objective-c
NSDictionary* translationDictionary = @{
@"greeting": @"Hello",
@"farewell": @"Goodbye"
};
//You can retrieve a value from the dictionary in a similar way to how you get objects out of an NSArray:
NSDictionary* translationDictionary = @{@"greeting": @"Hello"};
NSString* greeting = translationDictionary[@"greeting"];

// Here, aDictionary is an NSDictionary
for (NSString* key in aDictionary) {
NSObject* theValue = aDictionary[key];
// do something with theValue
}

//To set an object for a key in a mutable dictionary
NSMutableDictionary* aDictionary = @{};
aDictionary[@"greeting"] = @"Hello";
aDictionary[@"farewell"] = @"Goodbye";

###To create an NSNumber from a number, simply put an @ in front of it. 
NSNumber* theNumber = @123;
//This NSNumberinstance can be included in any collection object:

//'numbers' is an NSMutableArray
[numbers addObject:theNumber];
//This NSNumberinstance can be included in any collection object:

//'numbers' is an NSMutableArray
[numbers addObject:theNumber];
//You can also set NSNumbers to the result of an expression. For example:
int a = 100;
NSNumber* number = @(a+1);
// 'number' contains 101

#Data
###To load a text file into an NSDataobject, you can do the following:
// Assuming that there is a text file at /Examples/Test.txt:
NSString* filePath = @"/Examples/Test.txt";
NSData* loadedData = [NSData dataWithContentsOfFile:filePath];
```

##转换成NSString对象
>For example, to convert an  NSDataobject to an  NSString, you can use the  NSString class’s  initWithData:encoding:method, which takes an  NSData  object and  an NSStringEncodingvalue (which indicates to the class how it should interpret the bytes).

``` objective-c
NSString* loadedString = [[NSString alloc] initWithData:loadedData 	encoding:NSUTF8StringEncoding];

//You can write an  NSDataobject to disk in a similar way
// Here, loadedData is an NSData object
NSString* filePath = @"/Examples/Test.txt";
[loadedData writeToFile:filePath atomically:YES];
```

###The NSCodingprotocol contains two key methods, which your class must implement in order to be serializable:
* encodeWithCoder:
* initWithCoder:

###Here is an example of an implementation of the encodeWithCoder:method:
``` objective-c
- (void) encodeWithCoder:(NSKeyedArchiver*)aCoder {
// Store a string (or any other Objective-C object that supports coding)
[aCoder encodeObject:myStringVariable forKey:@"myString"];
// Store a number
[aCoder encodeInteger:myIntegerVariable forKey:@"anInteger"];
}
```
###Here is the corresponding initWithCoder:  method, which sets up the object and loads the encoded data:
``` objective-c
- (id) initWithCoder:(NSKeyedUnarchiver*)aDecoder {
self = [super init];
myStringVariable = [aDecoder decodeObjectForKey:@"myString"];
myIntegerVariable = [aDecoder decodeIntegerForKey:@"anInteger"];
return self;
}
```

####To actually convert an object to a usable NSData, you can do this:
``` objective-c
// myObject is an object that 
//conforms to NSCoding
NSData* object storedData = [NSKeyedArchiver archivedDataWithRootObject:myObject];
// storedData can now be written to a file
To load it back, you can do this:
// loadedData is an NSData loaded from somewhere, and SomeObject is 
//a class that conforms to NSCoding
SomeObject* myObject = [NSKeyedUnarchiver unarchiveObjectWithData:loadedData];
```
#Design Patterns in Cocoa
>Delegationis Cocoa’s term for passing off some responsibilities of an object to another.An example of this in action is the UIApplicationobject, which represents an application on iOS. This application needs to know what should happen when the application moves to the background. Many other languages handle this problem by subclassing

{% img /images/img/ocnote2.png %}
