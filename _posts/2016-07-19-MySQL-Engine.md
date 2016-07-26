---
layout: post
title:  "MySQL-数据引擎"
date:   2016-07-19 08
categories: 数据库
---





## 存储引擎简介 ##

	mysql> SHOW CREATE TABLE users \G;
	*************************** 1. row ***************************
	       Table: users
	Create Table: CREATE TABLE `users` (
	  `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT,
	  `username` varchar(20) NOT NULL,
	  `password` varchar(32) NOT NULL,
	  `age` tinyint(3) unsigned NOT NULL DEFAULT '10',
	  `sex` tinyint(1) DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB AUTO_INCREMENT=18 DEFAULT CHARSET=utf8
	1 row in set (0.00 sec)
	
	ERROR:
	No query specified

这里的InnoDB即是存储引擎。

MySQL可以将数据以不同的技术存储在文件（内存）中，这种技术就称为存储引擎。

每一种存储引擎使用不同的存储机制、索引技巧、锁定水平，最终提供广泛且不同的功能。

MySQL支持的存储引擎：

- MyISAM
- InnoDB
- Memory
- CSV
- Archive

## 相关知识点之并发处理 ##

- 并发控制
	- 当多个连接对记录进行修改时保证数据的一致性和完整性。

- 锁
	- 共享锁（读锁）：在同一时间段内，多个用户可以读取同一个资源，读取过程中数据不会发生任何变化。
	- 排它锁（写锁）：在任何时间只能有一个用户写入资源，当进行写锁时会阻塞其他的读锁或者写锁操作。

- 锁颗粒
	- 表锁，是一种开销最小的锁策略。
	- 行锁，是一种开销最大的锁策略。

## 相关知识点之事务处理 ##

- 事务
	- 事务用于保证数据库的完整性

- 事务的特性
	- 原子性（Atomicity）
	- 一致性（Consistency）
	- 隔离性（Isolation）
	- 持久性（Durability）

## 相关知识点之外键和索引 ##

- 外键
	- 是保证数据一致性的策略。

- 索引
	- 是对数据表中一列或多列的值进行排序的一种结构。
	- 分为普通索引、唯一索引、全文索引、btree索引、hash索引...


## 各种存储引擎的特点 ##

![各种存储引擎的特点](http://zdx0122.qiniudn.com/MySQL-%E5%90%84%E5%BC%95%E6%93%8E%E5%AF%B9%E6%AF%94.jpg)

CSV是有逗号分隔的存储引擎，不支持索引。

BlackHole： 黑洞引擎，写入的数据都会消失，一般用于做数据复制的中继。

MyISAM：适用于事务的处理不多的情况。

InnoDB: 适用于事务处理比较多，需要有外键支持的情况。

## 设置存储引擎 ##

修改存储引擎的方法：

- 通过修改MySQL配置文件实现
	- default-storage-engine = engine

- 通过创建数据表命令实现

示例：

	CREATE TABLE table_name(
	...
	...
	)ENGINE = engine;

