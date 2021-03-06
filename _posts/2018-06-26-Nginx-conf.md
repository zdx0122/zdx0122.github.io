---
layout: post
title:  "从一份配置清单详解 Nginx 服务器配置"
date:   2018-06-12 01
categories: 服务容器
tags:  Nginx 
author: i.itest.ren
---

* content
{:toc}

Nginx 是轻量级的高性能 Web 服务器，提供了诸如 HTTP 代理和反向代理、负载均衡、缓存等一系列重要特性，因而在实践之中使用广泛。









感谢原作者的奉献，原作者博客地址：[https://www.v2ex.com/t/465840](https://www.v2ex.com/t/465840)。

详解一下 Nginx 服务器的各种配置指令的作用和用法。

## Nginx 配置文件的整体结构 ##

![](http://zdx0122.qiniudn.com/9824247-6f7a9706b9f3982b.jpg)

从图中可以看出主要包含以下几大部分内容：

1.全局块

该部分配置主要影响 Nginx 全局，通常包括下面几个部分：

- 配置运行 Nginx 服务器用户（组）
- worker process 数
- Nginx 进程 PID 存放路径
- 错误日志的存放路径
- 配置文件的引入


2.events 块

该部分配置主要影响 Nginx 服务器与用户的网络连接，主要包括：

- 设置网络连接的序列化
- 是否允许同时接收多个网络连接
- 事件驱动模型的选择
- 最大连接数的配置


3.http 块

- 定义 MIMI-Type
- 自定义服务日志
- 允许 sendfile 方式传输文件
- 连接超时时间
- 单连接请求数上限

4.server 块

- 配置网络监听
- 基于名称的虚拟主机配置
- 基于 IP 的虚拟主机配置

5.location 块

- location 配置
- 请求根目录配置
- 更改 location 的 URI
- 网站默认首页配置

## 一份配置清单例析 ##

一份简要的清单配置举例：

![](http://zdx0122.qiniudn.com/9824247-2d04bfc0dfe6fa6e.jpg)

配置代码如下：

	user  nobody  nobody;
	worker_processes  3;
	error_log  logs/error.log;
	pid  logs/nginx.pid;
	
	events {
		use epoll;
	    worker_connections  1024;
	}
	
	
	http {
	    include       mime.types;
	    default_type  application/octet-stream;
	
	    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
	                      '$status $body_bytes_sent "$http_referer" '
	                      '"$http_user_agent" "$http_x_forwarded_for"';
	    access_log  logs/access.log  main;
	    sendfile  on;
	    keepalive_timeout  65;
	
	    server {
	        listen       8088;
	        server_name  codesheep;
	        access_log  /codesheep/webserver/server1/log/access.log;
	        error_page  404  /404.html;
	
	        location /server1/location1 {
	            root   /codesheep/webserver;
	            index  index.server2-location1.htm;
	        }
	
	        location /server1/location2 {
		    root   /codesheep/webserver;
	            index  index.server2-location2.htm;
	        }
	
	    }
	
	    server {
	        listen       8089;
	        server_name  192.168.31.177;
	        access_log  /codesheep/webserver/server2/log/access.log;
	        error_page  404  /404.html;
			
	        location /server2/location1 {
	            root   /codesheep/webserver;
	            index  index.server2-location1.htm;
	        }
	
	        location /srv2/loc2 {
	            alias   /codesheep/webserver/server2/location2/;
	            index  index.server2-location2.htm;
	        }
			
	        location = /404.html {
		        root /codesheep/webserver/;
		        index 404.html;
	        }
			
	    }
	
	}


接下来就来详细剖析以下配置文件中各个指令的含义

### 配置运行 Nginx 服务器用户（组） ###

指令格式：user user [group];

- user：指定可以运行 Nginx 服务器的用户
- group：可选项，可以运行 Nginx 服务器的用户组

如果 user 指令不配置或者配置为 user nobody nobody ，则默认所有用户都可以启动 Nginx 进程

### worker process 数配置 ###

Nginx 服务器实现并发处理服务的关键，指令格式：worker_processes number | auto;

- number：Nginx 进程最多可以产生的 worker process 数
- auto：Nginx 进程将自动检测

按照上文中的配置清单的实验，我们给 worker_processes 配置的数目是：3，启动 Nginx 服务器后，我们可以后台看一下主机上的 Nginx 进程情况：

    ps -aux | grep nginx

很明显，理解 worker_processes 这个指令的含义就很容易了

![](http://zdx0122.qiniudn.com/9824247-b39d1a874d9bb50a.jpg)

### Nginx 进程 PID 存放路径 ###

Nginx 进程是作为系统守护进程在运行，需要在某文件中保存当前运行程序的主进程号，Nginx 支持该保存文件路径的自定义

指令格式：pid file;

- file：指定存放路径和文件名称
- 如果不指定默认置于路径 logs/nginx.pid

### 错误日志的存放路径 ###

指定格式：error_log file | stderr;

- file：日志输出到某个文件 file
- stderr：日志输出到标准错误输出

### 配置文件的引入
指令格式：include file;

- 该指令主要用于将其他的 Nginx 配置或者第三方模块的配置引用到当前的主配置文件中

###设置网络连接的序列化
指令格式：accept_mutex on | off;

-该指令默认为 on 状态，表示会对多个 Nginx 进程接收连接进行序列化，防止多个进程对连接的争抢。
说到该指令，首先得阐述一下什么是所谓的 “惊群问题”，可以参考 WIKI 百科的解释。就 Nginx 的场景来解释的话大致的意思就是：当一个新网络连接来到时，多个 worker 进程会被同时唤醒，但仅仅只有一个进程可以真正获得连接并处理之。如果每次唤醒的进程数目过多的话，其实是会影响一部分性能的。

所以在这里，如果 accept_mutex on，那么多个 worker 将是以串行方式来处理，其中有一个 worker 会被唤醒；反之若 accept_mutex off，那么所有的 worker 都会被唤醒，不过只有一个 worker 能获取新连接，其它的 worker 会重新进入休眠状态

这个值的开关与否其实是要和具体场景挂钩的。

### 是否允许同时接收多个网络连接
指令格式：multi_accept on | off;

- 该指令默认为 off 状态，意指每个 worker process 一次只能接收一个新到达的网络连接。若想让每个 Nginx 的 worker process 都有能力同时接收多个网络连接，则需要开启此配置
### 事件驱动模型的选择
指令格式：use model;

- model 模型可选择项包括：select、poll、kqueue、epoll、rtsig 等......
### 最大连接数的配置
指令格式：worker_connections number;

- number 默认值为 512，表示允许每一个 worker process 可以同时开启的最大连接数
###定义 MIME-Type
指令格式：

include mime.types;
default_type mime-type;

- MIME-Type 指的是网络资源的媒体类型，也即前端请求的资源类型
- include 指令将 mime.types 文件包含进来

cat mime.types 来查看 mime.types 文件内容，我们发现其就是一个 types 结构，里面包含了各种浏览器能够识别的 MIME 类型以及对应类型的文件后缀名字，如下所示：

![](http://zdx0122.qiniudn.com/9824247-641526cfc902854c.png)

### 自定义服务日志
指令格式：

access_log path [format];

- path：自定义服务日志的路径 + 名称
- format：可选项，自定义服务日志的字符串格式。其也可以使用 log_format 定义的格式
### 允许 sendfile 方式传输文件
指令格式：

	sendfile on | off;
	sendfile_max_chunk size;
- 前者用于开启或关闭使用 sendfile()传输文件，默认 off
- 后者指令若 size>0，则 Nginx 进程的每个 worker process 每次调用 sendfile()传输的数据了最大不能超出此值；若 size=0 则表示不限制。默认值为 0
###连接超时时间配置
指令格式：keepalive_timeout timeout [header_timeout];

- timeout 表示 server 端对连接的保持时间，默认 75 秒
- header_timeout 为可选项，表示在应答报文头部的 Keep-Alive 域设置超时时间：“ Keep-Alive : timeout = header_timeout ”

### 单连接请求数上限
指令格式：keepalive_requests number;

- 该指令用于限制用户通过某一个连接向 Nginx 服务器发起请求的次数
###配置网络监听
指令格式：

- 第一种：配置监听的 IP 地址：listen IP[:PORT];
- 第二种：配置监听的端口：listen PORT;

实际举例：

	listen 192.168.31.177:8080; # 监听具体 IP 和具体端口上的连接
	listen 192.168.31.177;      # 监听 IP 上所有端口上的连接
	listen 8080;                # 监听具体端口上的所有 IP 的连接
### 基于名称和 IP 的虚拟主机配置
指令格式：server_name name1 name2 ...

- name 可以有多个并列名称，而且此处的 name 支持正则表达式书写

实际举例：

	server_name ~^www\d+\.myserver\.com$
此时表示该虚拟主机可以接收类似域名 www1.myserver.com 等的请求而拒绝 www.myserver.com 的域名请求，所以说用正则表达式可以实现更精准的控制

至于基于 IP 的虚拟主机配置比较简单，不再太赘述：

指令格式：server_name IP 地址

### location 配置
指令格式为：location [ = | ~ | ~* | ^~ ] uri {...}

- 这里的 uri 分为标准 uri 和正则 uri，两者的唯一区别是 uri 中是否包含正则表达式
uri 前面的方括号中的内容是可选项，解释如下：
- “=”：用于标准 uri 前，要求请求字符串与 uri 严格匹配，一旦匹配成功则停止
- “~”：用于正则 uri 前，并且区分大小写
- “~*”：用于正则 uri 前，但不区分大小写
- “^~”：用于标准 uri 前，要求 Nginx 找到标识 uri 和请求字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配

### 请求根目录配置
指令格式：root path;

- path：Nginx 接收到请求以后查找资源的根目录路径
当然，还可以通过 alias 指令来更改 location 接收到的 URI 请求路径，指令为：

	alias path;  # path 为修改后的根路径 
### 设置网站的默认首页
指令格式：index file ......

- file 可以包含多个用空格隔开的文件名，首先找到哪个页面，就使用哪个页面响应请求