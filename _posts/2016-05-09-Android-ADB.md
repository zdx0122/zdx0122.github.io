---
layout: post
title:  "Android常用adb命令总结"
date:   2016-05-09 01
categories: Android
tags:  Android ADB
author: Dex
---

* content
{:toc}






## Android Debug Bridge ##

adb的全称为Android Debug Bridge，就是起到调试桥的作用。

adb是android sdk里的一个工具, 用这个工具可以直接操作管理android模拟器或者真实的android设备, SDK Tools 目录下的 DDMS、Monitor 等工具，都是同样地用到了 adb 的功能来与 Android 设备进行交互。

adb 使用的端口号是 5037！

## adb命令 ##

使用adb命令前，必须要先配置 Android SDK 环境变量，配置 tools 文件夹环境变量， 配置 platform-tools 文件夹为环境变量。

下面介绍一些常用命令。

### `adb devices` ###

cmd命令行输出如下：

	C:\Users\Administrator>adb devices
	List of devices attached
	* daemon not running. starting it now on port 5037 *
	* daemon started successfully *
	MVBIUSLJSGR49SS4        device

### `adb get-state` ###

cmd命令行输出如下：

	C:\Users\Administrator>adb get-state
	device

设备的状态有三种：device, offline, unknown

- device: 设备正常连接
- offline: 连接出现异常，设备无响应
- unknown: 没有连接设备

### `adb kill-server` 和 `adb start-server` ###

分别是关闭 adb 服务和开启 adb 服务，通常设备连接出现异常时，两者在一起使用；

### `adb logcat` ###

打印日志信息；

可以使用下面的命令打印并把日志写入到指定的文件中，便于查询日志。

`adb logcat > D:\log.txt`

### `adb bugreport` ###

打印dumpsys、dumpstate、logcat的输出，也是用于分析错误

输出内容非常多，建议重定向到一个文件中

	C:\Users\Administrator>adb bugreport > d:\log.txt

结束命令，可以使用`Ctrl c`,就会中断命令。

### `adb install` ###

安装应用，使用 `-r` 参数可以覆盖安装。

	C:\Users\Administrator>adb install -r D:\tieba.apk
	5723 KB/s (38181077 bytes in 6.514s)
	        pkg: /data/local/tmp/tieba.apk
	Success

假如PC连接了多台Android设备，可以使用 `-s` 参数指定设备安装。

    adb -s 你指定的设备名 -r D:\tieba.apk

设备名可以从 `adb devices` 中获取。

### `adb uninstall` ###

卸载应用，后面参数是包名，不是安装包的名称，注意和 `adb install` 区分。

	C:\Users\Administrator>adb uninstall com.baidu.tieba
	Success

包名如何获取？

很多种方法, 说两个常用的：
1. 使用RE管理器，进入/data/app/目录，这里看到的就是你app的包名，使用卸载命令时，需要去除后面的数字和apk后缀名。
2. 问开发人员要
3. ...

### `adb pull` ###

将 Android 设备上的文件或者文件夹复制到本地。

例如，把我手机里的一张相片复制到D盘根目录：

	C:\Users\Administrator>adb pull sdcard/DCIM/Camera/IMG_20160430_123132.jpg D:\
	2765 KB/s (1090377 bytes in 0.385s)

也可以进行重命名操作：

	C:\Users\Administrator>adb pull sdcard/DCIM/Camera/IMG_20160430_123132.jpg d:\zi
	pai.jpg
	4884 KB/s (1090377 bytes in 0.218s)

这里，如果你想复制系统权限目录下的文件，手机需要root! 如果仍然出现权限拒绝的提示，需要使用RE管理器，把目录挂载为可读写，权限设置为777，即可正常进行。

### `adb push` ###

推送本地文件到 Android 设备。

例如：推送刚才的 zipai.jpg 到 sdcard :

	C:\Users\Administrator>adb push D:\zipai.jpg /sdcard/
	4418 KB/s (1090377 bytes in 0.241s)

推送zipai.jpg 到 设备根目录：

	C:\Users\Administrator>adb push D:\zipai.jpg /data/
	failed to copy 'D:\zipai.jpg' to '/data//zipai.jpg': Permission denied
	7551 KB/s (1090377 bytes in 0.141s)

出现权限拒绝的提示，需要设备root，并使用RE管理器，挂载为可读写，权限设置为777，即可正常进行。

	C:\Users\Administrator>adb push D:\zipai.jpg /data/
	6613 KB/s (1090377 bytes in 0.161s)


## adb shell命令 ##

adb命令是adb自带的命令，而adb shell 命令则是调用Android系统中的命令。

这些 Android 特有的命令都放在了 Android 设备的 system/bin 目录下。

### pm ###

#### pm list packages ####

`adb shell pm list packages`: 列出安装在设备上的所有应用的包名

`adb shell pm list packages -s`: 列出系统应用

`adb shell pm list packages -3`: 列出第三方应用

`adb shell pm list packages -f`: 列出应用包名及对应的apk名及存放位置

`adb shell pm list packages -i`: 列出应用包名及其安装来源

命令最后增加filter：过滤关键字，便于查找自己想要的应用信息。

参数可以组合使用，例如，查找第三方应用百度贴吧的包名、apk存放位置、安装来源：

	C:\Users\Administrator>adb shell pm list package -f -3 -i tieba
	package:/data/app/com.baidu.tiebabz-1.apk=com.baidu.tiebabz  installer=null
	package:/data/app/com.baidu.tieba-1.apk=com.baidu.tieba  installer=null

#### pm list features ####

列出所有硬件相关信息

`adb shell pm list features com.baidu.tieba`

#### pm list permissions ####

查看应用所需要的权限信息。

`adb shell pm list permissions com.baidu.tieba`

#### pm path ####

列出对应包名的 .apk 位置

	C:\Users\Administrator>adb shell pm path com.baidu.tieba
	package:/data/app/com.baidu.tieba-1.apk

#### pm list instrumentation ####

列出含有单元测试 case 的应用。后面可跟参数，与pm list package一样。

	C:\Users\Administrator>adb install pm list instrumentation
	Missing APK file

#### pm dump ####

后跟包名，列出指定应用的 dump 信息，里面有各种信息，自行查看。

内容较多，建议使用重定向。

	C:\Users\Administrator>adb shell pm dump com.baidu.tieba > d:\tieba.txt

#### pm install ####

目标 apk 存放于 PC 端，请用 adb install 安装

目标 apk 存放于 Android 设备上，请用 pm install 安装

#### pm uninstall ####

卸载应用，同 adb uninstall , 后面跟的参数都是应用的包名

#### pm clear ####

清除应用数据

#### pm set-install-location , pm get-install-location ####

设置应用安装位置，获取应用安装位置

- [0/auto]：默认为自动
- [1/internal]：默认为安装在手机内部
- [2/external]：默认安装在外部存储

更多命令，请输入 `adb shell pm` 查看帮助信息。

### am ###

使用此命令可以从cmd控制台启动 activity, services；发送 broadcast等等.

#### am start ####

启动一个Activity, 以启动系统相机应用为例：

启动相机：

	C:\Users\Administrator>adb shell am start -n com.android.camera/.Camera
	Starting: Intent { cmp=com.android.camera/.Camera }

先停止目标应用，再启动：

	C:\Users\Administrator>adb shell am start -S com.android.camera/.Camera
	Stopping: com.android.camera
	Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER]     cmp=com.android.camera/.Camera }

等待应用完成启动：

	C:\Users\Administrator>adb shell am start -W com.android.camera/.Camera
	Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.android.camera/.Camera }
	Status: ok
	Activity: com.android.camera/.Camera
	ThisTime: 500
	TotalTime: 500
	Complete

启动默认浏览器打开一个网页：

	C:\Users\Administrator>adb shell am start -a android.intent.action.VIEW -d http://testerhome.com
	Starting: Intent { act=android.intent.action.VIEW dat=http://testerhome.com }

启动拨号器拨打 10086：

	C:\Users\Administrator>adb shell am start -a android.intent.action.CALL -d tel:10086            
	Starting: Intent { act=android.intent.action.CALL dat=tel:xxxxx }

#### am instrument ####

启动一个 instrumentation , 单元测试或者 Robotium 会用到

#### am monitor ####

监控 crash 与 ANR

	C:\Users\Administrator>adb shell am monitor
	Monitoring activity manager...  available commands:
	(q)uit: finish monitoring
	** Activity starting: com.android.camera

#### am force-stop ####

后跟包名，结束应用。

#### am startservice ####

启动一个服务

#### am broadcast ####

发送一个广播

更多 am 命令，可以输入 `adb shell am` 进行查看。

### input ###

这个命令可以向 Android 设备发送按键事件.

#### input text ####

发送文本内容，不能发送中文

	adb shell input text test123456

前提先将键盘设置为英文键盘

#### input keyevent ####

发送按键事件

	adb shell input keyevent KEYCODE_HOME

#### input tap ####

对屏幕发送一个触摸事件

	adb shell input tap 500 500

点击屏幕上坐标为 (500, 500) 的位置

#### input swipe ####

滑动事件

从右往左滑动屏幕:

	adb shell input swipe 900 500 100 500

如果版本不低于 4.4 , 可以模拟长按事件:

	adb shell input swipe 500 500 501 501 2000

其实就是在小的距离内，在较长的持续时间内进行滑动，最后表现出来的结果就是长按动作

到这里会发现，MonkeyRunner 能做到的事情，通过 adb 命令都可以做得到，如果进行封装，会比 MR 做得更好。

### screencap ###

截图命令

截屏，保存至 sdcard 目录:

	adb shell screencap -p /sdcard/screen.png

### screenrecord ###

Android 4.4 新增的录制命令

执行命令后操作手机，ctrl + c 结束录制，录制结果保存至 sdcard：

	adb shell screenrecord sdcard/record.mp4

### uiautomator ###

执行 UI automation tests ， 获取当前界面的控件信息

> runtest：executes UI automation tests RunTestCommand.java
> 
> dump：获取控件信息，DumpCommand.java

	C:\Users\Administrator>adb shell uiautomator dump
	UI hierchary dumped to: /storage/sdcard0/window_dump.xml

不加 [file] 选项时，默认存放在 sdcard 下

### ime ###

列出设备中的输入法：

	C:\Users\Administrator>adb shell ime list -s
	com.sohu.inputmethod.sogouoem/.SogouIME
	io.appium.android.ime/.UnicodeIME

选择一种输入法：

	C:\Users\Administrator>adb shell ime set com.sohu.inputmethod.sogouoem/.SogouIME
	
	Input method com.sohu.inputmethod.sogouoem/.SogouIME selected

### wm ###

获取设备分辨率

	C:\Users\Administrator>adb shell wm size
	Physical size: 720x1280

### log ###

可以在logcat里面打印你设定的信息。

adb shell log -p d -t tieba "test adb shell log"

-p：优先级; -t：tag，标签; 后面加上 message.

### getprop ###

查看 Android 设备的参数信息，只运行 adb shell getprop，结果以 key : value 键值对的形式显示.

获取设备的 sdk 版本:

	adb shell getprop ro.build.version.sdk

### Linux命令 ###

cat, cd, chmod, cp, date, df, du, grep, kill, ln, ls, lsof, netstat, ping, ps, rm, rmdir, top, touch, >(重定向命令), >>(重定向命令), |(管道命令) 等等

END

补充一个引号的用途：

场景1、在 PC 端执行 monkey 命令，将信息保存至 D 盘 monkey.log，会这么写：

	adb shell monkey -p com.android.settings 5000 > d:\monkey.log

场景2、在 PC 端执行 monkey 命令，将信息保存至手机的 Sdcard，可能会这么写：

	adb shell monkey -p com.android.settings 5000 > sdcard/monkey.log

这里肯定会报错，因为最终是写向了 PC 端当前目录的 sdcard 目录下，而非写向手机的 Sdcard

这里需要用上引号：

	adb shell "monkey -p com.android.settings 5000 > sdcard/monkey.log"

对这些命令都熟悉之后，那么接下来就是综合对编程语言的应用，思考如何用语言去处理这些命令，使得这些命令更加的方便于测试工作。

文章出处：

主要内容来自微信公众号alibaba-mqc、搜索引擎搜索结果等，感谢！由本人进行了二次整理，如有侵权，请告知！