---
layout: post
title: "MySQL Integration Redis"
date: 2014-04-16 23:01
comments: true
categories: MySQL
---

在把一个大表从 MySQL 迁移到 Redis 时，你可能会发现，每次提取、转换、导入一条数据是让人难以忍受的慢!这里有一个技巧，你可以通过使用管道把 MySQL 的输出直接输入到 redis-cli输入端，这可以使两个数据库都能以他们的最顶级速度来运行。

　　使用了这个技术，我把 800 万条 MySQL 数据导入到 Redis 的时间从 90 分钟缩短到了两分钟。

　　Mysql到Redis的数据协议

　　redis-cli命令行工具有一个批量插入模式，是专门为批量执行命令设计的。这第一步就是把Mysql查询的内容格式化成redis-cli可用的数据格式。here we go!

　　我的统计表：

``` sql
　　CREATE TABLE events_all_time 
		( id int(11) unsigned NOT NULL AUTO_INCREMENT, 
		  action varchar(255) NOT NULL, 
		  count int(11) NOT NULL DEFAULT 0, 
		  PRIMARY KEY (id), UNIQUE KEY uniq_action (action) );
```


　　准备在每行数据中执行的redis命令如下：

``` sql
　　HSET events_all_time [action] [count]
```

　　按照以上redis命令规则，创建一个events_to_redis.sql文件，内容是用来生成redis数据协议格式的SQL：

```
　　-- events_to_redis.sql SELECT CONCAT( "*4\r\n", '$', LENGTH(redis_cmd), '\r\n', redis_cmd, '\r\n', '$', LENGTH(redis_key), '\r\n', redis_key, '\r\n', '$', LENGTH(hkey), '\r\n', hkey, '\r\n', '$', LENGTH(hval), '\r\n', hval, '\r' ) FROM ( SELECT 'HSET' as redis_cmd, 'events_all_time' AS redis_key, action AS hkey, count AS hval FROM events_all_time ) AS t
```

　　ok, 用下面的命令执行：

``` 
　　mysql stats_db --skip-column-names --raw < events_to_redis.sql | redis-cli --pipe
```

　　很重要的mysql参数说明：

　　--raw: 使mysql不转换字段值中的换行符。

　　--skip-column-names: 使mysql输出的每行中不包含列名。