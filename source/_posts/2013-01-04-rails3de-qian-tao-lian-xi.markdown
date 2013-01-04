---
layout: post
title: "Rails3的嵌套练习"
date: 2013-01-04 14:41
comments: true
categories: rails
---

### 假期一直想把嵌套模型的东西整理出来，因为用到的时候经常搞错。其实最重要的地方就是Routes和Path的变化。其余都还好，趁着下午比较无聊，开会之前做一个Demo，跑了一下还OK。

<!-- more -->

#####创建2个Model

``` ruby
rails g scaffold post memo:string category:string
rails g scaffold comment content:string post_id:integer
```

###修改Data Migration的内容

``` ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :memo,limit: 1000
      t.string :category, limit:20, default: 'tweet'

      t.timestamps
    end
  end
end


class CreateComments < ActiveRecord::Migration
  def change
    create_table :comments do |t|
      t.string :content,limit: 100
      t.integer :post_id

      t.timestamps
    end
  end
end
```

###修改routes

``` ruby
  resources :posts do
    resources :comments
  end
```

```
[root@mysql-test-woody ZombieTweets]# rake routes
    post_comments GET    /posts/:post_id/comments(.:format)            comments#index
                  POST   /posts/:post_id/comments(.:format)            comments#create
 new_post_comment GET    /posts/:post_id/comments/new(.:format)        comments#new
edit_post_comment GET    /posts/:post_id/comments/:id/edit(.:format)   comments#edit
     post_comment GET    /posts/:post_id/comments/:id(.:format)        comments#show
                  PUT    /posts/:post_id/comments/:id(.:format)        comments#update
                  DELETE /posts/:post_id/comments/:id(.:format)        comments#destroy
            posts GET    /posts(.:format)                              posts#index
                  POST   /posts(.:format)                              posts#create
         new_post GET    /posts/new(.:format)                          posts#new
        edit_post GET    /posts/:id/edit(.:format)                     posts#edit
             post GET    /posts/:id(.:format)                          posts#show
                  PUT    /posts/:id(.:format)                          posts#update
                  DELETE /posts/:id(.:format)                          posts#destroy
    zombie_tweets GET    /zombies/:zombie_id/tweets(.:format)          tweets#index
                  POST   /zombies/:zombie_id/tweets(.:format)          tweets#create
 new_zombie_tweet GET    /zombies/:zombie_id/tweets/new(.:format)      tweets#new
edit_zombie_tweet GET    /zombies/:zombie_id/tweets/:id/edit(.:format) tweets#edit
     zombie_tweet GET    /zombies/:zombie_id/tweets/:id(.:format)      tweets#show
                  PUT    /zombies/:zombie_id/tweets/:id(.:format)      tweets#update
                  DELETE /zombies/:zombie_id/tweets/:id(.:format)      tweets#destroy
          zombies GET    /zombies(.:format)                            zombies#index
                  POST   /zombies(.:format)                            zombies#create
       new_zombie GET    /zombies/new(.:format)                        zombies#new
      edit_zombie GET    /zombies/:id/edit(.:format)                   zombies#edit
           zombie GET    /zombies/:id(.:format)                        zombies#show
                  PUT    /zombies/:id(.:format)                        zombies#update
                  DELETE /zombies/:id(.:format)                        zombies#destroy
```

###修改Model

``` ruby
class Post < ActiveRecord::Base
  attr_accessible :category, :memo
  has_many :comments, dependent: :destroy
end

class Comment < ActiveRecord::Base
  attr_accessible :content, :post_is
  belongs_to :post, foreign_key: :post_id
end
```

###修改controller
####主要是修改Comments的controller 先获取上一级Post的信息

``` ruby
before_filter :get_post


def get_post
  @post = Post.find(params[:post_id])
end
				  
index || @comments = Comment.all 修改成@comments => @post.comments				  
show  || @comment = Comment.find(params[:id]) => @comment =  @post.comments.find(params[:id])
new 和 edit 不变
```

#####Create 

``` ruby
  def create
    @comment = Comment.new(params[:comment])

    respond_to do |format|
      if @comment.save
        format.html { redirect_to @comment, notice: 'Comment was successfully created.' }
        format.json { render json: @comment, status: :created, location: @comment }
      else
        format.html { render action: "new" }
        format.json { render json: @comment.errors, status: :unprocessable_entity }
      end
    end
  end

  修改
  
  def create
    @comment = @post.comments.new(params[:comment])
    respond_to do |format|
      if @comment.save
        format.html {redirect_to [@post,@comment],
                     notice: 'Comment was successfully created.'}
        format.json { render json: [@post,@comment],
                             status: :created,
                             location: [@post,@comment] }
      else
        format.html { render action: "new" }
        format.json { render json: @comment.errors,
                             status: :unprocessable_entity }
      end
    end
  end
```

#####Update

``` ruby
  def update
    @comment = Comment.find(params[:id])

    respond_to do |format|
      if @comment.update_attributes(params[:comment])
        format.html { redirect_to @comment, notice: 'Comment was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: "edit" }
        format.json { render json: @comment.errors, status: :unprocessable_entity }
      end
    end
  end

  修改
  
  def update   
    @comment = @post.comments.find(params[:id])
    respond_to do |format|
      if @comment.update_attributes(params[:comment])
        format.html { redirect_to [@post,@comment], notice: 'Comment was successfully updated.' }
        format.json { head :no_content }
      else
        format.html { render action: "edit" }
        format.json { render json: @comment.errors, status: :unprocessable_entity }
      end
    end
  end
```

#####Destroy 

``` ruby
  def destroy
    @comment = Comment.find(params[:id])
    @comment.destroy

    respond_to do |format|
      format.html { redirect_to comments_url }
      format.json { head :no_content }
    end
  end

  修改
  
  def destroy
    @comment = @post.comments.find(params[:id])
    @comment.destroy

    respond_to do |format|
      format.html { redirect_to post_comments_path(@post) }
      format.json { head :no_content }
    end
  end
```

### View 修改部分

#### post 页面
添加
``` ruby
<%= link_to 'New Comment',new_post_comment_path(@post) %>
```

#### Comments Index页面
``` ruby
    <td><%= link_to 'Show', [@post, comment] %></td>
    <td><%= link_to 'Edit', edit_post_comment_path(@post, comment) %></td>
    <td><%= link_to 'Destroy', [@post, comment], method: :delete, data: { confirm: 'Are you sure?' } %></td>
	
	<%= link_to 'New Comment',new_post_comment_path(@post) %>
```
#### Comments show 页面

``` ruby
<%= link_to 'Edit', edit_post_comment_path(@post,@comment) %> |
<%= link_to 'Back', post_comments_path(@post) %>
```
#### new

``` ruby
<%= link_to 'Back', post_comments_path(@post) %>
```

#### edit
``` ruby
<%= link_to 'Show', post_comments_path(@post,@comment) %> |
<%= link_to 'Back', post_comments_path(@post) %>
```
#### form_for
``` ruby
<%= form_for([@post, @comment]) do |f| %>
```
>将post_id的text_field删除

All Post 
``` ruby
<%= link_to 'Post',@post %>
<%= link_to 'Show', [@post, comment] %>
```
