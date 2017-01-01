---
layout: post
title:  "Android自动化测试工具-MonkeyRunner"
date:   2016-06-09 01
categories: Android
tags:  Android
author: Dex
---

* content
{:toc}




## MonkeyRunner简介 ##

MonkeyRunner工具提供一套API，可以使用Python语言编写程序，用来控制Android设备或者模拟器。

MonkeyRunner不同于Monkey，Monkey是直接在设备或者模拟器中生成伪随机数的用户和系统事件，MonkeyRunner工具可以发送指定的API指令和事件控制设备和模拟器。

MonkeyRunner工具有以下特征：

- 多设备控制：支持一台或者多台设备执行一个或者多个测试程序。
- 功能测试：提供输入值按键或者触摸事件，并查看结果截图。
- 回归测试：比较输出的一组结果截图。
- 可扩展自动化：MonkeyRunner提供API，用Python语言编写自动化测试脚本。

MonkeyRunner底层是Jython，一个Python使用Java编程语言实现。用Jython你能用python的语法来访问常量，类，和API的方法。

MonkeyRunner是Android SDK tools目录中的一个工具，所以需要配置Android SDK和JDK环境变量。


## 一个简单的程序 ##

导入模块、连接设备、安装apk、启动指定Activity、点击菜单键、截图结果保存。

	# Imports the monkeyrunner modules used by this program
	from com.android.monkeyrunner import MonkeyRunner, MonkeyDevice
	
	# Connects to the current device, returning a MonkeyDevice object
	device = MonkeyRunner.waitForConnection()
	
	# Installs the Android package. Notice that this method returns a boolean, so you can test
	# to see if the installation worked.
	device.installPackage('myproject/bin/MyApplication.apk')
	
	# sets a variable with the package's internal name
	package = 'com.example.android.myapplication'
	
	# sets a variable with the name of an Activity in the package
	activity = 'com.example.android.myapplication.MainActivity'
	
	# sets the name of the component to start
	runComponent = package + '/' + activity
	
	# Runs the component
	device.startActivity(component=runComponent)
	
	# Presses the Menu button
	device.press('KEYCODE_MENU', MonkeyDevice.DOWN_AND_UP)
	
	# Takes a screenshot
	result = device.takeSnapshot()
	
	# Writes the screenshot to a file
	result.writeToFile('myproject/shot1.png','png')


## MonkeyRunner API ##

MonkeyRunner API包（com.android.monkeyrunner） 包含三个模块：

- MonkeyRunner:这个类提供连接设备或模拟器的方法，还提供用于创建一个monkeyrunner程序的用户界面和显示内置帮助的方法。
- MonkeyDevice:标识一个设备或者模拟器。这个类提供了安装和卸载软件包的方法，启动一个活动和发送键盘或触摸事件到应用。你也可以使用这个类来运行测试包。
- MonkeyImage:代表一个屏幕捕获图像。这个类提供了捕捉屏幕的方法，将位图转换成各种格式，比较两个monkeyimage对象，保存图像到文件。

在Python程序中，你访问的每一类作为一个Python模块。monkeyrunner工具不会自动import这些模块。导入一个模块，使用Python`from`声明：

    from com.android.monkeyrunner import MonkeyRunner,MonkeyDevice,MonkeyImage

https://developer.android.com/studio/test/monkeyrunner/index.html#APIClasses

## 运行MonkeyRunner ##

你可以从一个程序文件中运行MonkeyRunner，也可以在命令行中交互运行MonkeyRunner。

MonkeyRunner命令语法：

	monkeyrunner-plugin<plugin_jar><program_filename><program_options>

- -plugin<plugin_jar>: 可选，指定一个MonkeyRunner插件文件。
- <program_filename>: 如果你提供这个参数，这monkeyrunner命令将该文件的内容作为一个Python程序。如果没有提供参数，该命令将启动一个交互式会话。
- <program_options>: 可选，<program_filename>的参数信息。

