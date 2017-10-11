---
layout: post
title:  "Python的国内第三方镜像源"
date:   2017-06-05 01
categories: Python
tags:  Python 
author: Dex
---

* content
{:toc}

虽然用easy_install和pip来安装第三方库很方便 
它们的原理其实就是从Python的官方源pypi.python.org/pypi 下载到本地，然后解包安装。 
不过因为某些原因，访问官方的pypi不稳定，很慢甚至有些还时不时的访问不了。

跟ubuntu的apt和centos的yum有各个镜像源一样，pypi也有。









虽然用easy_install和pip来安装第三方库很方便 
它们的原理其实就是从Python的官方源pypi.python.org/pypi 下载到本地，然后解包安装。 
不过因为某些原因，访问官方的pypi不稳定，很慢甚至有些还时不时的访问不了。

跟ubuntu的apt和centos的yum有各个镜像源一样，pypi也有。

在国内的强烈推荐豆瓣的源 

	http://pypi.douban.com/simple/

还有阿里的源：

	 https://mirrors.aliyun.com/pypi/simple/




注意后面要有/simple目录。

使用镜像源很简单，用`-i`指定就行了： 

	sudo easy_install -i http://pypi.douban.com/simple/ saltTesting 
	sudo pip install -i http://pypi.douban.com/simple/ saltTesting 

或者sudo pip install 文件名.whl

- 首先试着在pip在终端安装，如果下载过慢，把pip下载的官方文件名记下来；
- 然后在豆瓣Python镜像源中寻找，Ctrl+F快速找到，并下载下来
- 在下载的文件夹中打开终端，输入sudo pip install 文件名.whl
- 注意安装包依赖和先后安装顺序。


pypi国内镜像目前有：
 
	http://pypi.douban.com/  豆瓣
	http://pypi.hustunique.com/  华中理工大学
	http://pypi.sdutlinux.org/  山东理工大学
	http://pypi.mirrors.ustc.edu.cn/  中国科学技术大学