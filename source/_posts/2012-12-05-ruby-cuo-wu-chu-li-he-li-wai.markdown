---
layout: post
title: "Ruby 错误处理和例外"
date: 2012-12-05 11:50
comments: true
categories: Ruby
---

``` ruby
ltotal = 0
wtotal = 0
ctotal = 0

ARGV.each {|file|
  begin
    input = File.open(file)
    l = 0
    w = 0
    c = 0
    while line = input.gets
      l += 1
      c += line.size
      line.sub!(/^\s+/, "")
      ary = line.split(/\s+/).size
      w += ary.size
    end
    input.close
    printf("%8d %8d %8d %s\n",l,w,c,file)
    ltotal += 1
    wtotal += 1
    ctotal += 1
  rescue => ex
    print ">>>",ex.message,"<<<", "\n"
  end
}

printf("%8d %8d %8d %s\n",ltotal,wtotal,ctotal,"total")
```
### 当无法打开文件阅读的时候，就会将例外报给ex变量 并打印输出。
