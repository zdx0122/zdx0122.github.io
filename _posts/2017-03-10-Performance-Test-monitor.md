---
layout: post
title:  "性能测试监控"
date:   2017-03-10 02
categories: 性能测试
tags:  性能测试 Jmeter
author: Dex
---

* content
{:toc}

常见性能测试监控工具






一个系统的吞度量（承压能力）与request对CPU的消耗、外部接口、IO等等紧密关联。

单个reqeust 对CPU消耗越高，外部系统接口、IO影响速度越慢，系统吞吐能力越低，反之越高。

系统吞吐量几个重要参数：QPS（TPS）、并发数、响应时间

QPS（TPS）：每秒钟request/事务 数量

并发数： 系统同时处理的request/事务数

响应时间：  一般取平均响应时间

QPS（TPS）= 并发数/平均响应时间

无论有无思考时间（T_think），测试所得的TPS值和并发虚拟用户数(U_concurrent)、Loadrunner读取的交易响应时间（T_response）之间有以下关系（稳定运行情况下）：

TPS=U_concurrent / (T_response+T_think)。

http://www.ha97.com/5095.html

## 服务器监控 ##

top

iostat

	iostat -x -d -k  1

netstat

vmstat

mobaxterm

nmon

Jmeter插件监控


## 应用服务器监控 ##

jconsole

jvisualvm


## 数据库监控 ##



## web服务器监控 ##






未完待续。。。