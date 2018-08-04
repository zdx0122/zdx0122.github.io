---
layout: post
title:  "Monkey测试自动抓取Log"
date:   2017-03-29 02
categories: Android
tags:   Android 
author: i.itest.ren
---

* content
{:toc}


跑monkey测试，并自动抓取Error日志。







## 目录结构 ##

- Monkey
	- rizhi				//存放monkey日志
	- MonkeyTest.bat		//monkey脚本命令
	- StartMonkey.bat	//执行monkey自动抓日志测试
	- ErrorLogSave.bat	//抓取Error级别日志

## MonkeyTest.bat ##

	adb shell monkey -v -p com.itest.apptest --ignore-crashes --ignore-timeouts --monitor-native-crashes --ignore-security-exceptions --throttle 500 -s 10 100000 > %monkeylog% && adb reboot


## StartMonkey.bat ##

	set monkeylog=%date:~,4%%date:~5,2%%date:~8,2%_Monkey(HM_note).log
	start MonkeyTest.bat
	set errorlog=%date:~,4%%date:~5,2%%date:~8,2%_ErrorLog(HM_note).log
	start ErrorLogSave.bat

## ErrorLogSave.bat ##

	adb logcat *:E time > %errorlog% &



跑完之后，在rizhi文件夹中，打开日志，搜索`CRASH STACK`，即可找出crash点。