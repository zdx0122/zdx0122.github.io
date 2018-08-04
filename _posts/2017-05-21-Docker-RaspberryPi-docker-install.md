---
layout: post
title:  "树莓派安装Docker"
date:   2017-05-21 01
categories: Docker
tags:  Docker 树莓派
author: i.itest.ren
---

* content
{:toc}

在树莓派内安装Docker。






树莓派默认是安装Raspbian系统。

## step1. 安装好 Raspbian-jessie 后替换源 ##

替换国内 APT 源

将`/etc/apt/sources.list`中的内容替换成 USTC 或 清华的源。这些源都是从官方仓库同步的。

USTC 的 Raspbian 源：

	deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib
	deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main non-free contrib

清华的 Raspbian 源：

	deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ jessie main non-free contrib
	deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ jessie main non-free contrib

西电的 Raspbian 源：

	deb http://ftp.xdlinux.info/raspbian/raspbian/ jessie main non-free contrib
	deb-src http://ftp.xdlinux.info/raspbian/raspbian/ jessie main non-free contrib

替换后`sudo apt-get update`。

## step2. 添加阿里的 docker-project 镜像源 ##

在`/etc/apt/sources.list`里添加

	deb http://mirrors.aliyun.com/docker-engine/apt/repo raspbian-jessie main

## step3. 信任阿里源的公钥 ##

上一步添加阿里源的 docker-project 镜像源地址后，执行：

	$ sudo apt-get update

得到报错信息里提供的公钥，我得到的公钥为`F76221572C52609D`。

添加对该公钥的信任：

	$ gpg --keyserver ha.pool.sks-keyservers.net --recv-keys F76221572C52609D
	$ $ gpg -a --export F76221572C52609D | sudo apt-key add -

这里是以我得到的公钥为例，记住替换成你得到的公钥字串。如果不成功，将`ha.pool.sks-keyservers.NET`依次替换成其他两个网址`pgp.mit.edu`、`keyserver.ubuntu.com`试试。（如果时隔久远，你得看阿里源的脚本里用的是哪个网站）。


## step4. 安装 && 验证 ##

安装：

	$ sudo apt-get install docker-engine

添加当前用户到 docker 用户组：

	$ sudo usermod -aG docker ${USER}

登出系统：

	$ logout

然后再登入系统，验证：

	$ docker version