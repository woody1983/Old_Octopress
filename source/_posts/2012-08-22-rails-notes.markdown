---
layout: post
title: "Rails学习全记录"
date: 2012-08-22 00:10
comments: true
categories: Rails
---

###创建rails项目 跳过unit-test模块
`rails new sample_app --skip-test-unit`

###不安装production环境所需要的gem
`bundle install --without production`

###创建rspec测试模块
`rails generate rspec:install`

###创建一个负责静态页面的控制器
`rails generate controller StaticPages home help --no-test-framework`

其实还可以添加 about contact等方法

删除一个控制器`rails destroy  controller FooBars baz quux`
<!-- more -->
### route中的get方法
`config/routes.rb`

by using get we arrange for the route to respond to a GET request
`get "static_pages/about"`

####The first step is to generate an integration test (request spec) for our static pages:
``` 
$ rails generate integration_test static_pages
      invoke  rspec
      create    spec/requests/static_pages_spec.rb
```

#About RSpec

```
describe "场景/描述“ do
  it "..." do
    ...
  end
end
```

#### 一段it语句
```
it "should have the content 'Sample App'" do #应该可以看到Sample App这个字样
      visit '/static_pages/home' #访问这个path
      page.should have_content('Sample App') #页面上(page) 应该会包含这个content
    end
```

# should and should_not 一招鲜

uses the `Capybara` function `visit `

`spec/requests/static_pages_spec.rb` 可以将不同的静态页面定义到不同的describe中去测试 测试指定页面中是否包含什么文字

###再添加一个静态页面呢？
* 首先route中需要添加新的`get "static_pages/about"`
* controllers指定def
* views中有对应的`html`页面`app/views/static_pages/about.html.erb`

####选择测试某一个元素 比如 title h1 等
```
page.should have_selector('title',
                    :text => "Ruby on Rails Tutorial Sample App | Home")
```
This uses the have_selector method, which checks for an HTML element (the “selector”) with the given content.
要查找的内容也可以用缩写 只寻找`关键字`
`page.should have_selector('title', :text => " | Home")`

###Provide方法 可以提供临时变量给一个label
`<% provide(:title, 'Home') %>`
#####使用
`<title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>`

>http://ruby.railstutorial.org/chapters/static-pages#top 明天继续

