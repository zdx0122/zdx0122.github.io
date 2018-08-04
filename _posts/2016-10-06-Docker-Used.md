---
layout: post
title:  "Docker使用"
date:   2016-10-06 02
categories: Docker
tags:  Docker
author: i.itest.ren
---

* content
{:toc}

Docker使用入门





## 镜像 ##

### 获取镜像 ###

Docker 运行容器前需要本地存在对应的镜像，如果镜像不存在本地，Docker 会从镜像仓库下载（默认是 Docker Hub 公共注册服务器中的仓库）。

可以使用 `docker pull` 命令来从仓库获取所需要的镜像。

从 Docker Hub 仓库下载一个 Ubuntu 12.04 操作系统的镜像：

	$ sudo docker pull ubuntu:12.04
	Pulling repository ubuntu
	ab8e2728644c: Pulling dependent layers
	511136ea3c5a: Download complete
	5f0ffaa9455e: Download complete
	a300658979be: Download complete
	904483ae0c30: Download complete
	ffdaafd1ca50: Download complete
	d047ae21eeaf: Download complete

下载过程中，会输出获取镜像的每一层信息。

该命令实际上相当于 `$ sudo docker pull registry.hub.docker.com/ubuntu:12.04` 命令，即从注册服务器 `registry.hub.docker.com 中的 ubuntu` 仓库来下载标记为 `12.04` 的镜像。

有时候官方仓库注册服务器下载较慢，可以从其他仓库下载。 从其它仓库下载时需要指定完整的仓库注册服务器地址。例如

	$ sudo docker pull dl.dockerpool.com:5000/ubuntu:12.04
	Pulling dl.dockerpool.com:5000/ubuntu
	ab8e2728644c: Pulling dependent layers
	511136ea3c5a: Download complete
	5f0ffaa9455e: Download complete
	a300658979be: Download complete
	904483ae0c30: Download complete
	ffdaafd1ca50: Download complete
	d047ae21eeaf: Download complete

完成后，即可随时使用该镜像了，例如创建一个容器，让其中运行 bash 应用。

	$ sudo docker run -t -i ubuntu:12.04 /bin/bash
	root@fe7fc4bd8fc9:/#

### 镜像列表 ###

使用 `docker images` 显示本地已有的镜像。

	[zdx@localhost run]$ sudo docker images
	REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
	docker.io/hello-world       latest              c54a2cc56cbb        7 months ago        1.848 kB
	docker.io/training/webapp   latest              6fae60ef3446        20 months ago       348.7 MB

在列出信息中，可以看到几个字段信息

- REPOSTITORY：表示镜像的仓库源
- TAG：镜像的标签
- IMAGE ID：镜像ID
- CREATED：镜像创建时间
- SIZE：镜像大小

其中镜像的 ID 唯一标识了镜像，注意到 ubuntu:14.04 和 ubuntu:trusty 具有相同的镜像 ID，说明它们实际上是同一镜像。

TAG 信息用来标记来自同一个仓库的不同镜像。例如 ubuntu 仓库中有多个镜像，通过 TAG 信息来区分发行版本，例如 10.04、12.04、12.10、13.04、14.04 等。例如下面的命令指定使用镜像 ubuntu:14.04 来启动一个容器。

如果不指定具体的标记，则默认使用 latest 标记信息。

### 创建镜像 ###

创建镜像有很多方法，用户可以从 Docker Hub 获取已有镜像并更新，也可以利用本地文件系统创建一个。

