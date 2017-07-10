---
layout: post
title:  "MySQL性能优化"
date:   2017-07-09 01
categories: MySQL
tags:  MySQL 性能
author: Dex
---

* content
{:toc}

MySQL数据库优化








## MySQL数据库优化介绍 ##

数据库优化的目的：

- 避免出现页面访问错误
	- 由于数据库连接timeout产生页面5xx错误
	- 由于慢查询造成页面无法加载
	- 由于阻塞造成数据无法提交
- 增加数据库的稳定性
	- 很多数据库问题都是由于低效的查询引起的
- 优化用户体验
	- 流畅页面的访问速度
	- 良好的网站功能体验

可以从几个方面进行数据库优化

![](http://zdx0122.qiniudn.com/mysql%E4%BC%98%E5%8C%96%E6%96%B9%E5%90%91.png)

## SQL语句优化 ##

### MySQL慢查询日志的开启方式和存储格式 ###

如何发现有问题的SQL？

使用MySQL慢查询日志对有效率问题的SQL进行监控

- show variables like 'slow_query_log';
	- 查看是否开启慢查询日志
- set global slow_query_log_file = '/home/mysql/sql_log/mysql-slow.log';
	- 记录慢查询日志的位置
- set global log_queries_not_using_indexes = on;
	- 开启没有使用索引的慢查询日志
- set global long_query_time = 1;
	- 大于1s的慢查询记录到日志中
- set global slow_query_log = on;
	- 开启慢查询日志

慢查询日志的存储格式：

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2%E6%97%A5%E5%BF%97%E7%9A%84%E5%AD%98%E5%82%A8%E6%A0%BC%E5%BC%8F.png)

慢查询日志所包含的内容：

- 执行SQL的主机信息
- SQL的执行信息
- SQL执行时间
- SQL的内容


### MySQL慢查询日志分析工具之mysqldumpslow ###

查询3条慢查询的sql：`mysqldumpslow -t 3 /home/mysql/sql_log/mysql-slow.log`

输出包含sql的执行次数、时间、锁等信息，内容较少；

### MySQL慢查询日志分析工具之pt-query-digest ###

输出到文件：`pt-query-digest slow-log > slow_log.report`

输出到数据库表：

    pt-query-digest slow.log -review \
	  h=127.0.0.1,D=test,p=root,P=3306,u=root,t=query_review \
	  --creat-reviewtable \
	  --review-history t=hostname_slow

### 如何通过慢查询日志发现有问题的SQL？ ###

- 查询次数多且每次查询占用时间长的SQL
	- 通常为pt-query-digest分析的前几个查询
- IO大的SQL
	- 注意pt-query-digest分析中的Rows examine项
- 未命中索引的SQL
	- 注意pt-query-digest分析中Rows examine 和 Raws Send 的对比

### 如何分析SQL查询 ###

使用`explain`查询SQL的执行计划

`explain`返回各列的含义：
- table：显示这一行的数据是关于哪张表的
- type：这是重要的列，显示链接使用了何种类型。从最好到最差的连接类型为const、eq_req、ref、range、index和ALL
- possible_keys：显示可能应用在这张表中的索引。如果为空，没有可能的索引。
- key：实际使用的索引。如果为NULL，则没有使用索引。
- key_len：使用的索引的长度。在不损失精确性的情况下，长度越短越好
- ref：显示索引的那一列被使用了，如果可能的话，是一个常数
- rows：MySQL认为必须检查的用来返回请求数据的行数
- extra列需要注意的返回值
	- Using filesort：看到这个的时候，查询就需要优化了。MySQL需要进行额外的步骤来发现如何对返回的行排序。它根据连接类型以及存储排序键值和匹配条件的全部行的行指针来排序全部行
	- Using temporary：看到这个的时候，查询就需要优化了。这里，MySQL需要创建一个临时表来存储结果，这通常发生在对不同的列集进行ORDER BY上，而不是 GROUP BY上

### Count()和Max()的优化 ###

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2max.png)

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2count.png)

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2count%E4%BC%98%E5%8C%96.png)

### 子查询的优化 ###

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2%E5%AD%90%E6%9F%A5%E8%AF%A2.png)

### group by的优化 ###

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2groupby.png)

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2groupby%E4%BC%98%E5%8C%96.png)

### limit查询的优化 ###

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2limit.png)

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2limit%E4%BC%98%E5%8C%961.png)

![](http://zdx0122.qiniudn.com/mysql-%E6%85%A2%E6%9F%A5%E8%AF%A2limit%E4%BC%98%E5%8C%962.png)

## 索引优化 ##

### 如何选择合适的列建立索引？ ###

1. 在where从句，group by从句，order by从句，on 从句中出现的列
2. 索引字段越小越好
3. 离散度大的列放到联合索引的前面
	1. SELECT * FROM payment WHERE staff_id =2 AND customer_id = 584;
	2. 是index(staff_id,customer_id)好，还是index(customer_id, staff_id)好？
	3. 由于customer_id的离散度更大，所以应该使用index(customer_id,staff_id)

### 索引优化SQL的方法 ###

索引的维护及优化---重复及冗余索引

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E9%87%8D%E5%A4%8D%E7%B4%A2%E5%BC%95.png)

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E5%86%97%E4%BD%99%E7%B4%A2%E5%BC%95.png)

索引的维护及优化---重复及冗余索引

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E6%9F%A5%E6%89%BE%E9%87%8D%E5%A4%8D%E5%8F%8A%E5%86%97%E4%BD%99%E7%B4%A2%E5%BC%95.png)