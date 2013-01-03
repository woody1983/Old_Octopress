---
layout: post
title: "Holiday Notes with Rails3"
date: 2013-01-03 22:11
comments: true
categories: rails
---

####假期没有出去玩 在家里宅了十几天的时间，基本消耗在Rails和德州扑克上多一些。现在开始整理笔记了，之前写的太乱。

<!-- more -->

#Rails 重新开始

>这个是在CodeSchool上学习的Rails开发教程的笔记。纪录下来，认真回过头好好复习一下。

#Install Rails
rails new TwitterForZombies

## Rails Commands
* generate
* console
* server
* dbconsole

## Using Generators
* helper
* mailer
* migration
* model
* scaffold

## Using Scaffold
`rails g scaffold NAME [field:type field:type]`
## Variable types
* string
* text
* integer
* boolean
* decimal
* float
* binary
* date
* time
* datetime

#DB Migration 
##Create table in Migration
```
create_table :tablename do |t|
  t.数据类型 :column_name
  t.text :bio
  t.index :name #add index with db
  t.timestamps# same as t.datetime :created_at t.datetime :updated_at
end
```
## Adding Columns
* AddColumn`Add<Column_name>To<table_name>`
* RemoveColumn `Remove<>From<>`
* add_index :table, :column
* rename_column :table_name, :old, :new
* rename_table :old_table, :new_table
* drop_table :table_name
* change_column :table, :column, :type, :options
* change_column_default :table, :column, default: true/'string'

`rails g migration Drop__Table`

``` ruby
def change
  add_column :zombies, :email, :string, limit: 30,unique: true
end
```

# Migration options 
* default: <value>
* limit: 30
* null: false
* first: true
* after: email
* unique: true

## Run Migrations
* rake db:migrate
* rake db:rollback
* rake db:schema:dump #->￼db/schema.rb
* rake db:setup

#Ruby 1.9 Hash New Syntax
`{name: "hash new stntax"}`
`{render json: @zombies }`

#REST {Link_to:}
### to link to each action

``` ruby
zombie = Zombie.find(2)
link_to 'show', zombie
link_to 'create', zombie, method: :post
link_to 'edit', zombie, method: :put
link_to 'destroy', zombie, method: :delete
```

##resources :zombies

``` ruby
<%= link_to 'All Zombies',  zombie_path %>
<%= link_to 'New Zombies',  new_zombie_path %>
<%= link_to 'Edit Zombies', edit_zombie_path(@zombie) %>
<%= link_to 'Show Zombies', zombie_path(@zombie) %>
<%= link_to 'Show Zombies', @zombie %>
```

#Nested Routes
>嵌套路由 感觉平时项目里会经常用到 毕竟两个数据模型并行计算和处理的时候实在是太多了。

标准写法是：将routes写成嵌套写法。

``` ruby
resources :zombie do
  resources :tweets
end
```

嵌套路由写好以后 还修改crontroller & viewer{link_fo form_for}

## 标准的嵌套url是 /zombies/4/tweets/2 /zombies/4/tweets
那么在tweets的crontroller里 会接受到zombie和tweet两个id传参进来。

先在tweets's crontroller中定义一个filter 来获取上一级信息 比如zombie
`before_filter :get_zombie`

``` ruby
def get_zombie
  @zombie = Zombie.find(params[:zombie_id])
end

zombie 已经查出来了 show和index可以直接调用

def show
  @tweet = @zombie.tweets.find(params[:id])
end

def index
  @tweets = @zombie.tweets
end
```

##在Nested rake routes的link写法

``` ruby
link_to "#{@zombie.name}'s Tweets", zombie_tweets_path(@zombie)

link_to 'New Tweet', new_zombie_tweet_path(@zombie)

link_to 'Edit Tweet', edit_zombie_tweet_path(@zombie,tweet)

show 有两种写法 哪个简单用哪个
link_to 'Show', zombie_path(@zombie,tweet)
link_to 'Show',[@zombie,tweet]

link_to 'Destroy',[@zombie,tweet],method: :delete
```

##嵌套的显示Viewer
在二级目录里的index显示 需要使用到上一级资源的调用

``` ruby
 @tweets.each do |tweets|
   tweet,body
   link_to 'Show',[@zombie,tweet]
   link_to 'Edit',edit_zombie_tweet_path[@zombie,tweet]
   link_to 'Edstroy',[@zombie,tweet], method: :delete
 end
 
 link_to 'New Tweet',new_zombie_tweet_path(@zombie)
```

###创建_form 表单比较简单
`form_for([@zombie,@tweet]) do |f|`

###嵌套路由里 Controller中就不能直接些二级目录数据实体

``` ruby
def create
  @tweet = @zombie.tweets.new(params[:tweet])

返回信息的时候要将2级目录的信息一起返回。

respond_to do |format|
  if @tweet.save
    format.html {redirect_to [@zombie,@tweet],
                 notice: 'Tweet was successfully created.'}
     format.json { render json: [@zombie,@tweet],
                          status: :created,
                          location: [@zombie,@tweet] }
   else
     format.html { render action: "new" }
     format.json { render json: @tweet.errors,
                                status: :unprocessable_entity }
   end
end

end

```

#Asset Pipeline
所有的tag 不需要指明路径 因为所有的资源调用和显示都会指向到`/assets/`

* <%= javascript_include_tag "custom" %>
* <%= stylesheet_link_tag "style" %>
* <%= image_tag "rails.png" %>

在css中调用一些图片的路径时 需要使用asset_path 否则无法使用缓存里的文件

``` ruby
form_new_zombie input.submit {
  background-image: url(<%= asset_path('button.png') %>)
}
```

#SCSS 
`zombie.css.scss.erb` 嵌套写法的css

``` ruby
form.new_zombie {
  border: 1px dashed gray;
  
  .field#bio {
    display: none;
  }
  input.submit {
    background-image: url(<%= asset_path('button.png') %>);
  }
}
```