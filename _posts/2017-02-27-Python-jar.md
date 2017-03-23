---
layout: post
title:  "Python操作jar包"
date:   2017-02-27 01
categories: Python
tags:  Python Java
author: Dex
---

* content
{:toc}

在Python语言中，也可以使用jar包！




## 准备工作 ##

- Python
- jpype模块
- jdk

## 思路 ##

1. 在python中调用随意调用jar内的方法
1. 使用python中jpype模块作为桥梁，自己访问jar内的方法

代码：

	#incoding:utf-8
	 
	from jpype import *
	 
	import os.path
	 
	jvmpath=getDefaultJVMPath()
	 
	jarpath=os.path.join(os.path.abspath('.'),'E:/')#本地jar路径
	 
	startJVM(jvmpath,'-ea','-Djava.class.path=%s'%(jarpath+'jmeter.utils.jar'))#调用jar包
	 
	JDClass=JClass('com.xxx.xxx.app.jmeter.utils.httpEncode')#package name.class name
	 
	jd=JDClass()#new class
	 
	print jd.getEncodeUrl('sid=77777&orderid=88888&refer=1234&code=0&yesorno=1&batchorder=0')
	 
	shutdownJVM()


