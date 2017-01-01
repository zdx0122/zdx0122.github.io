---
layout: post
title:  "Python入门(1)-Python简介"
date:   2016-04-22 01
categories: Python
---


## Python简介 ##

Python（英国发音：/ˈpaɪθən/ 美国发音：/ˈpaɪθɑːn/）, 是一种面向对象、解释型计算机程序设计语言，由Guido van Rossum于1989年发明，第一个公开发行版发行于1991年。

Python的特点：优雅、明确、简单

Python适合的领域：

- Web网站和各种网络服务
- 系统工具和脚本
- 作为“胶水”语言把其他语言开发的模块包装起来方便使用

Python不适合的领域：

- 贴近硬件的代码(首选C)
- 移动开发：IOS/Android有各自的开发语言(Object-C,Swift/Java)
- 游戏开发：C/C++

Python实际应用：

国外：YouTube、Instagram

国内：豆瓣、搜狐邮箱

openstack著名的开源云计算平台

Google、Yahoo、NASA...

都在使用Python

**Python和其他语言对比**

![](http://7fvd6e.com1.z0.glb.clouddn.com/python%E5%AF%B9%E6%AF%94.jpg)

Python源码不能加密！

## 选择Python版本 ##

Python跨平台，可以在Linux、Windows、Mac系统上使用。

Python版本：2.7版、3.5版,两者不兼容

2.7版本支持更多的第三方库。

## Windows上安装 Python ##

官网：www.python.org

下载、双击、下一步安装；

配置环境变量；

在环境变量中，配置在Path中，添加`;C:\Python27`

## 第一个Python程序 ##

    print 'Hello, world!'

保存为hello.py

在cmd命令行中，跳转到hello.py文件目录，执行`python hello.py`。

