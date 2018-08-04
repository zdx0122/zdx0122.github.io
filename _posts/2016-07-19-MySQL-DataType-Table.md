---
layout: post
title:  "MySQL-数据类型和操作数据库表"
date:   2016-07-19 01
categories: 数据库
tags:  MySQL 数据库
author: i.itest.ren
---

* content
{:toc}




## 数据类型 ##

数据类型是指列、存储过程参数、表达式和局部变量的数据特征，它决定了数据的存储格式，代表了不同的信息类型。


## 数据类型之整型 ##

![整型](http://zdx0122.qiniudn.com/MySQL-int.jpg)

## 数据类型之浮点型 ##

![float和double对比](http://zdx0122.qiniudn.com/MySQL-float2.jpg)

## 数据类型之日期时间型 ##

![date型](http://zdx0122.qiniudn.com/MySQL-date.jpg)

## 数据类型之字符型 ##

![字符型](http://zdx0122.qiniudn.com/MySQL-string.jpg)

## 创建数据表 ##

数据表（或称表）是数据库最重要的组成成分之一，是其他对象的基础。

打开数据库命令：`USE 数据库名称;`

	mysql> USE test;
	Database changed

查看当前使用的是哪一个数据库，命令：

	mysql> SELECT DATABASE();
	+------------+
	| DATABASE() |
	+------------+
	| test       |
	+------------+
	1 row in set (0.00 sec)

创建数据表语法：

	CREATE TABLE [IF NOT EXISTS] table_name (
		column_name data_type,
		...
	)

创建`tb1`表：

	mysql> CREATE TABLE tb1(
	    -> username VARCHAR(20),
	    -> age TINYINT UNSIGNED,
	    -> salary FLOAT(8,2) UNSIGNED
	    -> );
	Query OK, 0 rows affected (0.91 sec)

## 查看数据表 ##

查看数据表列表语法：

	SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]

在当前库下，执行命令：

	mysql> SHOW TABLES;
	+----------------+
	| Tables_in_test |
	+----------------+
	| tb1            |
	+----------------+
	1 row in set (0.01 sec)

查看其他数据库下的数据表：

	mysql> SHOW TABLES FROM t2;
	Empty set (0.00 sec)

查看其他数据库下的表，并不会改变当前应用的库：

	mysql> SELECT DATABASE();
	+------------+
	| DATABASE() |
	+------------+
	| test       |
	+------------+
	1 row in set (0.00 sec)

	mysql> SHOW TABLES;
	+----------------+
	| Tables_in_test |
	+----------------+
	| tb1            |
	+----------------+
	1 row in set (0.00 sec)

## 查看数据表结构 ##

查看数据表结构语法：

	SHOW COLUMNS FROM tbl_name;

执行命令：

	mysql> SHOW COLUMNS FROM tb1;
	+----------+---------------------+------+-----+---------+-------+
	| Field    | Type                | Null | Key | Default | Extra |
	+----------+---------------------+------+-----+---------+-------+
	| username | varchar(20)         | YES  |     | NULL    |       |
	| age      | tinyint(3) unsigned | YES  |     | NULL    |       |
	| salary   | float(8,2) unsigned | YES  |     | NULL    |       |
	+----------+---------------------+------+-----+---------+-------+
	3 rows in set (0.25 sec)

## 记录的插入与查找 ##

### INSERT ###

插入记录语法：

	INSERT [INTO] tbl_name [(col_name,...)] VALUES(val,...)

执行命令：

	mysql> INSERT tb1 VALUES('Tom',25,7863.25);
	Query OK, 1 row affected (0.16 sec)

VALUES值少一个后，执行：

	mysql> INSERT tb1 VALUES('Tom',25);
	ERROR 1136 (21S01): Column count doesn't match value count at row 1

省略列名的情况下，必须输入所有的VALUES值。

只插入一个字段：

	mysql> INSERT tb1(username,salary) VALUES('John',4500.69);
	Query OK, 1 row affected (0.06 sec)

### SELECT ###

记录查找语法：

	SELECT expr,... FROM tbl_name;

执行命令：

	mysql> SELECT * FROM tb1;
	+----------+------+---------+
	| username | age  | salary  |
	+----------+------+---------+
	| Tom      |   25 | 7863.25 |
	| John     | NULL | 4500.69 |
	+----------+------+---------+
	2 rows in set (0.01 sec)

## 空值与非空 ##

- NULL, 字段值可以为空；
- NOT NULL, 字段值禁止为空；

执行命令：

	mysql> CREATE TABLE tb2(
	    -> username VARCHAR(20) NOT NULL,
	    -> age TINYINT UNSIGNED NULL
	    -> );
	Query OK, 0 rows affected (0.11 sec)
	
	mysql> SHOW COLUMNS FROM tb2;
	+----------+---------------------+------+-----+---------+-------+
	| Field    | Type                | Null | Key | Default | Extra |
	+----------+---------------------+------+-----+---------+-------+
	| username | varchar(20)         | NO   |     | NULL    |       |
	| age      | tinyint(3) unsigned | YES  |     | NULL    |       |
	+----------+---------------------+------+-----+---------+-------+
	2 rows in set (0.01 sec)

	mysql> INSERT tb2 VALUES('TOM',NULL);
	Query OK, 1 row affected (0.00 sec)
	
	mysql> SELECT * FROM tb2;
	+----------+------+
	| username | age  |
	+----------+------+
	| TOM      | NULL |
	+----------+------+
	1 row in set (0.00 sec)

如果非空项输入NULL，会报错：

	mysql> INSERT tb2 VALUES(NULL,22);
	ERROR 1048 (23000): Column 'username' cannot be null

## 自动编号 ##

AUTO_INCREMENT

- 自动编号，且必须与主键组合使用
- 默认情况下，起始值为1，每次的增量为1

		mysql> CREATE TABLE tb3(
		    -> id SMALLINT UNSIGNED AUTO_INCREMENT,
		    -> username VARCHAR(30) NOT NULL
		    -> );
		ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key

提示必须定义一个主键。

## 初涉主键约束 ##

PRIMARY KEY

- 主键约束
- 每张数据表只能存在一个主键
- 主键保证记录的唯一性
- 主键自动为NOT NULL

		mysql> CREATE TABLE tb3(
		    -> id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
		    -> username VARCHAR(30) NOT NULL
		    -> );
		Query OK, 0 rows affected (0.04 sec)
	
		mysql> SHOW COLUMNS FROM tb3;
		+----------+----------------------+------+-----+---------+----------------+
		| Field    | Type                 | Null | Key | Default | Extra          |
		+----------+----------------------+------+-----+---------+----------------+
		| id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
		| username | varchar(30)          | NO   |     | NULL    |                |
		+----------+----------------------+------+-----+---------+----------------+
		2 rows in set (0.00 sec)

依次插入一条条数据：

	mysql> INSERT tb3(username) VALUES('Tom');
	Query OK, 1 row affected (0.01 sec)
	
	mysql> INSERT tb3(username) VALUES('John');
	Query OK, 1 row affected (0.01 sec)
	
	mysql> INSERT tb3(username) VALUES('Rose');
	Query OK, 1 row affected (0.01 sec)
	
	mysql> INSERT tb3(username) VALUES('Dimitar');
	Query OK, 1 row affected (0.00 sec)

查看自增的`id`字段：

	mysql> SELECT * FROM tb3;
	+----+----------+
	| id | username |
	+----+----------+
	|  1 | Tom      |
	|  2 | John     |
	|  3 | Rose     |
	|  4 | Dimitar  |
	+----+----------+
	4 rows in set (0.00 sec)

主键的唯一性：

	mysql> CREATE TABLE tb4(
	    -> id SMALLINT UNSIGNED PRIMARY KEY,
	    -> username VARCHAR(30) NOT NULL
	    -> );
	Query OK, 0 rows affected (0.05 sec)
	
	mysql> SHOW COLUMNS FROM tb4;
	+----------+----------------------+------+-----+---------+-------+
	| Field    | Type                 | Null | Key | Default | Extra |
	+----------+----------------------+------+-----+---------+-------+
	| id       | smallint(5) unsigned | NO   | PRI | NULL    |       |
	| username | varchar(30)          | NO   |     | NULL    |       |
	+----------+----------------------+------+-----+---------+-------+
	2 rows in set (0.00 sec)
	
	mysql> INSERT tb4 VALUES(22,'Tom');
	Query OK, 1 row affected (0.01 sec)
	
	mysql> INSERT tb4 VALUES(4,'John');
	Query OK, 1 row affected (0.00 sec)
	
	mysql> SELECT * FROM tb4;
	+----+----------+
	| id | username |
	+----+----------+
	|  4 | John     |
	| 22 | Tom      |
	+----+----------+
	2 rows in set (0.00 sec)
	
	mysql> INSERT tb4 VALUES(22,'Rose');
	ERROR 1062 (23000): Duplicate entry '22' for key 'PRIMARY'

## 初涉唯一约束 ##

UNIQUE KEY

- 唯一约束
- 唯一约束可以保证记录的唯一性
- 唯一约束的字段可以为空值（NULL）
- 每张数据表可以存在多个唯一约束

		mysql> CREATE TABLE tb5
		    -> (
		    -> id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
		    -> username VARCHAR(30) NOT NULL UNIQUE KEY,
		    -> age TINYINT UNSIGNED
		    -> );
		Query OK, 0 rows affected (0.05 sec)
	
		mysql> SHOW COLUMNS FROM tb5;
		+----------+----------------------+------+-----+---------+----------------+
		| Field    | Type                 | Null | Key | Default | Extra          |
		+----------+----------------------+------+-----+---------+----------------+
		| id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
		| username | varchar(30)          | NO   | UNI | NULL    |                |
		| age      | tinyint(3) unsigned  | YES  |     | NULL    |                |
		+----------+----------------------+------+-----+---------+----------------+
		3 rows in set (0.01 sec)

插入同样的数据会报错：

	mysql> INSERT tb5(username,age) VALUES('Tom',22);
	Query OK, 1 row affected (0.00 sec)
	
	mysql> INSERT tb5(username,age) VALUES('Tom',22);
	ERROR 1062 (23000): Duplicate entry 'Tom' for key 'username'

## 初涉默认约束 ##

DEFAULT

- 默认值
- 当插入记录时，如果没有明确为字段赋值，则自动赋予默认值。

		mysql> CREATE TABLE tb6(
		    -> id SMALLINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
		    -> username VARCHAR(20) NOT NULL UNIQUE KEY,
		    -> sex ENUM('1','2','3') DEFAULT '3'
		    -> );
		Query OK, 0 rows affected (0.12 sec)
	
		mysql> SHOW COLUMNS FROM tb6;
		+----------+----------------------+------+-----+---------+----------------+
		| Field    | Type                 | Null | Key | Default | Extra          |
		+----------+----------------------+------+-----+---------+----------------+
		| id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
		| username | varchar(20)          | NO   | UNI | NULL    |                |
		| sex      | enum('1','2','3')    | YES  |     | 3       |                |
		+----------+----------------------+------+-----+---------+----------------+
		3 rows in set (0.00 sec)

只插入`username`，表中的`sex`字段默认为`3`：

	mysql> INSERT tb6(username) VALUES('Tom');
	Query OK, 1 row affected (0.04 sec)
	
	mysql> SELECT * FROM tb6;
	+----+----------+------+
	| id | username | sex  |
	+----+----------+------+
	|  1 | Tom      | 3    |
	+----+----------+------+
	1 row in set (0.00 sec)

## 总结 ##

- 数据类型
	- 字符型
	- 整型
	- 浮点型
	- 日期时间型

- 数据表操作
	- 插入记录
	- 查找记录

- 记录操作
	- 创建数据表
	- 约束的使用


