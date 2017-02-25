---
layout: post
title:  "Android电量测试：battery-historian"
date:   2017-02-21 01
categories: Android
tags:  Android 电量测试
author: Dex
---

* content
{:toc}

简介：Battery historian是一款通过上传bugreport文件分析用户手机中App的电池耗电情况的工具。




## 搭建battery-historian ##

- 安装JDK
- 安装Python
- 安装Go语言
	- 配置GOROOT和GOPATH
		- GOROOT：Go的安装根目录
		- GOPATH：Go的工程目录
	- 配置环境变量
		- 把Go的bin目录路径加入Path
	- go version
- 安装git

### 下载源码 ###

打开git bash:

	daojia@daojia-PC MINGW64 ~
	$ go get -d -u github.com/google/battery-historian/...

找到battery-historian的git目录

	daojia@daojia-PC MINGW64 /e/WorkSpace/GoWorkSpace/src/github.com/google/battery-historian (master)
	$ pwd
	/e/WorkSpace/GoWorkSpace/src/github.com/google/battery-historian

下载：

	daojia@daojia-PC MINGW64 /e/WorkSpace/GoWorkSpace/src/github.com/google/battery-historian (master)
	$ go run setup.go
	
	Downloading Closure library...
	
	Downloading Closure compiler...
	
	Downloading 3rd-party JS files...
	
	Generating JS runfiles...
	
	Generating optimized JS runfiles...

启动： `go run cmd/battery-historian/battery-historian.go`

	daojia@daojia-PC MINGW64 /e/WorkSpace/GoWorkSpace/src/github.com/google/battery-historian (master)
	$ go run cmd/battery-historian/battery-historian.go
	2017/02/21 14:00:54 Listening on port:  9999
	2017/02/21 14:00:59 Trace starting analysisServer processing for: GET
	2017/02/21 14:00:59 Trace finished analysisServer processing for: GET

浏览器打开http:localhost:9999

![](http://zdx0122.qiniudn.com/battery-historian-index.png)


## 电量测试 ##

### 运行 adb bugreport ###

连接手机，运行命令：`adb bugreport > C:\Users\daojia\bugreport.txt`

进行操作手机App等，运行一段时间后（看自己的运行场景而定），结束上面的命令（Ctrl + C）.

### 上传bugreport.txt文件 ###

把得到的bugreport.txt上传到battery-historian，结果如下：

![](http://zdx0122.qiniudn.com/battery-historian-bugreport.png)


官网文档：https://github.com/google/battery-historian