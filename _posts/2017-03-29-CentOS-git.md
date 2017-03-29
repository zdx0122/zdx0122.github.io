---
layout: post
title:  "CentOS6.5升级Git版本"
date:   2017-03-29 01
categories: Linux
tags:  Linux Git 
author: Dex
---

* content
{:toc}


CentOS6.5默认的git版本为1.7，yum也最高只能升级到1.7版本。可有些场景需要Git2.0+的版本，如何操作呢？







## 升级缘由 ##
Jenkins中配置git，构建时报错，提示如下：

![](http://zdx0122.qiniudn.com/gitoldversion.png)

检查jenkins机器为CentOS6.5，git版本为1.7.1；

使用yum对git升级：`sudo yum update git`，发现也只能升级到1.7.1。

## Git版本在CentOS6.5中升级步骤 ##

新建目录

	/opt/soft/git

安装依赖的包

	sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
	sudo yum install  gcc perl-ExtUtils-MakeMaker  

删除旧版本的git

	sudo yum remove git

下载新版本git，这里为2.3版本

	wget https://github.com/git/git/archive/v2.3.0.zip

编译git

	unzip v2.3.0.zip

	cd git-2.3.0

	make configure
	./configure --prefix=/opt/soft/git --with-iconv=/usr/local/libiconv

	make prefix=/opt/soft/git all

	make prefix=/opt/soft/git install

把新版本git加入环境变量

	vi addinenv.sh//新建一个shell文件，原因是由于公司机器权限控制，不能直接在bashrc文件中更改内容
		#!/bin/sh
		sudo echo "export PATH=$PATH:/opt/soft/git/bin" >> /etc/bashrc
	
	sudo sh /opt/soft/git/addinenv.sh
	source /etc/bashrc

查看git版本

	$ git --version
	git version 2.3.0

成功！