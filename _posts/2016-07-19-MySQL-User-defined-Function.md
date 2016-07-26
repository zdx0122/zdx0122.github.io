---
layout: post
title:  "MySQL-自定义函数"
date:   2016-07-19 06
categories: 数据库
---





## 自定义函数简介 ##

用户自定义函数（user-defined function, UDF）是一种对MySQL扩展的途径，其用法与内置函数相同。

自定义函数的两个必要条件：

- 参数
- 返回值

函数可以返回任意类型的值，同样可以接收这些类型的参数

创建自定义函数：

	CREATE FUNCTION function_name RETURNS {STRING | INTEGER | REAL | DECIMAL} routine_body

关于函数体

1. 函数体由合法的SQL语句构成；
1. 函数体可以是简单的SELECT或INSERT语句；
1. 函数体如果为符合结构则使用BEGIN...END语句；
2. 复合结构可以包含声明，循环，控制结果；

## 创建不带参数的自定义函数 ##

	mysql> CREATE FUNCTION f1() RETURNS VARCHAR(30)
	    -> RETURN DATE_FORMAT(NOW(), '%Y年%m月%d日 %H点：%i分：%s秒');
	Query OK, 0 rows affected (0.34 sec)
	
	mysql> SELECT f1();
	+-----------------------------------------+
	| f1()                                    |
	+-----------------------------------------+
	| 2016年07月26日 11点：22分：28秒         |
	+-----------------------------------------+
	1 row in set (0.01 sec)

## 创建带有参数的的自定义函数 ##

	mysql> CREATE FUNCTION f2(num1 SMALLINT UNSIGNED, num2 SMALLINT UNSIGNED)
	    -> RETURNS FLOAT(10,2) UNSIGNED
	    -> RETURN (num1 + num2)/2;
	Query OK, 0 rows affected (0.14 sec)
	
	mysql> SELECT f2();
	ERROR 1318 (42000): Incorrect number of arguments for FUNCTION test.f2; expected 2, got 0
	mysql> SELECT f2(10, 15);
	+------------+
	| f2(10, 15) |
	+------------+
	|      12.50 |
	+------------+
	1 row in set (0.01 sec)

## 创建具有复合结构函数体的自定义函数 ##

使用`DELIMITER ;`是把分号作为sql语句的结束符；使用`DELIMITER //`是把双斜线作为sql语句的结束符。

	mysql> DELIMITER //
	
	mysql> CREATE FUNCTION adduser(username VARCHAR(10))
	    -> RETURNS INT UNSIGNED
	    -> BEGIN
	    -> INSERT users1(username) VALUES(username);
	    -> RETURN LAST_INSERT_ID();
	    -> END
	    -> //
	Query OK, 0 rows affected (0.05 sec)
	
	mysql> SELECT adduser('DeX');
	    -> //
	+----------------+
	| adduser('DeX') |
	+----------------+
	|              4 |
	+----------------+
	1 row in set (0.33 sec)
	
	mysql> DELIMITER ;
	mysql> SELECT adduser('Tom');
	+----------------+
	| adduser('Tom') |
	+----------------+
	|              5 |
	+----------------+
	1 row in set (0.02 sec)
	
	mysql> SELECT * FROM users1;
	+----+----------+------+
	| id | username | pid  |
	+----+----------+------+
	|  3 | Rose     |    2 |
	|  4 | DeX      | NULL |
	|  5 | Tom      | NULL |
	+----+----------+------+
	3 rows in set (0.00 sec)

## 删除自定义函数 ##

	DORP FUNCTION [IF EXISTS] function_name

