---
layout: post
title:  "HttpClient教程"
date:   2016-06-17 01
categories: Java
tags:  Java
author: Dex
---

* content
{:toc}




## HttpClient是用来干什么的？ ##

HttpClient是一个客户端的HTTP通信实现库。HttpClient的目标是发送和接收HTTP报文。HttpClient不是一个浏览器。

在测试工作中，使用Java编写接口测试接口框架时，会用到 HttpClient，类似与Python接口测试框架中的requests.

官网下载地址：http://hc.apache.org/downloads.cgi

官网文档：http://hc.apache.org/httpcomponents-client-4.5.x/index.html

官网教程：http://hc.apache.org/httpcomponents-client-4.5.x/tutorial/html/index.html

## 功能特色 ##

写几个用的多的特色~

- 基于标准的纯java，HTTP版本1和1.1的实现
- 支持所有的HTTP方法(GET, POST, PUT, DELETE, HEAD, OPTION, and TRACE)
- 支持HTTPS加密协议
- 通过HTTP代理透明连接
- 用于多线程应用程序中的连接管理支持。支持设置最大的总连接以及主机的最大连接数。检测和关闭陈旧的连接。
- 自动处理Set-Cookie中的Cookie
- 在http1.0和http1.1中利用KeepAlive保持持久连接
- 直接获取服务器发送的response code和 headers
- 设置连接超时的能力
- 源代码基于Apache License 可免费获取


## HTTP请求和响应 ##

HttpClient的最本质的功能是执行HTTP方法。

每个HTTP方法都有一个相对应的类：HttpGet,HttpHead,HttpPost,HttpPut,HttpDelete, HttpOptions, HttpTrace.

未完待补充。