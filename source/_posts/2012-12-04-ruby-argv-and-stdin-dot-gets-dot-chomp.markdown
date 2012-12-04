---
layout: post
title: "Ruby ARGV &amp; STDIN.gets.chomp()"
date: 2012-12-04 14:32
comments: true
categories: Ruby
---

``` ruby
ser = ARGV.first
prompt = '> '

puts "Hi #{user}, I'm the #{$0} script."
puts "I'd like to ask you a few questions."
puts "Do you like me #{user}?"
print prompt
likes = STDIN.gets.chomp()

puts "Where do you live #{user}?"
print prompt
lives = STDIN.gets.chomp()

puts "What kind of computer do you have?"
print prompt
computer = STDIN.gets.chomp()

puts <<MESSAGE
Alright, so you said #{likes} about liking me.
You live in #{lives}.  Not sure where that is.
And you have a #{computer} computer.  Nice.
MESSAGE
```

>Important: 同時必須注意的是，我們也用了 STDIN.gets 取代了 gets。這是因為如果有東西在 ARGV 裡，標準的gets 會認為將第一個參數當成檔案而嘗試從裡面讀東西。在要從使用者的輸入（如stdin）讀取資料的情況下我們必須明確地使用 STDIN.gets。
