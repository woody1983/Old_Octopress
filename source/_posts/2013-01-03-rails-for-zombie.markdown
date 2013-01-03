---
layout: post
title: "Rails For Zombie"
date: 2013-01-03 21:58
comments: true
categories: rails
---

###这是国外的一个在线教育平台的课程 感觉非常不错 有很多知识点。现在共享出来，一起学习。

<!-- more -->

####Rails new
>Enter the command to start a new rails app called 'ZombieTweets'

rails new Zombi­eTweets 
####GENERATE SCAFFOLD
>Enter the command to create tweet scaffold with a string status and an integer of zombie_id

rails g scaff­old tweet­ statu­s:string zombi­e_id:integ­er
####RUN MIGRATION
>Enter the command to run the migration you just created.

```
$ rake db:mi­grate
==  CreateTweets: migrating ===================================================
-- create_table(:tweets)
   -> 0.0017s
==  CreateTweets: migrated (0.0025s) ==========================
```
####BOOT IT UP
>Enter the command to start a rails server

```
rails s
=> Booting WEBrick
=> Rails 3.1.0 application starting in development on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2013-01-03 12-26-14] INFO  WEBrick 1.3.1
[2013-01-03 12-26-14] INFO  ruby 1.9.2 (2011-02-18) [x86_64-darwin10.7.0]
[2013-01-03 12-26-14] INFO  WEBrick::HTTPServer#start: pid=35740 port=3000
```

####ADD COLUMN TO TABLE
>Enter the command line text to generate a migration called AddPrivateToTweets which adds a boolean field called private.

```
$ rails g migra­tion AddPr­ivateToTwe­ets priva­te:boolean­
invoke  active_record
create    db/migrate/20130103122847_add_private_to_tweets.rb
```

####ROLLBACK MIGRATION
>Assume we ran that last migration. However we forgot a column we wanted to add, could you please roll it back from the command line?

```
$ rake db:ro­llback
==  AddLocationToTweets: reverting ============================================
-- remove_column("tweets", :show_location)
   -> 0.0008s
-- remove_column("tweets", :location)
   -> 0.0003s
==  AddLocationToTweets: reverted (0.0012s) ===================================
```

####REMOVE COLUMN
>On second thought, that category_name string column was a bad idea. Write a migration to remove the category_name column.

``` ruby
class RemoveCategoryNameFromTweets < ActiveRecord::Migration
  def up
    remove_column :tweets , :category_name
  end

  def down
    add_column :tweets,:category_name , :string
  end
end
```

####DATABASE SETUP
>You've decided to install the app on another computer. Please enter the command you should use (instead of rake db:migrate) to create the database, load the schema, and run the seed file.

rake db:setup

####IN THE CONSOLE
>Enter the command to enter the Rails console

```
$ rails c
Loading development environment (Rails 3.1.0)
```

>Enter the command to enter the Rails console.
You are now running in the Rails console!
Experiment with creating a new Tweet and saving it to the database. Type next to move on.

```
ruby > Tweet.crea­te(message­: 'hell­o', categ­ory_name: 'cate­gory', locat­ion: 'Chin­a', show_­location: true,­ zombi­e_id: 1)
#<Tweet id: 1, message: "hello", category_name: "category", location: "China", show_location: true, zombie_id: 1>
ruby >  
```

schema:

``` ruby
create_table(:tweets) do |t|
  t.string :message
  t.string :category_name
  t.string :location, :limit => 30
  t.boolean :show_location, :default => false
  t.integer :zombie_id
end
```

####SCOPES 
>Write a scope on the Tweet model called recent which returns the 4 most recent tweets. Hint: You'll need an order AND a limit scope.

``` ruby
class Tweet < ActiveRecord::Base
  scope :recent , order("created_at desc").limit(4)
end
```

>Write another scope called graveyard which only shows tweets where the show_location column is true and the location is "graveyard"

``` ruby
class Tweet < ActiveRecord::Base
  scope :recent, order('created_at desc').limit(4) 
  scope :graveyard, where(show_location: true,location: 'graveyard')
end
```

####USING SCOPES
>In this controller action create an instance variable called @graveyard_tweets which uses both of the two scopes recent and graveyard together.

``` ruby
class TweetsController < ApplicationController
  def index
    @tweets = Tweet.all
    @graveyard_tweets = Tweet.recent.graveyard
  end
end
```

####BEFORE SAVE
>Create a before_save callback that checks to see if a tweet has a location, if it does have a location then set show_location to true.
Tip: You can check to see if location exists with if self.location?

``` ruby
class Tweet < ActiveRecord::Base
  before_save :tweet
  
  
  def tweet
    self.show_location = true if self.location
  end
end
```

####CALLBACK LOGGING
>Add callbacks so the appropriate log function is called after an update and destroy.

``` ruby
class Tweet < ActiveRecord::Base

after_update :log_update
after_destroy :log_destroy

  def log_update
    logger.info "Tweet #{id} updated"
  end

  def log_destroy
    logger.info "Tweet #{id} deleted"
  end
end
```

####HAS ONE
>Instead of storing location inside the Tweet model, let's instead break it out into a separate table (as you see below). In this case we want to define that a Tweet can have one Location, and a Location belongs to a Tweet. Fill out the models below accordingly.

``` ruby
class Tweet < ActiveRecord::Base
has_one :location
end

class Location < ActiveRecord::Base
belongs_to :tweet
end
```

####FOREIGN KEY
>OH NO! Our Database Admin turned into a Zombie and decided to rename the belongs_to field in our locations table tweeter_id instead of the intelligent default tweet_id. We're going to slay him and correct this, but in the meantime set the foreign_key on both relationships to tweeter_id. Also set the dependency so when a tweet is destroyed, the location is destroyed as well.

``` ruby
class Tweet < ActiveRecord::Base
  has_one :location, dependent: :destroy, foreign_key: :tweeter_id
end

class Location < ActiveRecord::Base
  belongs_to :tweet, foreign_key: :tweeter_id
end
```

####INCLUDES
>We're going to be iterating through many tweets and printing out their location. Refactor the controller code below to use the includes method.

``` ruby
class TweetsController < ApplicationController 
  def index
    @tweets = Tweet.includes(:location).recent
  end
end
```

####RELATIONSHIP MIGRATION
>A Tweet can belong to one or more Categories (e.g. eating flesh, walking dead, searching for brains). Write a migration that creates two tables, categories, and categorizations. Give categories one column named name of type string; and give categorizations two integer columns: tweet_id and category_id.

``` ruby
class AddTweetCategories < ActiveRecord::Migration
  def change
    create_table :categories do |t|
      t.string :name
    end
    
    create_table :categorizations do |t|
      t.integer :tweet_id
      t.integer :category_id
    end
  end 
end
```

####HAS MANY THROUGH
>Now that we have our new tables, it's time to define the relationships between each of the models. Define the has_many through relationships in the Tweet & Category model and the belongs_to relationships in the Categorization model.

``` ruby
class Tweet < ActiveRecord::Base
  has_many :categorizations
  has_many :categories, through: :categorizations
end

class Categorization < ActiveRecord::Base
  belongs_to :tweet
  belongs_to :category
end

class Category < ActiveRecord::Base
  has_many :categorizations
  has_many :tweets, through: :categorizations
end
```