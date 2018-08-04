---
layout: post
title:  "Docker实例"
date:   2016-10-06 03
categories: Docker
tags:  Docker
author: i.itest.ren
---

* content
{:toc}

Docker实例





## 运行一个web应用 ##

我们将在docker容器中运行一个 Python Flask 应用来运行一个web应用。

	[zdx@localhost run]$ sudo docker run -d -P training/webapp python app.py
	[sudo] password for zdx: 
	Unable to find image 'training/webapp:latest' locally
	Trying to pull repository docker.io/training/webapp ... 
	latest: Pulling from docker.io/training/webapp
	e190868d63f8: Pull complete 
	909cd34c6fd7: Pull complete 
	0b9bfabab7c1: Pull complete 
	a3ed95caeb02: Pull complete 
	10bbbc0fc0ff: Pull complete 
	fca59b508e9f: Pull complete 
	e7ae2541b15b: Pull complete 
	9dd97ef58ce9: Pull complete 
	a4c1b0cb7af7: Pull complete 
	Digest: sha256:06e9c1983bd6d5db5fba376ccd63bfa529e8d02f23d5079b8f74a616308fb11d
	Status: Downloaded newer image for docker.io/training/webapp:latest
	da22a36ebc4b777ac42ceffadabffc6a41f35cd272b8334e91925b41f000815c

参数说明:

- -d:让容器在后台运行。
- -P:将容器内部使用的网络端口映射到我们使用的主机上。

### 查看WEB应用容器 ###

使用 docker ps 来查看我们正在运行的容器

	[zdx@localhost run]$ sudo docker ps
	[sudo] password for zdx: 
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                     NAMES
	da22a36ebc4b        training/webapp     "python app.py"     5 minutes ago       Up 5 minutes        0.0.0.0:32768->5000/tcp   pensive_jennings

这里多出了端口信息：

	PORTS
	0.0.0.0:32768->5000/tcp

Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 32769 上。
这时我们可以通过浏览器访问WEB应用(192.168.127.128:5000)，可以看到页面打开后，展示着Hello world。

我们也可以指定 `-p` 标识来绑定指定端口。

	docker run -d -p 5000:5000 training/webapp python app.py

docker ps查看正在运行的容器

	[zdx@localhost run]$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                              NAMES
	0633e1a4363e        training/webapp     "python app.py"     2 minutes ago       Up 2 minutes        0.0.0.0:5000->5000/tcp             determined_hodgkin

容器内部的 5000 端口映射到我们本地主机的 5000 端口上。


### 网络端口的快捷方式 ###

通过`docker ps` 命令可以查看到容器的端口映射，docker还提供了另一个快捷方式：`docker port`,使用 `docker port` 可以查看指定 （ID或者名字）容器的某个确定端口映射到宿主机的端口号。

上面我们创建的web应用容器ID为:`0633e1a4363e` 名字为：`determined_hodgkin`
我可以使用`docker port 0633e1a4363e` 或`docker port determined_hodgkin`来查看容器端口的映射情况。

### 查看WEB应用程序日志 ###

`docker logs [ID或者名字]` 可以查看容器内部的标准输出。

	[zdx@localhost run]$ sudo docker logs -f 0633e1a4363e
	 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
	192.168.127.1 - - [31/Jan/2017 11:27:17] "GET / HTTP/1.1" 200 -
	192.168.127.1 - - [31/Jan/2017 11:27:54] "GET / HTTP/1.1" 200 -
	192.168.127.1 - - [31/Jan/2017 11:27:55] "GET / HTTP/1.1" 200 -
	192.168.127.1 - - [31/Jan/2017 11:27:55] "GET / HTTP/1.1" 200 -
	192.168.127.1 - - [31/Jan/2017 13:19:26] "GET / HTTP/1.1" 200 -

`-f`:让 `dokcer logs` 像使用 `tail -f` 一样来输出容器内部的标准输出。

从上面，我们可以看到应用程序使用的是 5000 端口并且能够查看到应用程序的访问日志。

### 查看WEB应用程序容器的进程 ###

我们还可以使用 `docker top` 来查看容器内部运行的进程

	[zdx@localhost run]$ sudo docker top 0633e1a4363e
	UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
	root                3288                2321                0                   19:26               ?                   00:00:05            python app.py

### 检查WEB应用程序 ###

使用 `docker inspect` 来查看Docker的底层信息。它会返回一个 JSON 文件记录着 Docker 容器的配置和状态信息。

	[zdx@localhost run]$ sudo docker inspect 0633e1a4363e
	[
	    {
	        "Id": "0633e1a4363e8a22910b3d54cc9e964b2e7d3afb29afac52e40226f29a7ed266",
	        "Created": "2017-01-31T11:26:42.78896343Z",
	        "Path": "python",
	        "Args": [
	            "app.py"
	        ],
	        "State": {
	            "Status": "running",
	            "Running": true,
	            "Paused": false,
	            "Restarting": false,
	            "OOMKilled": false,
	            "Dead": false,
	            "Pid": 3288,
	            "ExitCode": 0,
	            "Error": "",
	            "StartedAt": "2017-01-31T11:26:44.472080518Z",
	            "FinishedAt": "0001-01-01T00:00:00Z"
	        },
	        "Image": "sha256:6fae60ef344644649a39240b94d73b8ba9c67f898ede85cf8e947a887b3e6557",
	        "ResolvConfPath": "/var/lib/docker/containers/0633e1a4363e8a22910b3d54cc9e964b2e7d3afb29afac52e40226f29a7ed266/resolv.conf",
	        "HostnamePath": "/var/lib/docker/containers/0633e1a4363e8a22910b3d54cc9e964b2e7d3afb29afac52e40226f29a7ed266/hostname",
	        "HostsPath": "/var/lib/docker/containers/0633e1a4363e8a22910b3d54cc9e964b2e7d3afb29afac52e40226f29a7ed266/hosts",

### 停止WEB应用容器 ###

	[zdx@localhost run]$ sudo docker stop 0633e1a4363e
	0633e1a4363e

### 重启WEB应用容器 ###

已经停止的容器，我们可以使用命令 `docker start` 来启动。

	[zdx@localhost run]$ sudo docker restart 0633e1a4363e
	0633e1a4363e

查看正在运行的容器：

	[zdx@localhost run]$ sudo docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
	0633e1a4363e        training/webapp     "python app.py"     2 hours ago         Up 59 seconds       0.0.0.0:5000->5000/tcp   determined_hodgkin

正在运行的容器，我们可以使用 `docker restart` 命令来重启。

### 移除WEB应用容器 ###

我们可以使用 `docker rm` 命令来删除不需要的容器

	[zdx@localhost run]$ sudo docker rm loving_darwin
	loving_darwin

删除容器时，容器必须是停止状态，否则会报如下错误：

	[zdx@localhost run]$ sudo docker rm 0633e1a4363e
	Failed to remove container (0633e1a4363e): Error response from daemon: Conflict, You cannot remove a running container. Stop the container before attempting removal or use -f
