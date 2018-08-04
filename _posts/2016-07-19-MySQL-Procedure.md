---
layout: post
title:  "MySQL-存储过程"
date:   2016-07-19 07
categories: 数据库
tags:  MySQL 数据库
author: i.itest.ren
---

* content
{:toc}





## 存储过程简介 ##

SQL执行过程：

	SQL命令 -> MySQL引擎 ->分析语法是否正确 -> 可识别命令 -> 执行 -> 执行结果 -> 返回客户端

存储过程： 存储过程是SQL语句和控制语句的预编译集合，以一个名称存储并作为一个单元处理。

存储过程的优点：

- 增强SQL语句的功能和灵活性
- 实现较快的执行速度
- 减少网络流量

存储过程第一次执行是和普通SQL语句一样要编译，但编译后会保存在内存中，以后再次执行存储过程就不用再次编译，速度快很多。

## 存储过程语法结果解析 ##

### 创建存储过程语法 ###

	CREATE [DEFINER = {user | CURRENT_USER}] PROCEDURE sp_name ([proc_parameter[,...]]) [characteristic ...] routine_body
	
	proc_parameter: [IN | OUT | INOUT] param_name type

### 参数 ###

- IN: 表示该参数的值必须在调用存储过程时指定
- OUT: 表示该参数的值可以被存储过程改变，并且可以返回
- INOUT: 表示该参数的调用时指定，并且可以被改变和返回

### 特性 ###

	COMMENT 'string' | {CONTAINS SQL | NO SQL | READS SQL DATA | MODIFY | SQL DATA | SQL SECURITY {DEFINER | INVOKER}}

- `COMMENT`: 注释
- `CONTAINS SQL`: 包含SQL语句，但不包含读或写数据的语句
- `NO SQL`: 不包含SQL语句
- `READS SQL DATA`: 包含读数据的语句
- `MODIFIES SQL DATA`: 包含写数据的语句
- `SQL SECURITY {DEFINER | INVOKER}` 指明谁有权限来执行

### 过程体 ###

过程体由合法的SQL语句构成；
过程体可以是任意SQL语句；
过程体如果为复合结构则使用BEGIN...END语句；
复合结构可以包含声明，循环，控制结构；

### 调用存储过程 ###

- CALL sp_name([parameter[,...]])
- CALL sp_name[()]


### 修改存储过程 ###

	ALTER PROCEDURE sp_name [characteristic ...] COMMENT 'string' | {CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA} | SQL SECURITY {DEFINER | INVOKER}

### 删除存储过程 ###

	DROP PROCEDURE [IF EXISTS] sp_name

## 创建不带参数的存储过程 ##

	mysql> CREATE PROCEDURE sp1() SELECT VERSION();
	Query OK, 0 rows affected (0.00 sec)
	
	mysql> CALL sp1;
	+-----------+
	| VERSION() |
	+-----------+
	| 5.7.13    |
	+-----------+
	1 row in set (0.00 sec)
	
	Query OK, 0 rows affected (0.01 sec)
	
	mysql> CALL sp1();
	+-----------+
	| VERSION() |
	+-----------+
	| 5.7.13    |
	+-----------+
	1 row in set (0.00 sec)
	
	Query OK, 0 rows affected (0.00 sec)

## 创建带有IN类型参数的存储过程 ##

	mysql> DELIMITER //
	mysql> CREATE PROCEDURE removeUserById(IN id INT UNSIGNED)
	    -> BEGIN
	    -> DELETE FROM users WHERE id = id;
	    -> END
	    -> //
	Query OK, 0 rows affected (0.00 sec)
	
	mysql> DELIMITER ;
	mysql> CALL removeUserById(3);
	Query OK, 7 rows affected (0.08 sec)
	
	mysql> SELECT * FROM users;
	Empty set (0.00 sec)

上面的id不规范，导致记录全删。

重新插入数据到这个表中，重新建立存储过程。

	mysql> DELIMITER //
	mysql> CREATE PROCEDURE removeUserById(IN p_id INT UNSIGNED)
	    -> BEGIN
	    -> DELETE FROM users WHERE id = p_id;
	    -> END
	    -> //
	mysql> DELIMITER ;
	mysql> SELECT * FROM users;
	+----+----------+----------+-----+------+
	| id | username | password | age | sex  |
	+----+----------+----------+-----+------+
	|  9 | a        | 123      |  10 | NULL |
	| 10 | b        | 123      |  10 | NULL |
	| 11 | c        | 123      |  10 | NULL |
	| 12 | d        | 123      |  10 | NULL |
	| 13 | e        | 123      |  10 | NULL |
	| 14 | f        | 123      |  10 | NULL |
	| 15 | g        | 123      |  10 | NULL |
	| 16 | h        | 123      |  10 | NULL |
	| 17 | i        | 123      |  10 | NULL |
	+----+----------+----------+-----+------+
	9 rows in set (0.00 sec)
	
	mysql> CALL removeUserById(12);
	Query OK, 1 row affected (0.50 sec)
	
	mysql> SELECT * FROM users;
	+----+----------+----------+-----+------+
	| id | username | password | age | sex  |
	+----+----------+----------+-----+------+
	|  9 | a        | 123      |  10 | NULL |
	| 10 | b        | 123      |  10 | NULL |
	| 11 | c        | 123      |  10 | NULL |
	| 13 | e        | 123      |  10 | NULL |
	| 14 | f        | 123      |  10 | NULL |
	| 15 | g        | 123      |  10 | NULL |
	| 16 | h        | 123      |  10 | NULL |
	| 17 | i        | 123      |  10 | NULL |
	+----+----------+----------+-----+------+
	8 rows in set (0.00 sec)

成功创建一条删除记录的存储过程，并执行成功。

## 创建带有IN和OUT类型参数的存储过程 ##

	mysql> DELIMITER //
	mysql> CREATE PROCEDURE removeUserAndReturnUserNums(IN p_id INT UNSIGNED, OUT userNums INT UNSIGNED)
	    -> BEGIN
	    -> DELETE FROM users WHERE id = p_id;
	    -> SELECT count(id) FROM users INTO userNums;
	    -> END
	    -> //
	Query OK, 0 rows affected (0.02 sec)

	mysql> DELIMITER ;
	mysql> SELECT COUNT(id) FROM users;
	+-----------+
	| COUNT(id) |
	+-----------+
	|         8 |
	+-----------+
	1 row in set (0.04 sec)
	
	mysql> CALL removeUserAndReturnUserNums(14, @nums);
	Query OK, 1 row affected (0.01 sec)
	
	mysql> SELECT @nums;
	+-------+
	| @nums |
	+-------+
	|     7 |
	+-------+
	1 row in set (0.00 sec)


在存储过程中声明的变量为局部变量，在当前数据库下用`SET`声明的变量为用户变量。


## 创建带有多个OUT类型参数的存储过程 ##

`ROW_COUNT()`可以查看数据表受到影响的行数。

	mysql> UPDATE users SET username = CONCAT(username, '--imooc') WHERE id <= 10;
	Query OK, 2 rows affected (0.12 sec)
	Rows matched: 2  Changed: 2  Warnings: 0
	
	mysql> SELECT ROW_COUNT();
	+-------------+
	| ROW_COUNT() |
	+-------------+
	|           2 |
	+-------------+
	1 row in set (0.00 sec)

删除年龄为10的所有记录，并把删除的记录个数保存到变量，把剩余记录的个数保存到另一个变量：

	mysql> DELIMITER //
	mysql> CREATE PROCEDURE removeUserByAgeAndReturnInfos(IN P_AGE SMALLINT UNSIGNED, OUT deleteUser SMALLINT UNSIGNED, OUT userCounts SMALLINT UNSIGNED)
	    -> BEGIN
	    -> DELETE FROM users WHERE age = p_age;
	    -> SELECT ROW_COUNT() INTO deleteuser;
	    -> SELECT COUNT(id) FROM users INTO userCounts;
	    -> END
	    -> //
	Query OK, 0 rows affected (0.00 sec)
	
	mysql> DELIMITER ;
	mysql> SELECT * FROM users;
	+----+----------+----------+-----+------+
	| id | username | password | age | sex  |
	+----+----------+----------+-----+------+
	|  9 | a--imooc | 123      |  10 | NULL |
	| 10 | b--imooc | 123      |  10 | NULL |
	| 11 | c        | 123      |  10 | NULL |
	| 13 | e        | 123      |  10 | NULL |
	| 15 | g        | 123      |  10 | NULL |
	| 16 | h        | 123      |  10 | NULL |
	| 17 | i        | 123      |  10 | NULL |
	+----+----------+----------+-----+------+
	7 rows in set (0.00 sec)
	
	mysql> CALL  removeUserByAgeAndReturnInfos(10, @a, @b);
	Query OK, 1 row affected (0.01 sec)
	
	mysql> SELECT @a, @b;
	+------+------+
	| @a   | @b   |
	+------+------+
	|    7 |    0 |
	+------+------+
	1 row in set (0.00 sec)

## 存储过程与自定义函数的区别 ##

- 存储过程实现的功能要复杂一些；而函数的针对性更强；
- 存储过程可以返回多个值；函数只能有一个返回值；
- 存储过程一般独立的来执行；而函数可以作为其他SQL语句的组成部分来出现；

