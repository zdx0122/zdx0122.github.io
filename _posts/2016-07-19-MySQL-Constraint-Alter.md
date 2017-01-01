---
layout: post
title:  "MySQL-约束和数据表操作"
date:   2016-07-19 02
categories: 数据库
tags:  MySQL 数据库
author: Dex
---

* content
{:toc}





## 外键约束的要求解析 ##

1. 约束保证数据的完整性和一致性
2. 约束分为表级约束和列级约束
3. 约束类型包括：
	1. NOT NULL(非空约束)
	2. PRIMARY KEY(主键约束)
	3. UNIQUE KEY(唯一约束)
	4. DEFAULT(默认约束)
	5. FOREIGN KEY(外键约束)

FOREIGN KEY：

- 保持数据一致性，完整性。
- 实现一对一或一对多关系。

### 外键约束的要求 ###

1. 父表和子表必须使用相同的存储引擎，而且禁止使用临时表。
2. 数据表的存储引擎只能为InnoDB。
3. 外键列和参照列必须具有相似的数据类型。其中数字的长度或是否有符号位必须相同；而字符的长度则可以不同。
4. 外键列和参照列必须创建索引。如果外键列不存在索引的话，MySQL将自动创建索引。

### 编辑数据表的默认存储引擎 ###

修改MySQL配置文件：

	default-storage-engine=INNODB

### 演示外键约束 ###

创建数据表：

	mysql> CREATE TABLE provinces(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> pname VARCHAR(20) NOT NULL
	    -> );
	Query OK, 0 rows affected (0.07 sec)

查看数据表DB类型：
	
	mysql> SHOW CREATE TABLE provinces;
	+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------	------------------------+
	| Table     | Create Table
	
	                        |
	+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------	------------------------+
	| provinces | CREATE TABLE `provinces` (
	  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
	  `pname` varchar(20) NOT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
	+-----------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
	1 row in set (0.02 sec)

创建一个`users`表，建立外键约束：

	mysql> CREATE TABLE users(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> username VARCHAR(10) NOT NULL,
	    -> pid BIGINT,
	    -> FOREIGN KEY(pid) REFERENCES provinces(id)
	    -> );
	ERROR 1215 (HY000): Cannot add foreign key constraint

出现错误，因为`users`表的`pid`和`provinces`表中的`id`字段的数据类型不一致。

创建`users`表，字段和`provinces`表`id`字段数据类型一致，但默认是有符号位的：

	mysql> CREATE TABLE users(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> username VARCHAR(10) NOT NULL,
	    -> pid SMALLINT,
	    -> FOREIGN KEY(pid) REFERENCES provinces(id)
	    -> );
	ERROR 1215 (HY000): Cannot add foreign key constraint

依然报错，所以符号位也必须要一致。

再次修改新建表的sql:

	mysql> CREATE TABLE users(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> username VARCHAR(10) NOT NULL,
	    -> pid SMALLINT UNSIGNED,
	    -> FOREIGN KEY(pid) REFERENCES provinces(id)
	    -> );
	Query OK, 0 rows affected (0.11 sec)

创建`users`表成功。

这时，`users`表为子表，`provinces`为父表；`user.pid`为外键列，`provinces.id`为参照列。

查看参照列索引：

	mysql> SHOW INDEXES FROM provinces\G;
	*************************** 1. row ***************************
	        Table: provinces
	   Non_unique: 0
	     Key_name: PRIMARY
	 Seq_in_index: 1
	  Column_name: id
	    Collation: A
	  Cardinality: 0
	     Sub_part: NULL
	       Packed: NULL
	         Null:
	   Index_type: BTREE
	      Comment:
	Index_comment:
	1 row in set (0.00 sec)
	
	ERROR:
	No query specified

注：`\G`为查询结果按列打印。

查看外键列索引：

	mysql> SHOW INDEXES FROM userS \G;
	*************************** 1. row ***************************
	        Table: users
	   Non_unique: 0
	     Key_name: PRIMARY
	 Seq_in_index: 1
	  Column_name: id
	    Collation: A
	  Cardinality: 0
	     Sub_part: NULL
	       Packed: NULL
	         Null:
	   Index_type: BTREE
	      Comment:
	Index_comment:
	*************************** 2. row ***************************
	        Table: users
	   Non_unique: 1
	     Key_name: pid
	 Seq_in_index: 1
	  Column_name: pid
	    Collation: A
	  Cardinality: 0
	     Sub_part: NULL
	       Packed: NULL
	         Null: YES
	   Index_type: BTREE
	      Comment:
	Index_comment:
	2 rows in set (0.01 sec)
	
	ERROR:
	No query specified

查看`users`表的表结构：

	mysql> SHOW CREATE TABLE users\G;
	*************************** 1. row ***************************
	       Table: users
	Create Table: CREATE TABLE `users` (
	  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(10) NOT NULL,
	  `pid` smallint(5) unsigned DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `pid` (`pid`),
	  CONSTRAINT `users_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `provinces` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8
	1 row in set (0.00 sec)
	
	ERROR:
	No query specified


## 外键约束的参照操作 ##

1. CASCADE:从父表或更新且自动删除或更新子表中匹配的行
2. SET NULL:从父表删除或更新行，并设置子表中的外键列为NULL。如果使用该选项，必须保证子表列没有指定NOT NULL。
3. RESTRICT:拒绝对父表的删除或更新操作。
4. NO ACTION:标准SQL的关键字，在MySQL中与RESTRICT相同。

创建一个新表：

	mysql> CREATE TABLE users1(
	    -> id SMALLINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
	    -> username VARCHAR(10) NOT NULL,
	    -> pid SMALLINT UNSIGNED,
	    -> FOREIGN KEY(pid) REFERENCES provinces(id) ON DELETE CASCADE
	    -> );
	Query OK, 0 rows affected (0.11 sec)

查看表结构：

	mysql> SHOW CREATE TABLE users1\G;
	*************************** 1. row ***************************
	       Table: users1
	Create Table: CREATE TABLE `users1` (
	  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(10) NOT NULL,
	  `pid` smallint(5) unsigned DEFAULT NULL,
	  PRIMARY KEY (`id`),
	  KEY `pid` (`pid`),
	  CONSTRAINT `users1_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `provinces` (`id`) ON DELETE CASCADE
	) ENGINE=InnoDB DEFAULT CHARSET=utf8
	1 row in set (0.00 sec)
	
	ERROR:
	No query specified

在`provinces`表中插入几条数据：

	mysql> INSERT provinces(pname) VALUES('A');
	Query OK, 1 row affected (0.10 sec)
	
	mysql> INSERT provinces(pname) VALUES('B');
	Query OK, 1 row affected (0.01 sec)
	
	mysql> INSERT provinces(pname) VALUES('C');
	Query OK, 1 row affected (0.00 sec)
	
	mysql> SELECT * FROM provinces;
	+----+-------+
	| id | pname |
	+----+-------+
	|  1 | A     |
	|  2 | B     |
	|  3 | C     |
	+----+-------+
	3 rows in set (0.03 sec)

在users1表插入记录：

	mysql> INSERT users1(username,pid) VALUES('Tom',3);
	Query OK, 1 row affected (0.02 sec)
	
	mysql> INSERT users1(username,pid) VALUES('John',7);
	ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint
	ails (`test`.`users1`, CONSTRAINT `users1_ibfk_1` FOREIGN KEY (`pid`) REFERENCE
	 `provinces` (`id`) ON DELETE CASCADE)

第二条记录出错，因为pid为7，在父表中不存在，所以出错。

继续插入两条数据：

	mysql> INSERT users1(username,pid) VALUES('Rose',2);
	Query OK, 1 row affected (0.00 sec)
	
	mysql> INSERT users1(username,pid) VALUES('John',3);
	Query OK, 1 row affected (0.00 sec)

	mysql> SELECT * FROM users1;
	+----+----------+------+
	| id | username | pid  |
	+----+----------+------+
	|  1 | Tom      |    3 |
	|  3 | Rose     |    2 |
	|  4 | John     |    3 |
	+----+----------+------+
	3 rows in set (0.00 sec)

中间插入失败一次，`id`值也会递增1。

删除`id`为3的记录：

	mysql> DELETE FROM provinces WHERE id=3;
	Query OK, 1 row affected (0.07 sec)

查看父表和子表的结果：

	mysql> SELECT * FROM provinces;
	+----+-------+
	| id | pname |
	+----+-------+
	|  1 | A     |
	|  2 | B     |
	+----+-------+
	2 rows in set (0.00 sec)
	
	mysql> SELECT * FROM users1;
	+----+----------+------+
	| id | username | pid  |
	+----+----------+------+
	|  3 | Rose     |    2 |
	+----+----------+------+
	1 row in set (0.00 sec)

## 表级约束与列级约束 ##

- 对一个数据列建立的约束，称为列级约束。
- 对多个数据列简历的约束，称为标记约束。
- 列级约束既可以在列定义时声明，也可以在列定义后声明。
- 表级约束只能在列定义后声明。


## 修改数据表-添加/删除列 ##

### 添加单列 ###

语法：

	ALTER TABLE tbl_name ADD [COLUMN] col_name column_definition [FIRST | AFTER col_name]

查看`users1`表结构：

	mysql> SHOW COLUMNS FROM users1;
	+----------+----------------------+------+-----+---------+----------------+
	| Field    | Type                 | Null | Key | Default | Extra          |
	+----------+----------------------+------+-----+---------+----------------+
	| id       | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
	| username | varchar(10)          | NO   |     | NULL    |                |
	| pid      | smallint(5) unsigned | YES  | MUL | NULL    |                |
	+----------+----------------------+------+-----+---------+----------------+
	3 rows in set (0.02 sec)

增加一列：

	mysql> ALTER TABLE users1 ADD age TINYINT UNSIGNED NOT NULL DEFAULT 10;
	Query OK, 0 rows affected (0.23 sec)
	Records: 0  Duplicates: 0  Warnings: 0

再增加一列：

	mysql> ALTER TABLE users1 ADD password1 VARCHAR(32) NOT NULL AFTER username;
	Query OK, 0 rows affected (0.10 sec)
	Records: 0  Duplicates: 0  Warnings: 0
	
	mysql> SHOW COLUMNS FROM users1;
	+-----------+----------------------+------+-----+---------+----------------+
	| Field     | Type                 | Null | Key | Default | Extra          |
	+-----------+----------------------+------+-----+---------+----------------+
	| id        | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
	| username  | varchar(10)          | NO   |     | NULL    |                |
	| password1 | varchar(32)          | NO   |     | NULL    |                |
	| pid       | smallint(5) unsigned | YES  | MUL | NULL    |                |
	| age       | tinyint(3) unsigned  | NO   |     | 10      |                |
	+-----------+----------------------+------+-----+---------+----------------+
	5 rows in set (0.00 sec)

再插入一列，并查看表结构：

	mysql> ALTER TABLE users1 ADD truename VARCHAR(20) NOT NULL FIRST;
	Query OK, 0 rows affected (0.10 sec)
	Records: 0  Duplicates: 0  Warnings: 0
	
	mysql> SHOW COLUMNS FROM users1;
	+-----------+----------------------+------+-----+---------+----------------+
	| Field     | Type                 | Null | Key | Default | Extra          |
	+-----------+----------------------+------+-----+---------+----------------+
	| truename  | varchar(20)          | NO   |     | NULL    |                |
	| id        | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
	| username  | varchar(10)          | NO   |     | NULL    |                |
	| password1 | varchar(32)          | NO   |     | NULL    |                |
	| pid       | smallint(5) unsigned | YES  | MUL | NULL    |                |
	| age       | tinyint(3) unsigned  | NO   |     | 10      |                |
	+-----------+----------------------+------+-----+---------+----------------+
	6 rows in set (0.00 sec)

### 添加多列 ###

语法：

	ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition...)

### 删除列 ###

	ALTER TABLE tbl_name DROP col_name;

删除多列： `ALTER TABLE tbl_name DROP col_name1，DROP col_name2;`

## 修改数据表-添加约束 ##

添加主键约束语法：

	ALTER TABLE tbl_name ADD [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name,...)

创建一个新表：

	mysql> CREATE TABLE users2(
	    -> username VARCHAR(10) NOT NULL,
	    -> pid SMALLINT UNSIGNED
	    -> );
	Query OK, 0 rows affected (0.09 sec)

添加一个字段：

	mysql> ALTER TABLE users2 ADD id SMALLINT UNSIGNED;
	Query OK, 0 rows affected (0.12 sec)
	Records: 0  Duplicates: 0  Warnings: 0

添加主键：

	mysql> ALTER TABLE users2 ADD CONSTRAINT PE_users2_id PRIMARY KEY (id);
	Query OK, 0 rows affected (0.07 sec)
	Records: 0  Duplicates: 0  Warnings: 0
	
	mysql> SHOW COLUMNS FROM users2;
	+----------+----------------------+------+-----+---------+-------+
	| Field    | Type                 | Null | Key | Default | Extra |
	+----------+----------------------+------+-----+---------+-------+
	| username | varchar(10)          | NO   |     | NULL    |       |
	| pid      | smallint(5) unsigned | YES  |     | NULL    |       |
	| id       | smallint(5) unsigned | NO   | PRI | NULL    |       |
	+----------+----------------------+------+-----+---------+-------+
	3 rows in set (0.00 sec)

添加唯一约束：

	mysql> ALTER TABLE users2 ADD UNIQUE (username);
	Query OK, 0 rows affected (0.03 sec)
	Records: 0  Duplicates: 0  Warnings: 0

### 添加/删除默认约束 ###

	ALTER TABLE tbl_name ALTER [COLUMN] col_name {SET DEFAULT literal | DROP DEFAULT}

## 修改数据表-删除约束 ##

### 删除主键约束 ###

语法：

	ALTER TABLE tbl_name DROP PRIMARY KEY

### 删除唯一约束 ###

语法：

	ALTER TABLE tbl_name DROP {INDEX | KEY} index_name


## 修改数据表-修改列定义和更名数据库 ##

修改列定义：

	ALTER TABLE tbl_name MODIFY [COLUMN] col_name column_definition [FIRST | AFTER col_name]

数据表更名：

- 方法1：`ALTER TABLE tbl_name RENAME [TO | AS] new_tbl_name`

- 方法2:`	RENAME TABLE tbl_name TO new_tbl_name [, tbl_name2 TO new_tbl_name2] ...`

## 总结 ##

- 约束
	- 功能
		- NOT NULL(非空约束)
		- PRIMARY KEY(主键约束)
		- UNIQUE KEY(唯一约束)
		- DEFAULT(默认约束)
		- FOREIGN KEY(外键约束)
	- 数据列的数目
		- 表级约束
		- 列级约束

- 修改数据表
	- 针对字段的操作：添加/删除字段、修改列定义，修改列名称等；
	- 针对约束的操作：添加/删除各种约束
	- 针对数据表的操作：数据表更名（两种方式）

