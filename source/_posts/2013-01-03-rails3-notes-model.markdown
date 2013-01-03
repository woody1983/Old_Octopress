---
layout: post
title: "Rails3 Notes Model"
date: 2013-01-03 22:21
comments: true
categories: rails
---

#### Rails3 中和Model有关的内容我单独整理出来了。可能是做数据库的时间比较多，喜欢从数据的层面分析问题。

<!-- more -->

#Rails Model

##Names Scope

``` ruby
scope :rotting, where(rotting: true)
scope :fresh, where("age < 20")
scope :recent, order("created_at desc").limit(10)
```
####调用比较简单 直接在Crontroller中Call这些Scope

``` ruby
Zombie.rotting.limit(10)
Zombie.recent.fresh.rotting
```

##Callbacks

>应用场景比较简单，比如在save一个对象之前，根据目前这个对象的一些属性自动赋值。ex: When zombie age > 20 , set rotting to true.

``` ruby
defore_save :make_rotting


def make_rotting
  self.rotting = true if age > 20
end

```

#### All Callbacks
* before_validation
* save
* create
* update
* destroy

##model关系
>单数和复数 has_one has_many belongs_to.复数的形式和英语标准文法里一致

#Create Model 单数

`rails g model 单数 column:type`

##dependent: :destroy
has_one 

##"include" option
``` ruby
@zombie = Zombie.all #然后在view里再去拉brain的数据 
@zombie = Zombie.includes(:brain).all
#Brain Load (0.3ms) SELECT * FROM "brains" WHERE "zombie_id" IN (4, 5, 6, 7)
```

## through
>创建R表 也就是关联表。然后多对多关系的两边 同时宣告拥有多个R表 并且拥有多个第二个模型

``` ruby
has_many :assignments
has_many :roles, through: :assignments
```
through关系建立完毕以后，创建多对多数据时 可以直接向第二个对象添加 ex

``` ruby
z = Zombie.last
z.roles << Role.find_by_title('Captain')
#也可以用z.roles.push 看个人习惯
这个时候check一下role的数据会看到他会inner join关系表 调用a表的时候会使用b表的主键
z.roles
```



