---
layout: post
title: "MySQL 多實例練習實錄"
date: 2010-05-23 19:12
comments: true
categories: MySQL
---

####流程：
* 初始化一个库 然后-P端口号 -hIP地址进去 
* 配置权限 
* 接着用multi来实现管理。 以start和stop为主。
* @MYSQL server没有启动
* @建仓
* @修改my.cnf
* @mysqld_multi start 005
* @mysql_install_db --user=mysql --datadir=/db5

```
[root@MySQL-NDB ~]# cd /
[root@MySQL-NDB /]# mkdir db7
[root@MySQL-NDB /]# chown -R mysql.mysql db7
[root@MySQL-NDB /]# cd db7
[root@MySQL-NDB db7]# vim /etc/my.cnf 
<!-- more-->

[mysqld007]
port            = 3312
socket          = /db7/mysql.sock
pid-file        = /db7/mysqld007.pid
datadir         = /db7
server-id       = 7
log-bin         = mysql-bin
```

```
[root@MySQL-NDB db7]# mysql_install_db --user=mysql --datadir=/db7
[root@MySQL-NDB db7]# ls -la
total 20
drwxr-xr-x  4 mysql mysql 4096 May 13 05:14 .
drwxr-xr-x 27 root  root  4096 May 13 05:12 ..
drwx------  2 mysql root  4096 May 13 05:14 mysql
drwx------  2 mysql root  4096 May 13 05:14 test
[root@MySQL-NDB db7]# mysqld_multi start 007
[root@MySQL-NDB db7]# ls -la
total 11408
drwxr-xr-x  4 mysql mysql     4096 May 13 05:15 .
drwxr-xr-x 27 root  root      4096 May 13 05:12 ..
-rw-rw----  1 mysql mysql 10485760 May 13 05:15 ibdata1
-rw-rw----  1 mysql mysql  1150976 May 13 05:15 ib_logfile0
drwx------  2 mysql root      4096 May 13 05:14 mysql
-rw-rw----  1 mysql mysql        0 May 13 05:15 mysql-bin.index
-rw-rw----  1 mysql root       543 May 13 05:15 MySQL-NDB.err
drwx------  2 mysql root      4096 May 13 05:14 test

[root@MySQL-NDB db7]# netstat -tunlp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name   
tcp        0      0 0.0.0.0:3307                0.0.0.0:*                   LISTEN      2011/mysqld         
tcp        0      0 0.0.0.0:3308                0.0.0.0:*                   LISTEN      2113/mysqld         
tcp        0      0 0.0.0.0:3309                0.0.0.0:*                   LISTEN      2213/mysqld         
tcp        0      0 0.0.0.0:3310                0.0.0.0:*                   LISTEN      2606/mysqld         
tcp        0      0 0.0.0.0:3311                0.0.0.0:*                   LISTEN      2858/mysqld         
tcp        0      0 0.0.0.0:3312                0.0.0.0:*                   LISTEN      3023/mysqld         
tcp        0      0 :::22                       :::*                        LISTEN      1649/sshd   
```

```
[root@MySQL-NDB db7]# mysql -P3312 -h127.0.0.1
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.1.46-log MySQL Community Server (GPL)

Copyright (c) 2000, 2010, Oracle and/or its affiliates. All rights reserved.
This software comes with ABSOLUTELY NO WARRANTY. This is free software,
and you are welcome to modify and redistribute it under the GPL v2 license

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> \s
--------------
mysql  Ver 14.14 Distrib 5.1.46, for pc-linux-gnu (i686) using readline 5.1

Connection id:          1
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         5.1.46-log MySQL Community Server (GPL)
Protocol version:       10
Connection:             127.0.0.1 via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    latin1
Conn.  characterset:    latin1
TCP port:               3312
Uptime:                 39 sec

Threads: 1  Questions: 4  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.102
--------------
```

```
[root@MySQL-NDB db7]# ls -la
total 20556
drwxr-xr-x  4 mysql mysql     4096 May 13 05:15 .
drwxr-xr-x 27 root  root      4096 May 13 05:12 ..
-rw-rw----  1 mysql mysql 10485760 May 13 05:15 ibdata1
-rw-rw----  1 mysql mysql  5242880 May 13 05:15 ib_logfile0
-rw-rw----  1 mysql mysql  5242880 May 13 05:15 ib_logfile1
drwx------  2 mysql root      4096 May 13 05:14 mysql
-rw-rw----  1 mysql mysql      106 May 13 05:15 mysql-bin.000001
-rw-rw----  1 mysql mysql       19 May 13 05:15 mysql-bin.index
-rw-rw----  1 mysql mysql        5 May 13 05:15 mysqld006.pid
-rw-rw----  1 mysql root      1196 May 13 05:15 MySQL-NDB.err
srwxrwxrwx  1 mysql mysql        0 May 13 05:15 mysql.sock
drwx------  2 mysql root      4096 May 13 05:14 test
```

```
[root@MySQL-NDB db]# mysql_install_db --user=mysql
[root@MySQL-NDB db]# service mysql start

否则
[root@MySQL-NDB db]# service mysql start
Starting MySQL... ERROR! Manager of pid-file quit without updating file.
pid-file=/db/mysqld001.pid


GRANT SHUTDOWN ON *.*  TO 'multi_admin'@'localhost' IDENTIFIED BY 'multipass';
```

```
[mysqld_multi]
mysqld     = /usr/bin/mysqld_safe
mysqladmin = /usr/bin/mysqladmin
user       = multi_admin
password   = multipass

[mysqld002]
port            = 3307
socket          = /db/mysql.sock
pid-file        = /db/mysqld001.pid
datadir         = /db
skip-locking
key_buffer_size = 16M
max_allowed_packet = 1M
table_open_cache = 64
sort_buffer_size = 512K
net_buffer_length = 8K
read_buffer_size = 256K
read_rnd_buffer_size = 512K
myisam_sort_buffer_size = 8M
log-bin=mysql-bin
binlog_format=mixed
server-id       = 1


[mysqld003]
port            = 3308
socket          = /db2/mysql.sock
pid-file        = /db2/mysqld002.pid
datadir         = /db2
server-id       = 2
log-bin         = mysql-bin



###############
| Innodb_buffer_pool_read_requests  | 77      |
| Innodb_buffer_pool_reads               | 12      |
(Innodb_buffer_pool_read_requests - Innodb_buffer_pool_reads) / Innodb_buffer_pool_read_requests * 100%

77-12/77


###################

[mysqld008]
port            = 3318
socket          = /db8/mysql.sock
pid-file        = /db8/mysqld008.pid
datadir         = /db8
server-id       = 8
log-bin         = mysql-bin
skip_slave_start = 1

innodb_buffer_pool_size = 64M
innodb_flush_log_at_trx_commit = 2
innodb_additional_mem_pool_size = 16M
innodb_lock_wait_timeout = 30
innodb_support_xa = OFF
innodb_log_buffer_size = 32M
innodb_flush_method = O_DIRECT
innodb_log_file_size = 15M

[mysqld009]
port            = 3319
socket          = /db9/mysql.sock
pid-file        = /db9/mysqld009.pid
datadir         = /db9
server-id       = 9
log-bin         = mysql-bin
skip_slave_start = 1

replicate_ignore_table = mysql.columns_priv
replicate_ignore_table = mysql.db
replicate_ignore_table = mysql.host
replicate_ignore_table = mysql.procs_priv
replicate_ignore_table = mysql.tables_priv
replicate_ignore_table = mysql.user

innodb_buffer_pool_size = 64M
innodb_flush_log_at_trx_commit = 2
innodb_additional_mem_pool_size = 16M
innodb_lock_wait_timeout = 30
innodb_support_xa = OFF
innodb_log_buffer_size = 32M
innodb_flush_method = O_DIRECT
innodb_log_file_size = 15M
```

`grant replication slave ON *.* TO 'rep009'@'localhost' identified by '12345';`

`GRANT SHUTDOWN ON *.*  TO 'multi_admin'@'localhost' IDENTIFIED BY 'multipass';`

```
mysql-bin.000001 527

009
CHANGE MASTER TO
MASTER_HOST = '192.168.200.200',
MASTER_PORT = 3318,
MASTER_USER = 'rep009',
MASTER_PASSWORD = '12345',
MASTER_LOG_FILE = 'mysql-bin.000001',
MASTER_LOG_POS = 743
```;
