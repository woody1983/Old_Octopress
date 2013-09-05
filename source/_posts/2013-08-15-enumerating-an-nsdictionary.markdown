---
layout: post
title: "Enumerating an NSDictionary"
date: 2013-08-15 11:36
comments: true
categories: IOS
---

``` objective-c
NSDictionary *funnyWords = @{
@"Schadenfreude": @"pleasure derived by someone from another person's misfortune.",
@"Portmanteau": @"consisting of or combining two or more separable aspects or qualities",
@"Penultimate": @"second to the last"
};

for (NSString *word in funnyWords){
    NSString *definition = funnyWords[word];//
    NSLog(@"%@ is defined as %@", word, definition);
}
``` 

## Practice makes perfect

``` objective-c
NSDictionary *newHats = @{
@"Cowboy": @"White",
@"Conductor": @"Brown",
@"Baseball": @"Red"
};

for (NSString *hat in newHats){
    
    NSString *color = newHats[hat];
    
    NSLog(@"Trying on new %@ %@ hat", color, hat);
    
    if([mrHiggie tryOnHat:hat withColor:color]) {
        NSLog(@"Mr. Higgie loves it");
    } else {
        NSLog(@"Mr. Higgie hates it");
    }
}
``` 