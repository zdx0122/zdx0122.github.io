---
layout: post
title:  "Jmeter工具常用功能和性能监控插件"
date:   2017-03-10 01
categories: 性能测试
tags:  性能测试 Jmeter
author: Dex
---

* content
{:toc}

Jmeter常用功能和性能监控工具





## Jmeter常用功能 ##

### 参数化 ###

参数化的变量(参数)，使用方式为${name}进行引用

#### CSV Data Set Config ####

![](http://zdx0122.qiniudn.com/Jmeter-%E5%8F%82%E6%95%B0%E5%8C%96.png)

Filename: 填写文件名称，如果不是当前路径，则需加入绝对或者相对路径

File encoding:编码，一般为utf-8，默认为ANSI

Variable Names:参数名称(有几个参数，在这里面就写几个参数名称，每个名称中间用分隔符分割，分隔符使用下面的“Delimitet”中定义的

Delimiter:定义分隔符，用于分隔文件中的参数，及上述Variable Names中定义的参数名称

Recycle on EOF：遇到文件结束符时，是否从头开始循环读入
注：程序从CSV Data Set Config文件中，每次读取一行，每次读取的参数仅供一个线程使用（类似Loadrunner里面的参数唯一值功能），如果线程数超过文本的记录行数，那么可以选择 True (从头再次读取)

Stop Thread on EOF: 当Recycle on EOF为False并且Stop Thread on EOF为True,则读完csv文件中的记录后, 停止运行

Allow Quoated data: True --设置文件中的参数值都必须用引用引起来,False则不需要

Sharing Mode: 设置是否线程共享

#### 用户定义的变量 ####

位置：右键-配置元件-用户定义的变量

填写变量名，变量值即可。

变量值中，也可以填写函数，例如：七天后的日期：`${__javaScript((new Date()).getFullYear()+'-'+ ((new Date()).getMonth()+1) + '-' + ((new Date()).getDate()+7),)} 19:00`


### 关联：正则表达式提取器 ###

正则表达式提取器就是LoadRunner中的关联。

说明：

- 引用名称：下一个请求要引用的参数名称，如填写token，则可用`${token}`引用它。
- 正则表达式：以`"token":"(.+?)"`为例
	- `()`：括起来的部分就是要提取的。
	- `.`：匹配任何字符串。
	- `+`：一次或多次。
	- `?`：不要太贪婪，在找到第一个匹配项后停止。
- 模板：用`$$`引用起来，如果在正则表达式中有多个正则表达式，则可以是`$2$$3$`等等，表示解析到的第几个值给token。如：`$1$`表示解析到的第1个值
- 匹配数字：0代表随机取值，1代表全部取值，通常情况下填0
- 缺省值：如果参数没有取得到值，那默认给一个值让它取。

提取多个字符串：

假如想匹配Web页面的如下部分：name = "file.name" value = "readme.txt">并提取file.name和readme.txt。一个合适的正则表达式：name = "(.+?)" value = "(.+?)"。这样就会创建2个组，分别用于$1$和$2$


### 数据库操作 ###

#### JDBC Connection Configuration ####

连接mysql库的话，需要下载`mysql-connector-java-3.1.14-bin.jar`驱动包,放置在Jmeter安装目录的lib目录里面；

![](http://zdx0122.qiniudn.com/JDBC-Connection-Configuration.png)

重要参数说明：

- Variable Name：数据库连接池的名称，我们可以有多个jdbc connection configuration，每个可以起个不同的名称，在jdbc request中可以通过这个名称选择合适的连接池进行使用。
- Database URL：数据库url，jdbc:mysql://主机ip或者机器名称:mysql监听的端口号/数据库名称， 如：jdbc:mysql://localhost:3306/test
- JDBC Driver class：JDBC驱动
- username：数据库登陆的用户名
- passwrod：数据库登陆的密码

不同数据库具体的填写方式：

- Datebase：MySQL
	- Driver class：com.mysql.jdbc.Driver
	- Database URL：jdbc:mysql://host:port/{dbname}
- Datebase：PostgreSQL
	- Driver class：org.postgresql.Driver
	- Database URL：jdbc:postgresql:{dbname}
- Datebase：Oracle
	- Driver class：oracle.jdbc.driver.OracleDriver
	- Database URL：jdbc:oracle:thin:user/pass@//host:port/service
- Datebase：Ingres (2006)
	- Driver class：ingres.jdbc.IngresDriver
	- Database URL：	jdbc:ingres://host:port/db[;attr=value]
- Datebase：MSSQL
	- Driver class：com.microsoft.sqlserver.jdbc.SQLServerDriver或者net.sourceforge.jtds.jdbc.Driver
	- Database URL：jdbc:sqlserver://IP:1433;databaseName=DBname或者jdbc:jtds:sqlserver://localhost:1433/"+"library"


#### JDBC Request ####

重要的参数说明：

- Variable Name：数据库连接池的名字，需要与JDBC Connection Configuration的Variable Name Bound Pool名字保持一致
- Query：填写的sql语句未尾不要加“;”
- Parameter valus：参数值
- Parameter types：参数类型，可参考：Javadoc for java.sql.Types
- Variable names：保存sql语句返回结果的变量名
- Result variable name：创建一个对象变量，保存所有返回的结果
- Query timeout：查询超时时间
- Handle result set：定义如何处理由callable statements语句返回的结果


## Jmeter监控工具 ##

### 聚合报告 ###

查看响应时间、QPS、错误率信息

### jp@gc - Active Threads Over Time ###

查看活动线程数折线图，导入jmeter-plugins-manager-0.11.jar，在选项-Plugins Manager中下载。

### jp@gc - Response Times Over Time ###

查看响应时间折线图；导入jmeter-plugins-manager-0.11.jar，在选项-Plugins Manager中下载。

### jp@gc - Transactions per Second ###

查看TPS折线图，导入jmeter-plugins-manager-0.11.jar，在选项-Plugins Manager中下载。

### jp@gc - PerfMon Metrics Collector ###

查看服务器CPU、Mem、磁盘IO、网络IO、SWAP、TCP、JMX、EXEC、TAIL等信息。

需要下载ServerAgent-2.2.1.zip，在压测机和被压测服务器中间建立链接(都要安装并保持运行状态)，默认端口为4444.

