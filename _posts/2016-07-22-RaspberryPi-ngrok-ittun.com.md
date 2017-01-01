---
layout: post
title:  "搭建支持外网访问的树莓派Web服务器"
date:   2016-07-22 03
categories: 折腾
tags:  树莓派
author: Dex
---

* content
{:toc}





## 0x00 本文出现的名词 ##

- 树莓派
- php
- nginx
- sqlite
- 域名解析
- 内网映射
- ngrok
- ittun.com

树莓派： 最火的卡片式电脑！麻雀虽小，五脏俱全！功耗低！

本文中使用的树莓派型号为 `raspberry pi 3 B+`

## 0x01 搭建web服务器 ##

考虑到LAMP太耗资源，所以搭建一个`nginx + sqlite + php`的轻量级Web服务器。

参照教程：`http://www.eeboard.com/bbs/thread-27383-1-1.html`

### nginx ###

安装： sudo apt-get install nginx

启动： sudo /etc/init.d/nginx start

nginx的www根目录默认在 /usr/share/nginx/www中

### php 和 sqlite ###

sudo apt-get install php5-fpm php5-sqlite

树莓派本地访问网页正常，即是把Web服务器搭建成功。

## 0x02 内网穿透的缘由 ##

如果树莓派的网络环境是在局域网中，比如像我的：树莓派连接的是路由器的wifi上网，并且路由器的宽带运营商把80端口给封了；那就必须要用到内网穿透！

如何实现内网穿透？

两种方案：

1. 花生壳
2. ngrok

### 花生壳实现内网穿透 ###

官网给的教程通俗易懂： `http://service.oray.com/question/2680.html`

按步骤操作，即可实现内网穿透，不过这里的花生壳内网版是要认证操作的（付费几块钱）。

## 0x03 ngrok神器 ##

ngrok 是一个反向代理，通过在公共的端点和本地运行的 Web 服务器之间建立一个安全的通道。开源！

通过ngrok可以实现内网穿透，不过ngrok是在国外，速度慢，不过有国内的一款，速度快。>>> `ittun.com`

## 0x04 ittun.com搭建ngrok实现内网穿透详细步骤 ##

### 下载客户端文件 ###

可在ittun.com首页中下载对应系统的客户端文件。

树莓派下载`Linux-arm`版本。

### 解压并开始使用 ###

在目录中解压压缩包： `unzip ittun_linuxarm.zip`

cd到解压的目录中，并执行命令：

	cd ittun_linuxarm
	
	./ngrok 80

显示如下：

	ngrok                                                                                                   (Ctrl+C to quit)
	                                                                                                                        
	Tunnel Status                 online                                                                                    
	Version                       1.7/1.7                                                                                   
	Forwarding                    http://eb4c342.ittun.com -> 127.0.0.1:80                                                  
	Forwarding                    https://eb4c342.ittun.com -> 127.0.0.1:80                                                 
	Web Interface                 127.0.0.1:4040                                                                            
	# Conn                        0                                                                                         
	Avg Conn Time                 0.00ms                                                                                    


这时，ngrok生成了一个随机的二级ittun.com域名，访问http://eb4c342.ittun.com或者https://eb4c342.ittun.com即实现了内网穿透！

### 实现自定义的域名访问树莓派Web服务器 ###

在ittun_linuxarm目录中，输入：

	./ngrok -hostname pi.itest.ren 80

然后域名服务器那里，解析自定义域名`CNAME`到`ittun.com`

解析生效后，即可访问 https://pi.itest.ren 

注意： 这里是`https`!如果域名没有备案，只能访问https,如果域名已经备案的话，才可以访问http.

目前，未备案也可以访问http了：

域名解析`CNAME`到`ittun.cn`，重新执行命令,即可实现访问http://pi.itest.ren，不过由于ittun.cn是在海外，所以不稳定且速度慢。

### 利用ngrok同时开通ssh+远程桌面+自定义域名内网映射 ###

在ittun_linuxarm目录中，新增一个yml配置文件：

	sudo vi ittun.yml

yml内容输入如下：

	server_addr: "ittun.com:44433"
	trust_host_root_certs: false
	tunnels:
	  ssh:
	    remote_port: 50001
	    proto:
	      tcp: "127.0.0.1:22"
	  mstsc:
	    remote_port: 50002
	    proto:
	      tcp: "127.0.0.1:3389"
	  web:
	    hostname: "*.itest.ren"
	    proto:
	      https: 80

这里，ssh端口和mstsc端口是自己设置，端口号一定要在50000~59999之间，且不可与其他使用ittun.com的用户设置的端口一致！yml文件内容格式一定不可错误，尤其是空格！

另外，我的网站没有备案，所以在最后的协议那里写的是`https: 80`，如果是备案域名的话，写`http: 80`。

配置完yml文件，执行： `./ngrok -config ittun.yml start ssh mstsc web`

如果没有提示出错，则大功告成！

参考资料：

    http://bbs.ittun.com/forum.php?mod=viewthread&tid=1&extra=page%3D1



示例我在树莓派上搭建的Web服务器： [https://pi.itest.ren/](https://pi.itest.ren/ "https://pi.itest.ren/")

End