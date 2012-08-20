---
layout: post
title: "关于rspec浅见"
date: 2012-08-20 20:52
comments: true
categories:  [RSpec,Rails]
---

###刚入手没几天，感觉开发过程中多了很多乐趣。

`should` and `should_not`

##before and after

before(:each) 每段it之前执行
before(:all) 每段describe 之前执行
after(:each) 
after(:all)
	
###pending 可以先列出来打算要写的代码
``` ruby 
it "…"
  pending
end
```
<!-- more-->
###let 语法 有需要才会出来运算。
``` ruby 
let(:user) {User.new(:is_vip => true)
```
###should 后面
be_true
be_false
be_nil

###检查class和def
* target.should be_a_kind_of(Array)      `target.class.should == Array`
* target.should be_an_instance_of(Array) `target.class.should == Array`
* target.should respond_to(:foo)         `target.respond_to?(:foo).should == true`

###检查array hash
* target.should have_key(:foo) `#target[:foo].should_not == nil`
* target.should include(4)     `#target.include?(4).should == true`
* target.should have(3).items  `#target.items.length == 3`

###任何be_开头的
* target.should be_empty `#target.empty?.should == true`
* target.should be_blank `#target.blank?.should == true`
* target.should be_admin `#target.admin?.should == true`

###should ==   一招鲜吃遍天

###expect to
####希望一段代码会改变某个值货丢出例外

``` ruby 
it "should update status to shipping" do
  expect{order.ship!}.to change{order.status}.from("new").to("shippping")
end
```
####希望status从new变成shipping
和这段差不多

``` ruby 
it "should update status to shipping "do
order.status.should == "new"
order.ship!
order.status.should == "shipping"
end
```

###subject 可以省略 receiver
``` ruby 
subject {Order.new(:status => "New")}
it {should be_valid} #subject.valid?should == true
its(:status){should == "New"} #subject.status.should == "New"
```

###it example specify 是一样的 都是别名

#### rspec filename.rb -fs 输出specdox文件
#### rspec filename.rb -fh 输出html文件

### useful Rspec tools
* factory_girl 造测试数据
* shoulda 更多rails专用的matcher
* capybara 提供浏览器模拟

###rcov 测试覆盖度 看有哪些代码没有测试到

###Capydara
* fill_in 'Login', :with=>'username'
* click_link 'Sign up'

###每个it只有一个测试目的。不被周边因素影响
