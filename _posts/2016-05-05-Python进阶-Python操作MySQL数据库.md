---
layout: post
title:  "Python进阶-Python操作MySQL数据库"
date:   2016-05-05 02
categories: Python
tags:  Python
author: i.itest.ren
---

* content
{:toc}





## Python DB API介绍 ##

Python DB API： Python访问数据库的统一接口规范

Python DB API 包含的内容：

![](http://7fvd6e.com1.z0.glb.clouddn.com/python_DB_API%E5%8C%85%E5%90%AB%E7%9A%84%E5%86%85%E5%AE%B9.jpg)

使用Python DB API访问数据库流程

![](http://7fvd6e.com1.z0.glb.clouddn.com/Python_%E8%AE%BF%E9%97%AE%E6%95%B0%E6%8D%AE%E5%BA%93%E6%B5%81%E7%A8%8B.jpg)

## 环境准备 ##

MySQL环境准备；

安装mysql for python，注意区分32位和64位系统，注意区分对应的Python版本；

下载地址：http://dev.mysql.com/downloads/connector/python/

## 数据库连接对象connection ##

连接对象：建立Python客户端与数据库的网络连接

创建方法：MySQLdb.Connect(参数)

![](http://7fvd6e.com1.z0.glb.clouddn.com/Python_connect%E5%8F%82%E6%95%B0.jpg)

connection对象支持的方法：

![](http://7fvd6e.com1.z0.glb.clouddn.com/Python_connection%E6%94%AF%E6%8C%81%E7%9A%84%E6%96%B9%E6%B3%95.jpg)

代码：

	import MySQLdb
	
	conn = MySQLdb.Connect(
		host = '192.168.1.108',
		port = '3306',
		user = 'root',
		password = '123456',
		db = 'test_db',
		charset = 'utf-8'
	)
	
	cursor = conn.cursor()
	
	print conn
	print cursor
	
	cursor.close()
	conn.close()

## 游标对象cursor ##

游标对象：用于执行查询和获取结果

cursor对象支持的方法：

![](http://7fvd6e.com1.z0.glb.clouddn.com/Python_cursor%E6%94%AF%E6%8C%81%E7%9A%84%E6%96%B9%E6%B3%95.jpg)

## insert/update/delete更新数据库 ##

![](http://7fvd6e.com1.z0.glb.clouddn.com/Python_%E6%9B%B4%E6%96%B0%E6%95%B0%E6%8D%AE%E5%BA%93%E6%B5%81%E7%A8%8B.jpg)

## 事务 ##

事务：访问和更新数据库的一个程序执行单元

- 原子性：事务中包括的诸操作要么都做，要么都不做
- 一致性：事务必须使数据库从一致性状态变到另一个一致性状态
- 隔离性：一个事务的执行不能被其他事务干扰
- 持久性：事务一旦提交，它对数据库的改变就是永久性的

开发中怎样使用事务？

- 关闭自动commit:设置conn.autocommit(Fault)
- 正常结束事务：conn.commit()
- 异常结束事务：conn.rollback()

http://www.runoob.com/python/python-mysql.html