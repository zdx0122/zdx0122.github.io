---
layout: post
title:  "MySQL-增删改查"
date:   2016-07-19 03
categories: 数据库
---





## 插入记录-INSERT ##

插入记录语法：

	INSERT [INTO] tbl_name [(col_name,...)] {VALUES | VALUE} ({expr | DEFAULT},...),(...),...

创建一个新表：

	mysql> CREATE TABLE users(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> username VARCHAR(20) NOT NULL,
	    -> password VARCHAR(32) NOT NULL,
	    -> age TINYINT UNSIGNED NOT NULL DEFAULT 10,
	    -> sex BOOLEAN
	    -> );
	Query OK, 0 rows affected (0.04 sec)

插入记录：

	mysql> INSERT users VALUES(NULL, 'Tom', '123', 25, 1);
	Query OK, 1 row affected (0.10 sec)
	
	mysql> INSERT users VALUES(NULL, 'John', '456', 25, 1);
	Query OK, 1 row affected (0.01 sec)

	mysql> INSERT users VALUES(DEFAULT, 'John', '456', 25, 1);
	Query OK, 1 row affected (0.05 sec)
	
查询记录：

	mysql> SELECT * FROM users;
	+----+----------+----------+-----+------+
	| id | username | password | age | sex  |
	+----+----------+----------+-----+------+
	|  1 | Tom      | 123      |  25 |    1 |
	|  2 | John     | 456      |  25 |    1 |
	|  3 | John     | 456      |  25 |    1 |
	+----+----------+----------+-----+------+
	3 rows in set (0.00 sec)

插入数据可以使用表达式：

	mysql> INSERT INTO users VALUES(DEFAULT, 'Tom', '123', 3*8-5, 1);
	Query OK, 1 row affected (0.06 sec)

有默认项的字段，插入数据时，可填入`DEFAULT`：

	mysql> INSERT INTO users VALUES(DEFAULT, 'Tom', '123', DEFAULT, 1);
	Query OK, 1 row affected (0.00 sec)

插入数据可以使用`md5()`：

	mysql> INSERT INTO users VALUES(DEFAULT, 'Rose', md5('123'), DEFAULT, 0);
	Query OK, 1 row affected (0.09 sec)

记录如下：

	mysql> SELECT * FROM users;
	+----+----------+----------------------------------+-----+------+
	| id | username | password                         | age | sex  |
	+----+----------+----------------------------------+-----+------+
	|  1 | Tom      | 123                              |  25 |    1 |
	|  2 | John     | 456                              |  25 |    1 |
	|  3 | John     | 456                              |  25 |    1 |
	|  4 | Tom      | 123                              |  19 |    1 |
	|  5 | Tom      | 123                              |  10 |    1 |
	|  6 | Rose     | 202cb962ac59075b964b07152d234b70 |  10 |    0 |
	+----+----------+----------------------------------+-----+------+
	6 rows in set (0.00 sec)


## 插入记录-INSERT SET-SELECT ##

INSERT SET 插入记录语法：

	INSERT [INTO] tbl_name SET col_name={expr | DEFAULT},...

说明： 与第一种方式的区别在于，此方法可以使用子查询(SubQuery)

	mysql> INSERT INTO users SET username='Ben', password='456';
	Query OK, 1 row affected (0.01 sec)

INSERT SELECT 插入记录语法：

	INSERT [INTO] tbl_name [(col_name,...)] SELECT ...

说明： 此方法可以将查询结果插入到指定数据表。


## 更新记录-UPDATE ##

更新记录（单表更新）：

	UPDATE [LOW_PRIORITY] [IGNORE] table_references SET col_name1={expr1 | DEFAULT} [, col_name2={expr2 | DEFAULT}]... [WHERE where_condition]

更新所有人的年龄加五岁：

	mysql> UPDATE users SET age = age+5;
	Query OK, 7 rows affected (0.13 sec)
	Rows matched: 7  Changed: 7  Warnings: 0

更新年龄为年龄-id,性别为0:

	mysql> UPDATE users SET age= age-id, sex=0;
	Query OK, 7 rows affected (0.01 sec)
	Rows matched: 7  Changed: 7  Warnings: 0

把偶数id的年龄加10岁：

	mysql> UPDATE users SET age = age + 10 WHERE id % 2 = 0;
	Query OK, 3 rows affected (0.08 sec)
	Rows matched: 3  Changed: 3  Warnings: 0

记录如下：

	mysql> SELECT * FROM users;
	+----+----------+----------------------------------+-----+------+
	| id | username | password                         | age | sex  |
	+----+----------+----------------------------------+-----+------+
	|  1 | Tom      | 123                              |  29 |    0 |
	|  2 | John     | 456                              |  38 |    0 |
	|  3 | John     | 456                              |  27 |    0 |
	|  4 | Tom      | 123                              |  30 |    0 |
	|  5 | Tom      | 123                              |  10 |    0 |
	|  6 | Rose     | 202cb962ac59075b964b07152d234b70 |  19 |    0 |
	|  7 | Ben      | 456                              |   8 |    0 |
	+----+----------+----------------------------------+-----+------+
	7 rows in set (0.00 sec)


## 删除记录-DELETE ##

删除记录（单表删除）：

	DELETE FROM tbl_name [WHERE where_conditiion]

删除id为6的记录：

	DELETE FROM users WHERE id = 6;

这时，再插入一条记录：

	mysql> INSERT users VALUES(NULL, '111', '222', 33, NULL);
	Query OK, 1 row affected (0.01 sec)

新插入的字段id为8：

	mysql> SELECT * FROM users;
	+----+----------+----------+-----+------+
	| id | username | password | age | sex  |
	+----+----------+----------+-----+------+
	|  1 | Tom      | 123      |  29 |    0 |
	|  2 | John     | 456      |  38 |    0 |
	|  3 | John     | 456      |  27 |    0 |
	|  4 | Tom      | 123      |  30 |    0 |
	|  5 | Tom      | 123      |  10 |    0 |
	|  7 | Ben      | 456      |   8 |    0 |
	|  8 | 111      | 222      |  33 | NULL |
	+----+----------+----------+-----+------+
	7 rows in set (0.00 sec)

## 查询表达式解析 ##

用的最多的sql是SELECT!

查找记录：

	SELECT select_expr [, select_expr ...]
	[
		FROM table_references
		[WHERRE where_condition]
		[GROUP BY {col_name | position} [ASC | DESC],...]
		[HAVING where_condition]
		[ORDER BY {col_name | expr | position} [ASC | DESC],...]
		[LIMIT {[offset,] row_count | row_count OFFSET offset}]
	]

### select_expr ###

查询表达式：

- 每一个表达式标识想要的一列，必须有至少一个。
- 多个列之间以英文逗号分隔。
- 星号(*)表示所有列。tbl_name.* 可以表示命名表的所有列。
- 查询表达式可以使用[ AS ] alias_name 为其赋予别名。
- 别用可用于GROUP BY, ORDER BY 或HAVING字句。

## where语句进行条件查询 ##

条件表达式

对记录进行过滤，如果没有指定WHERE子句，则显示所有记录。

在WHERE表达式中，可以使用MySQL支持的函数或运算符。

## GROUP BY语句对查询结果分组 ##

查询结果分组

	[GROUP BY {col_name | position} [ASC | DESC],...]

## HAVING语句设置分组条件 ##

分组条件

	[HAVING where_condition]

## ORDER BY语句对查询结果排序 ##

对查询结果进行排序

	[ORDER BY {col_name | expr | position} [ASC | DESC], ...]

## LIMIT语句限制查询数量 ##

限制查询结果返回的数量

	[LIMIT {[offset,] row_count | row_count OFFSET offset}]

