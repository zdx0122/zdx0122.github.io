---
layout: post
title:  "初涉MySQL"
date:   2016-07-17 01
categories: 数据库
---





## 初涉MySQL ##

MySQL由瑞典MySQL AB公司开发，目前属于Oracle公司。

MySQL是一个开源的关系型数据库管理系统。

MySQL分为社区版和企业版。

## 安装MySQL ##

MySQL安装方式：

- MSI安装
- ZIP安装

MSI安装MySQL配置方式：执行安装目录/bin/MySQLInstanceConfig.exe

ZIP安装MySQL配置文件：

如果下载的是zip文件解压安装MySQL，在根目录下，需要新建一个`my.ini`文件，输入以下内容：

	[mysql]
	# 设置mysql客户端默认字符集
	default-character-set=utf8 
	[mysqld]
	#设置3306端口
	port = 3306 
	# 设置mysql的安装目录
	basedir=D:\mysql\mysql-5.6.17-winx64
	# 设置mysql数据库的数据的存放目录
	datadir=D:\mysql\mysql-5.6.17-winx64\data
	# 允许最大连接数
	max_connections=200
	# 服务端使用的字符集默认为8比特编码的latin1字符集
	character-set-server=utf8
	# 创建新表时将使用的默认存储引擎
	default-storage-engine=INNODB

MySQL目录结构：

- MySQL
	- bin	存储可执行文件
	- data	存储数据文件
	- docs	文档
	- include	存储包含的头文件
	- lib	存储库文件
	- share	错误信息和字符集文件

根目录下的my.ini文件可以修改编码方式、端口号等

## 启动/关闭MySQL ##

services.msc中，可以启动MySQL服务。

启动MySQL服务：`net start mysql`

停止MySQL服务：`net stop mysql`


### 启动错误解决方法 ###

zip安装的话，会出现错误如下：

	C:\Users\Administrator>net start mysql
	MySQL 服务正在启动 ..
	MySQL 服务无法启动。
	
	服务没有报告任何错误。
	
	请键入 NET HELPMSG 3534 以获得更多的帮助。

解决方案：

1. 在`my.ini`文件中，注释掉`data`目录。
2. 删除掉根目录下的`data`目录。
3. cmd命令行执行`mysqld –initialize`。
4. 重新执行`net start mysql`启动MySQL服务。

## MySQL登录 ##

MySQL参数：

	-D, --database=name	打开指定数据库
	--delimiter = name	指定分隔符
	-h, --host=name	服务器名称
	-p, --password[=name]	密码
	-P, --port=#		端口号
	--prompt=name	设置提示符
	-u, --user=name	用户名
	-V， --ersion	输出版本信息并且退出

例如：`mysql -h127.0.0.1 -P3306 -uroot -p`

MySQL退出：

	1. mysql > exit;
	2. mysql > quit;
	3. mysql > \q;

###　MySQL5.7.13版本无法登录的问题　###

1.my.nin文件里在 `[mysqld]`下增加一行`skip-grant-tables`

2.cmd：`net start mysql`

	->mysql
	->use mysql;
	->UPDATE user SET authentication_string=PASSWORD("NEWPASSWORD") WHERE User='root';
	->FLUSH PRIVILEGES;
	->quit;

3.登陆时，将`my.min`文件里的“`skip-grant-tables`”删除；
登陆`mysql -u用户 -p密码`;

修改密码的sql：`alter user 'root'@'localhost' identified by '123456'; `


## 修改MySQL提示符 ##

连接客户端时通过参数指定：`mysql -uroot -proot --prompt 提示符`
例如：`mysql -uroot -proot --prompt \h`, 提示符会修改为`localhost`.

连接上客户端后，通过prompt命令修改:`prompt 提示符`

例如：`prompt mysql>`, 提示符会修改为`mysql>`.

提示符参数：

	\D	完整的日期
	\d	当前数据库
	\h	服务器名称
	\u	当前用户

例如：`prompt \u@\h \d>`, 则显示为`root@localhost (none)>`

## MySQL常用命令 ##

- 显示当前服务器版本

		mysql> SELECT VERSION();
		+-----------+
		| VERSION() |
		+-----------+
		| 5.7.13    |
		+-----------+
		1 row in set (0.00 sec)

- 显示当前日期时间

		mysql> SELECT NOW();
		+---------------------+
		| NOW()               |
		+---------------------+
		| 2016-07-18 18:21:31 |
		+---------------------+
		1 row in set (0.00 sec)

- 显示当前用户

		mysql> SELECT USER();
		+----------------+
		| USER()         |
		+----------------+
		| root@localhost |
		+----------------+
		1 row in set (0.00 sec)

## MySQL语句的规范 ##

- 关键字与函数名称全部大写
- 数据库名称、表名称、字段名称全部小写
- SQL语句必须以分号结尾

## 操作数据库 ##

注：`{}`为必填项, `|`为两项或者多项中做选择，`[]`为可填项。

### 创建数据库 ###

    CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] CHARACTER SET [=] charset_name

例如：`CREATE DATABASE t1;`创建`t1`数据库：

	mysql> CREATE DATABASE t1;
	Query OK, 1 row affected (0.00 sec)

继续创建一次`t1`数据库：

	mysql> create database t1;
	ERROR 1007 (HY000): Can't create database 't1'; database exists

执行`CREATE DATABASE IF NOT EXISTS t1;`：

	mysql> CREATE DATABASE IF NOT EXISTS t1;
	Query OK, 1 row affected, 1 warning (0.00 sec)

出现警告信息，可以执行`SHOW WARNINGS;`查看警告信息：

	mysql> SHOW WARNINGS;
	+-------+------+---------------------------------------------+
	| Level | Code | Message                                     |
	+-------+------+---------------------------------------------+
	| Note  | 1007 | Can't create database 't1'; database exists |
	+-------+------+---------------------------------------------+
	1 row in set (0.00 sec)

查看数据库创建时使用的编码方式`SHOW CREATE DATABASE t1`：

	mysql> SHOW CREATE DATABASE t1;
	+----------+-------------------------------------------------------------+
	| Database | Create Database                                             |
	+----------+-------------------------------------------------------------+
	| t1       | CREATE DATABASE `t1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
	+----------+-------------------------------------------------------------+
	1 row in set (0.00 sec)

创建一个和默认编码方式不一样的数据库, 执行`CREATE DATABASE IF NOT EXISTS t2 CHARACTER SET gbk;`：

	mysql> CREATE DATABASE IF NOT EXISTS t2 CHARACTER SET gbk;
	Query OK, 1 row affected (0.00 sec)

查看`t2`数据库的编码方式：

	mysql> SHOW CREATE DATABASE t2;
	+----------+------------------------------------------------------------+
	| Database | Create Database                                            |
	+----------+------------------------------------------------------------+
	| t2       | CREATE DATABASE `t2` /*!40100 DEFAULT CHARACTER SET gbk */ |
	+----------+------------------------------------------------------------+
	1 row in set (0.00 sec)

### 查看当前服务器下的数据库列表 ###

    SHOW {DATABASES | SCHEMAS} [LIKE 'pattern' | WHERE expr]
	
输出：

	mysql> show databases;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| sys                |
	| t1                 |
	+--------------------+
	5 rows in set (0.00 sec)

### 修改数据库 ###

    ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET [=] charset_name

修改`t2`数据库的编码方式`ALTER DATABASE t2 CHARACTER SET = utf-8;`

	mysql> ALTER DATABASE t2 CHARACTER SET = utf8;
	Query OK, 1 row affected (0.00 sec)
	
查看`t2`数据库编码方式是否修改：

	mysql> SHOW CREATE DATABASE t2;
	+----------+-------------------------------------------------------------+
	| Database | Create Database                                             |
	+----------+-------------------------------------------------------------+
	| t2       | CREATE DATABASE `t2` /*!40100 DEFAULT CHARACTER SET utf8 */ |
	+----------+-------------------------------------------------------------+
	1 row in set (0.00 sec)

### 删除数据库 ###

    DROP {DATABASE | SCHEMA} [IF EXISTS] db_name

删除`t1`数据库`DROP DATABASE t1;`：

	mysql> DROP DATABASE t1;
	Query OK, 0 rows affected (0.00 sec)

验证是否已经删除`t1`数据库：

	mysql> SHOW DATABASES;
	+--------------------+
	| Database           |
	+--------------------+
	| information_schema |
	| mysql              |
	| performance_schema |
	| sys                |
	| t2                 |
	+--------------------+
	5 rows in set (0.00 sec)

再次删除一次`t1`数据库：

	mysql> DROP DATABASE t1;
	ERROR 1008 (HY000): Can't drop database 't1'; database doesn't exist
执行`DROP DATABASE IF EXISTS t1;`：

	mysql> DROP DATABASE IF EXISTS t1;
	Query OK, 0 rows affected, 1 warning (0.00 sec)

