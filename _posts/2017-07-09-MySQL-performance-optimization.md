---
layout: post
title:  "MySQL性能优化"
date:   2017-07-09 01
categories: MySQL
tags:  MySQL 性能
author: i.itest.ren
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

索引的维护及优化---查找重复及冗余索引

使用`pt-duplicate-key-checker`工具检查重复及冗余索引

	pt-duplicate-key-checker \
	-uroot \
	-p '123456' \
	-h 127.0.0.1

### 索引维护的方法 ###

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E7%B4%A2%E5%BC%95%E7%BB%B4%E6%8A%A4%E7%9A%84%E6%96%B9%E6%B3%95.png)

## 数据库结构优化 ##

### 选择合适的数据类型 ###

- 使用可以存下你的数据的最小的数据类型
- 使用简单的数据类型。Int要比varchar类型在mysql处理上简单
- 尽可能的是使用not null定义字段
- 尽量少用text类型，非用不可时最好考虑分表

### 数据库表的范式化优化 ###

表的范式化

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E8%A1%A8%E7%9A%84%E8%8C%83%E5%BC%8F%E5%8C%96%E5%92%8C%E5%8F%8D%E8%8C%83%E5%BC%8F%E5%8C%964.png)

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E8%A1%A8%E7%9A%84%E8%8C%83%E5%BC%8F%E5%8C%96%E5%92%8C%E5%8F%8D%E8%8C%83%E5%BC%8F%E5%8C%96.png)

表的反范式化

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E8%A1%A8%E7%9A%84%E8%8C%83%E5%BC%8F%E5%8C%96%E5%92%8C%E5%8F%8D%E8%8C%83%E5%BC%8F%E5%8C%962.png)

![](http://zdx0122.qiniudn.com/mysql-%E7%B4%A2%E5%BC%95-%E8%A1%A8%E7%9A%84%E8%8C%83%E5%BC%8F%E5%8C%96%E5%92%8C%E5%8F%8D%E8%8C%83%E5%BC%8F%E5%8C%963.png)

### 表的垂直拆分 ###

所谓的垂直拆分，就是把原来一个有很多列的表拆分成多个表，这解决了表的宽度问题。通常垂直拆分可以按以下原则进行：

- 把不常用的字段单独存放到一个表中
- 把大字段独立存放到一个表中
- 把经常一起使用的字段放到一起

### 表的水平拆分 ###

表的水平拆分是为了解决单表的数据量过大的问题，水平拆分的表每一个表的结果都是完全一致的。

常用的拆分方式：

1. 对id进行hash运算，如果要拆分成5个表，则使用mod(id, 5)取出0-4个值
2. 针对不同的hashID把数据存到不同的表中。

## 数据库系统配置优化 ##

### 操作系统配置优化 ###

数据库是基于操作系统的，目前大多数MySQL都是安装在Linux系统之上，所以对于操作系统的一些参数配置也会影响到MySQL的性能。

常用的系统配置：

- 网络方面的配置，要修改`/etc/sysctl.conf`文件
	- 增加tcp支持的队列数
		- net.ip4.tcp_max_syn_backlog = 65535
	- 减少断开连接时，资源回收
		- net.ipv4.tcp_max_tw_buckets =8000
		- net.ipv4.tcp_reuse = 1
		- net.ipv4.tcp_recycle = 1
		- net.ipv4.tcp_fin_timeout = 10
- 打开文件数的限制，可以使用ulimit -a 查看目录的限制，可以修改`/etc/security/limits.conf`文件，增加以下内容：
	- soft nofile 65535
	- hard nofile 65535
- 除此之外最好在MySQL服务器上关闭iptables，selinux等防火墙软件。

### MySQL配置文件 ###

MySQL可以通过启动时指定配置参数和使用配置文件两种方法进行配置，在大多数情况下配置文件位于/etc/my.cnf或是/etc/mysql/my.cnf；在windows系统配置文件可以是位于C:/windows/my.ini文件，MySQL查找配置文件的顺序可以通过以下方法获得：

	$ /usr/sbin/mysqld --verbose --help | grep -A 1 'Default options '

注意：如果存在多个位置存在配置文件，则后面的会覆盖前面的

常用参数说明：

- innodb_buffer_pool_size
	- 非常重要的参数，用于配置Innodb的缓冲池，如果数据库中只有Innodb表，则推荐配置量为总内存的75%
	- SELECT ENGINE, ROUNT(SUM(data_length + index_length)/1024/1024, 1) AS "Total MB", FROM INFORMATION_SCHEMA.TABLES WHERE table_schema not in ("information_schema", "performance_schema") GROUP BY ENGINE;
	- Innodb_buffer_pool_size >= Total MB
- innodb_buffer_pool_instances
	- MySQL5.5中新增加参数，可以控制缓冲池的个数，默认情况下只有一个缓冲池
- innodb_log_buffer_size
	- innodb log 缓冲的大小，由于日志最长每秒钟就会刷新所以一般不用太大
- innodb_flush_log_at_trx_commit
	- 关键参数，对innodb的IO效率影响很大。默认值为1，可以取0，1，2三个值，一般建议为2，但如果数据安全性要求比较高，则使用默认值1
- innodb_read_io_threads 和 innodb_white_io_threads
	- 以上两个参数决定了读写IO进程数，默认为4
- innodb_file_per_table
	- 关键参数，控制Innodb每个表使用独立的表空间，默认为OFF，也就是所有表都会减利在共享表空间中
- innodb_stats_on_metadata
	- 决定了MySQL在什么情况下会刷新innodb表的统计信息

### 第三方配置工具 ###

Percon Configuration Wizard

https://tools.percona.com/wizard

## 服务器硬件优化 ##

### 如何选择CPU？ ###

- MySQL有一些工作只能使用到单核CPU
	- Replicate，SQL......
- MySQL对CPU核数的支持并不是越快越好
	- MySQL5.5使用的服务器不要超过32核

### Disk IO优化 ###

常用RAID级别简介：

- RAID0：也成为条带，就是把多个磁盘连接成一个硬盘使用，这个级别IO最好
- RAID1：也成为镜像，要求至少有两个磁盘，魅族磁盘存储的数据相同
- RAID5：也是把多个（最少三个）硬盘合并成1个逻辑盘使用，数据读写时会简历奇偶校验信息，并且奇偶校验信息和相对应的数据分别存储于不同的磁盘上。当RAID5的一个磁盘数据发生损坏后，利用剩下的数据和相应的奇偶校验信息去回复被损坏的数据
- RAID1+0：就是RAID1和RAID0的结合。同时具备两个级别的优缺点。一般建议数据库使用这个级别。

SNA 和 NAT 是否适合数据库？

- 常用于高可用解决方案。
- 顺序读写效率很高，但是随机读写不如人意。
- 数据库随机读写比率很高。

