---
layout: post
title: "Ruby Moudle &amp; Class &amp; Method"
date: 2012-12-11 15:17
comments: true
categories: Ruby
--- 

####不能创建实例和继承 作用之一是提供namespace 通过使用`Moudle.method`的方式调用 可以避免方法 常数和类名称的冲突。

include一个module以后 可以直接调用里面的常数和方法 一般调用方法为
<!-- more -->
``` ruby
module HelloWorld
  Version = "1.0"

  def hello(name)
    print "Hi #{name}. \n"
  end
  #如果该method需要对外部公开的话
  module_function :hello
end
```

调用的时候就是 `HelloWorld::Verison ` or `HelloWorld.hello('1024bit')`


> Mix-in 讲moduld混到Class中 可以将两个Class中相同的功能植入Module中

``` ruby
module MyModule
  #
end

class MyClass1
  include MyModule
end

class MyClass2
  include MyModule
end
```

## Class

* 初始化方法 initialize
* 访问方法 `attr_reader :name` & `attr_writer` & `attr_accessor`
* 类变量`@@`是所有实例共享的变量
* 类方法 不能对实例操作。或者说实例无法调用类方法

类方法的声明三种写法

``` ruby
class HelloWorld
  def HelloWorld.hello(name)
    #
  end
end
#=>

class HelloWorld
  #
end

class << HelloWorld
  def hello
  end
end
#=>

class HelloWorld
  def self.hello
  end
end
```

## Method

如果Method的参数过多 建议放到Hash里去处理

``` ruby
def hello(name="Ruby",option={})
  long = options[:long]
  body = message
end
```
