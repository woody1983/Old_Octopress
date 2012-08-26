---
layout: post
title: "MySQL存储过程 写着玩的"
date: 2010-05-23 19:23
comments: true
categories: MySQL
---

###uid and mid 
``` sql
DELIMITER $$

DROP PROCEDURE  IF EXISTS file_test;
CREATE PROCEDURE file_test(IN p_uid INT, IN p_mid INT, OUT p_count INT)
READS SQL DATA
BEGIN
SELECT id,uname
FROM emp
WHERE uid = p_uid
AND mid = p_mid;

SELECT FOUND_ROWS() INTO p_count;
END $$

DELIMITER ;
```
<!-- more-->
###查询 uid 大于等于多少 并且 mid小于多少
``` sql
DELIMITER $$

DROP PROCEDURE  IF EXISTS pro_or;
CREATE PROCEDURE pro_or(IN p_uid INT, IN p_mid INT, OUT p_count INT)
READS SQL DATA
BEGIN
SET @x = 10;
SELECT id,uname
FROM emp
WHERE uid >= p_uid
AND mid < p_mid;

SELECT FOUND_ROWS() INTO p_count;
END $$

DELIMITER ;
```
```
mysql> CALL pro_or(1,13,@asd);
+----+-------+
| id | uname |
+----+-------+
|  1 | aaa   |
|  2 | bbb   |
+----+-------+
2 rows in set (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> select @asd;
+------+
| @asd |
+------+
|    2 |
+------+
1 row in set (0.00 sec)

mysql> select @x;
+------+
| @x   |
+------+
|   10 |
+------+
1 row in set (0.00 sec)
```

>ceil(100*rand())
###@插入N条数据
``` sql
DELIMITER $$
DROP PROCEDURE  IF EXISTS auto_insert;
CREATE PROCEDURE auto_insert(IN a_string CHAR(12))
BEGIN
set @x = 0;
ins: LOOP
set @x = @x + 1;
IF @x = 1000 then
leave ins;
END IF;
INSERT INTO emp(uname,uid,mid) VALUES(a_string,ceil(100*rand()),ceil(100*rand()));
END LOOP ins;
END;
$$
DELIMITER ;
```

####简单的
``` sql
CREATE PROCEDURE usp1(IN P INT)
BEGIN
SET @x = p;
END

CALL usp1(12345);
select @x;
```
####输出函数

``` sql
create procedure usp2(OUT p INT, IN p2 INT)
BEGIN
SET p = p2 * 2;
END
```

```
mysql> delimiter $$
mysql> create procedure usp2(OUT p INT, IN p2 INT)
-> BEGIN
-> SET p = p2 * 2;
-> END
-> $$
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;
mysql> CALL usp2(@a,123);
Query OK, 0 rows affected (0.00 sec)

mysql> select @a;
+------+
| @a   |
+------+
|  246 |
+------+
1 row in set (0.00 sec)
```

###INOUT
``` sql
DROP PROCEDURE  IF EXISTS demosp;
delimiter $$
CREATE PROCEDURE demosp(IN inputParam VARCHAR(255), INOUT inOutParam INT)
BEGIN
SET inOutParam = 1000;
SELECT inOutParam;
SELECT CONCAT('Hello ',inputParam);
END
$$
delimiter ;
```
CALL demosp('Woody',@q);
select @q;

###互相调用
``` sql
CREATE PROCEDURE get_time()
SET @current_time = CURTIME();

CREATE PROCEDURE part_of_day()
BEGIN
CALL get_time();
IF @current_time < '12:00:00' THEN
SET @day_part = 'morning';
ELSEIF @current_time = '12:00:00' THEN
SET @day_part = 'noon';
ELSE
SET @day_part = 'afternoon or night';
END IF;
END;
```
