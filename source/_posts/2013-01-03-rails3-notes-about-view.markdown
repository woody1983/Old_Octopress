---
layout: post
title: "Rails3 Notes about view"
date: 2013-01-03 22:19
comments: true
categories: rails
---

#### Rails3中View的Helper提供了很多实用的方法。

<!-- more -->
#Viewer
##Create a form
``` ruby
form_for(@zombie) do |f|

end
```

如果`@zombie`已经存在数据库里了

``` ruby
￼<form action="/zombies/8" method="post">
  <input name="_method" type="hidden" value="put" />
```
否则就是post

``` ruby
<form action="/zombies" method="post">
```

`f.submit`也是根据Zombie数据对象再数据库是否存在而选择显示Create 还是Update

#常见的Text_field
`f.text_field :name`提交出去的时候数据模型是这样子的

`:params => {:zombie =>{:name => 'Name'}}`

#Label helper
`f.label :name` 在html里显示`<label for="zombie_name">Name</label>`

#input helpers
* text_area
* check_box
* radio_button :decomp, 'fresh', checked: true
* select :decomp, ['frsh','new']
* select :decomp, [['fresh,1],['new',2]]
* password_field
* number_field
* range_field
* email_field
* url_field
* telephone_field

##Partials 减少重复代码 比如新建和更新的表单

<% render 'form'%> `_form.html.erb`

#View helper
## div_for
之前的`<div id="tweet_<%= tweet.id%>" class='tweet'>`可以写成
<%= div_for tweet do %>
`dom_id(@tweet)` => tweet_2

##截断字符串
`truncate("I need brains!", :length => 10,:separator => ' ')`
:separator是为了保证不会切半个单词什么的。

##自动显示数量词单复数形式
`pluralize(Zombie.count,"zombie")`

##String的标题化 //首字母自动大小写之类的设置 很使用
`@zombie.name.titleize`	

##队列化 a , b and c
`@role.name.to_sentence`

## 时间间隔表达
`time_ago_in_words @zombie.created_at`

##Number的转换
* number_to_currency
* number_to_human 

