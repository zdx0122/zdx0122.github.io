---
layout: post
title:  "Bash教程"
date:   2017-09-10 01
categories: Linux
tags:  Linux Shell
author: Dex
---

* content
{:toc}

Bash == Bourne Again shell

Shell有很多种，⽐如ksh，csh,bsh等。我们使⽤的是bash，也是使⽤最多的。

Shell是⼀种解释性的语⾔，适⽤于基本的逻辑处理和不追求速度的应⽤。







## 查看本机⽀持的shell ##

	pi@raspberrypi:~ $ more /etc/shells
	# /etc/shells: valid login shells
	/bin/sh
	/bin/dash
	/bin/bash
	/bin/rbash


## 变量 ##

变量不需要定义，可以直接使⽤。

### 定义变量 ###

`var=“xxx”` `=` 左右不要有空格。需要的时候，必须⽤`”`或
者`’`括起来。