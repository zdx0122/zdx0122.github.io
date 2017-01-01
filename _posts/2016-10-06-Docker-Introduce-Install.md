---
layout: post
title:  "Docker介绍及安装"
date:   2016-10-06 01
categories: Docker
---





## Docker 概述 ##

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从Apache2.0协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

### Docker的应用场景 ###

- Web 应用的自动化打包和发布。
- 自动化测试和持续集成、发布。
- 在服务型环境中部署和调整数据库或其他的后台应用。
- 从头编译或者扩展现有的OpenShift或Cloud Foundry平台来搭建自己的PaaS环境。

### Docker 的优点 ###

1、简化程序：

Docker 让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，便可以实现虚拟化。Docker改变了虚拟化的方式，使开发者可以直接将自己的成果放入Docker中进行管理。方便快捷已经是 Docker的最大优势，过去需要用数天乃至数周的任务，在Docker容器的处理下，只需要数秒就能完成。

2、避免选择恐惧症：

如果你有选择恐惧症，还是资深患者。Docker 帮你打包你的纠结！比如 Docker 镜像；Docker 镜像中包含了运行环境和配置，所以 Docker 可以简化部署多种应用实例工作。比如 Web 应用、后台应用、数据库应用、大数据应用比如 Hadoop 集群、消息队列等等都可以打包成一个镜像部署。

3、节省开支：

一方面，云计算时代到来，使开发者不必为了追求效果而配置高额的硬件，Docker 改变了高性能必然高价格的思维定势。Docker 与云的结合，让云空间得到更充分的利用。不仅解决了硬件管理的问题，也改变了虚拟化的方式。

相关链接:

- Docker 官网：http://www.docker.com
- Github Docker 源码：https://github.com/docker/docker


## Docker 架构 ##

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。

Docker 容器通过 Docker 镜像来创建。

容器与镜像的关系类似于面向对象编程中的对象与类。

![](http://zdx0122.qiniudn.com/Docker-object.png)

![](http://zdx0122.qiniudn.com/576507-docker1.png)

- Docker 镜像(Images)
	- Docker 镜像是用于创建 Docker 容器的模板。
- Docker 容器(Container)
	- 容器是独立运行的一个或一组应用。
- Docker 客户端(Client)
	- Docker 客户端通过命令行或者其他工具使用 Docker API (https://docs.docker.com/reference/api/docker_remote_api) 与 Docker 的守护进程通信。
- Docker 主机(Host)
	- 一个物理或者虚拟的机器用于执行 Docker 守护进程和容器。
- Docker 仓库(Registry)
	- Docker 仓库用来保存镜像，可以理解为代码控制中的代码仓库。
	- Docker Hub(https://hub.docker.com) 提供了庞大的镜像集合供使用。
- Docker Machine
	- Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。


## Docker 安装 ##

### CentOS Docker 安装 ###

Docker支持以下的CentOS版本：

- CentOS 7 (64-bit)
- CentOS 6.5 (64-bit) 或更高的版本

前提条件：

- 目前，CentOS 仅发行版本中的内核支持 Docker。
- Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。
- Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。

使用 yum 安装（CentOS 7下）

Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。

通过 `uname -r` 命令查看你当前的内核版本

	[zdx@localhost ~]$ uname -r
	3.10.0-327.el7.x86_64

安装 Docker
Docker 软件包和依赖包已经包含在默认的 CentOS-Extras 软件源里，安装命令如下：

	[root@localhost ~]# yum -y install docker
	已加载插件：fastestmirror
	Loading mirror speeds from cached hostfile
	 * base: mirrors.yun-idc.com
	 * extras: mirrors.neusoft.edu.cn
	 * updates: mirrors.neusoft.edu.cn
	正在解决依赖关系
	--> 正在检查事务
	---> 软件包 docker.x86_64.0.1.10.3-46.el7.centos.14 将被 安装
	--> 正在处理依赖关系 docker-common = 1.10.3-46.el7.centos.14，它被软件包 docker-1.10.3-46.el7.centos.14.x86_64 需要
	--> 正在处理依赖关系 oci-systemd-hook >= 1:0.1.4-4，它被软件包 docker-1.10.3-46.el7.centos.14.x86_64 需要
	--> 正在处理依赖关系 oci-register-machine >= 1:0-1.8，它被软件包 docker-1.10.3-46.el7.centos.14.x86_64 需要
	--> 正在处理依赖关系 docker-selinux >= 1.10.3-46.el7.centos.14，它被软件包 docker-1.10.3-46.el7.centos.14.x86_64 需要
	--> 正在处理依赖关系 libseccomp.so.2()(64bit)，它被软件包 docker-1.10.3-46.el7.centos.14.x86_64 需要
	--> 正在检查事务
	---> 软件包 docker-common.x86_64.0.1.10.3-46.el7.centos.14 将被 安装
	---> 软件包 docker-selinux.x86_64.0.1.10.3-46.el7.centos.14 将被 安装
	--> 正在处理依赖关系 policycoreutils-python，它被软件包 docker-selinux-1.10.3-46.el7.centos.14.x86_64 需要
	---> 软件包 libseccomp.x86_64.0.2.2.1-1.el7 将被 安装
	---> 软件包 oci-register-machine.x86_64.1.0-1.8.gitaf6c129.el7 将被 安装
	---> 软件包 oci-systemd-hook.x86_64.1.0.1.4-4.git41491a3.el7 将被 安装
	--> 正在处理依赖关系 libyajl.so.2()(64bit)，它被软件包 1:oci-systemd-hook-0.1.4-4.git41491a3.el7.x86_64 需要
	--> 正在检查事务
	---> 软件包 policycoreutils-python.x86_64.0.2.2.5-20.el7 将被 安装
	--> 正在处理依赖关系 libsemanage-python >= 2.1.10-1，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 audit-libs-python >= 2.1.3-4，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 python-IPy，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libselinux-python，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libqpol.so.1(VERS_1.4)(64bit)，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libqpol.so.1(VERS_1.2)(64bit)，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libcgroup，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libapol.so.4(VERS_4.0)(64bit)，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 checkpolicy，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libqpol.so.1()(64bit)，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	--> 正在处理依赖关系 libapol.so.4()(64bit)，它被软件包 policycoreutils-python-2.2.5-20.el7.x86_64 需要
	---> 软件包 yajl.x86_64.0.2.0.4-4.el7 将被 安装
	--> 正在检查事务
	---> 软件包 audit-libs-python.x86_64.0.2.4.1-5.el7 将被 安装
	---> 软件包 checkpolicy.x86_64.0.2.1.12-6.el7 将被 安装
	---> 软件包 libcgroup.x86_64.0.0.41-8.el7 将被 安装
	---> 软件包 libselinux-python.x86_64.0.2.2.2-6.el7 将被 安装
	---> 软件包 libsemanage-python.x86_64.0.2.1.10-18.el7 将被 安装
	---> 软件包 python-IPy.noarch.0.0.75-6.el7 将被 安装
	---> 软件包 setools-libs.x86_64.0.3.3.7-46.el7 将被 安装
	--> 解决依赖关系完成
	
	依赖关系解决
	
	===============================================================================================================================================================================================
	 Package                                              架构                                 版本                                                     源                                    大小
	===============================================================================================================================================================================================
	正在安装:
	 docker                                               x86_64                               1.10.3-46.el7.centos.14                                  extras                               9.5 M
	为依赖而安装:
	 audit-libs-python                                    x86_64                               2.4.1-5.el7                                              base                                  69 k
	 checkpolicy                                          x86_64                               2.1.12-6.el7                                             base                                 247 k
	 docker-common                                        x86_64                               1.10.3-46.el7.centos.14                                  extras                                61 k
	 docker-selinux                                       x86_64                               1.10.3-46.el7.centos.14                                  extras                                79 k
	 libcgroup                                            x86_64                               0.41-8.el7                                               base                                  64 k
	 libseccomp                                           x86_64                               2.2.1-1.el7                                              base                                  49 k
	 libselinux-python                                    x86_64                               2.2.2-6.el7                                              base                                 247 k
	 libsemanage-python                                   x86_64                               2.1.10-18.el7                                            base                                  94 k
	 oci-register-machine                                 x86_64                               1:0-1.8.gitaf6c129.el7                                   extras                               1.1 M
	 oci-systemd-hook                                     x86_64                               1:0.1.4-4.git41491a3.el7                                 extras                                27 k
	 policycoreutils-python                               x86_64                               2.2.5-20.el7                                             base                                 435 k
	 python-IPy                                           noarch                               0.75-6.el7                                               base                                  32 k
	 setools-libs                                         x86_64                               3.3.7-46.el7                                             base                                 485 k
	 yajl                                                 x86_64                               2.0.4-4.el7                                              base                                  39 k
	
	事务概要
	===============================================================================================================================================================================================
	安装  1 软件包 (+14 依赖软件包)
	
	总下载量：12 M
	安装大小：54 M
	Downloading packages:
	(1/15): audit-libs-python-2.4.1-5.el7.x86_64.rpm                                                                                                                        |  69 kB  00:00:00     
	(2/15): libcgroup-0.41-8.el7.x86_64.rpm                                                                                                                                 |  64 kB  00:00:00     
	(3/15): docker-selinux-1.10.3-46.el7.centos.14.x86_64.rpm                                                                                                               |  79 kB  00:00:00     
	(4/15): docker-common-1.10.3-46.el7.centos.14.x86_64.rpm                                                                                                                |  61 kB  00:00:00     
	(5/15): libseccomp-2.2.1-1.el7.x86_64.rpm                                                                                                                               |  49 kB  00:00:00     
	(6/15): checkpolicy-2.1.12-6.el7.x86_64.rpm                                                                                                                             | 247 kB  00:00:01     
	(7/15): oci-systemd-hook-0.1.4-4.git41491a3.el7.x86_64.rpm                                                                                                              |  27 kB  00:00:00     
	(8/15): libsemanage-python-2.1.10-18.el7.x86_64.rpm                                                                                                                     |  94 kB  00:00:01     
	(9/15): python-IPy-0.75-6.el7.noarch.rpm                                                                                                                                |  32 kB  00:00:00     
	(10/15): libselinux-python-2.2.2-6.el7.x86_64.rpm                                                                                                                       | 247 kB  00:00:01     
	(11/15): yajl-2.0.4-4.el7.x86_64.rpm                                                                                                                                    |  39 kB  00:00:00     
	(12/15): oci-register-machine-0-1.8.gitaf6c129.el7.x86_64.rpm                                                                                                           | 1.1 MB  00:00:02     
	(13/15): policycoreutils-python-2.2.5-20.el7.x86_64.rpm                                                                                                                 | 435 kB  00:00:02     
	(14/15): setools-libs-3.3.7-46.el7.x86_64.rpm                                                                                                                           | 485 kB  00:00:03     
	(15/15): docker-1.10.3-46.el7.centos.14.x86_64.rpm                                                                                                                      | 9.5 MB  00:00:19     
	-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
	总计                                                                                                                                                           654 kB/s |  12 MB  00:00:19     
	Running transaction check
	Running transaction test
	Transaction test succeeded
	Running transaction
	  正在安装    : libsemanage-python-2.1.10-18.el7.x86_64                                                                                                                                   1/15 
	  正在安装    : audit-libs-python-2.4.1-5.el7.x86_64                                                                                                                                      2/15 
	  正在安装    : yajl-2.0.4-4.el7.x86_64                                                                                                                                                   3/15 
	  正在安装    : 1:oci-systemd-hook-0.1.4-4.git41491a3.el7.x86_64                                                                                                                          4/15 
	  正在安装    : 1:oci-register-machine-0-1.8.gitaf6c129.el7.x86_64                                                                                                                        5/15 
	  正在安装    : libseccomp-2.2.1-1.el7.x86_64                                                                                                                                             6/15 
	  正在安装    : python-IPy-0.75-6.el7.noarch                                                                                                                                              7/15 
	  正在安装    : checkpolicy-2.1.12-6.el7.x86_64                                                                                                                                           8/15 
	  正在安装    : libcgroup-0.41-8.el7.x86_64                                                                                                                                               9/15 
	  正在安装    : libselinux-python-2.2.2-6.el7.x86_64                                                                                                                                     10/15 
	  正在安装    : docker-common-1.10.3-46.el7.centos.14.x86_64                                                                                                                             11/15 
	  正在安装    : setools-libs-3.3.7-46.el7.x86_64                                                                                                                                         12/15 
	  正在安装    : policycoreutils-python-2.2.5-20.el7.x86_64                                                                                                                               13/15 
	  正在安装    : docker-selinux-1.10.3-46.el7.centos.14.x86_64                                                                                                                            14/15 
	  正在安装    : docker-1.10.3-46.el7.centos.14.x86_64                                                                                                                                    15/15 
	  验证中      : setools-libs-3.3.7-46.el7.x86_64                                                                                                                                          1/15 
	  验证中      : docker-common-1.10.3-46.el7.centos.14.x86_64                                                                                                                              2/15 
	  验证中      : docker-1.10.3-46.el7.centos.14.x86_64                                                                                                                                     3/15 
	  验证中      : libselinux-python-2.2.2-6.el7.x86_64                                                                                                                                      4/15 
	  验证中      : libcgroup-0.41-8.el7.x86_64                                                                                                                                               5/15 
	  验证中      : 1:oci-systemd-hook-0.1.4-4.git41491a3.el7.x86_64                                                                                                                          6/15 
	  验证中      : checkpolicy-2.1.12-6.el7.x86_64                                                                                                                                           7/15 
	  验证中      : python-IPy-0.75-6.el7.noarch                                                                                                                                              8/15 
	  验证中      : libseccomp-2.2.1-1.el7.x86_64                                                                                                                                             9/15 
	  验证中      : 1:oci-register-machine-0-1.8.gitaf6c129.el7.x86_64                                                                                                                       10/15 
	  验证中      : docker-selinux-1.10.3-46.el7.centos.14.x86_64                                                                                                                            11/15 
	  验证中      : yajl-2.0.4-4.el7.x86_64                                                                                                                                                  12/15 
	  验证中      : audit-libs-python-2.4.1-5.el7.x86_64                                                                                                                                     13/15 
	  验证中      : policycoreutils-python-2.2.5-20.el7.x86_64                                                                                                                               14/15 
	  验证中      : libsemanage-python-2.1.10-18.el7.x86_64                                                                                                                                  15/15 
	
	已安装:
	  docker.x86_64 0:1.10.3-46.el7.centos.14                                                                                                                                                      
	
	作为依赖被安装:
	  audit-libs-python.x86_64 0:2.4.1-5.el7                          checkpolicy.x86_64 0:2.1.12-6.el7                           docker-common.x86_64 0:1.10.3-46.el7.centos.14                   
	  docker-selinux.x86_64 0:1.10.3-46.el7.centos.14                 libcgroup.x86_64 0:0.41-8.el7                               libseccomp.x86_64 0:2.2.1-1.el7                                  
	  libselinux-python.x86_64 0:2.2.2-6.el7                          libsemanage-python.x86_64 0:2.1.10-18.el7                   oci-register-machine.x86_64 1:0-1.8.gitaf6c129.el7               
	  oci-systemd-hook.x86_64 1:0.1.4-4.git41491a3.el7                policycoreutils-python.x86_64 0:2.2.5-20.el7                python-IPy.noarch 0:0.75-6.el7                                   
	  setools-libs.x86_64 0:3.3.7-46.el7                              yajl.x86_64 0:2.0.4-4.el7                                  
	
	完毕！

启动 Docker 后台服务

	[root@localhost ~]# service docker start
	Redirecting to /bin/systemctl start  docker.service

测试运行 hello-world

	[root@localhost ~]# docker run hello-world
	Unable to find image 'hello-world:latest' locally
	Trying to pull repository docker.io/library/hello-world ... 
	latest: Pulling from docker.io/library/hello-world
	c04b14da8d14: Pull complete 
	Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
	Status: Downloaded newer image for docker.io/hello-world:latest
	
	Hello from Docker!
	This message shows that your installation appears to be working correctly.
	
	To generate this message, Docker took the following steps:
	 1. The Docker client contacted the Docker daemon.
	 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
	 3. The Docker daemon created a new container from that image which runs the
	    executable that produces the output you are currently reading.
	 4. The Docker daemon streamed that output to the Docker client, which sent it
	    to your terminal.
	
	To try something more ambitious, you can run an Ubuntu container with:
	 $ docker run -it ubuntu bash
	
	Share images, automate workflows, and more with a free Docker Hub account:
	 https://hub.docker.com
	
	For more examples and ideas, visit:
	 https://docs.docker.com/engine/userguide/

由于本地没有hello-world这个镜像，所以会下载一个hello-world的镜像，并在容器内运行。

#### 使用脚本安装 Docker ####

1、使用 sudo 或 root 权限登录 Centos。

2、确保 yum 包更新到最新。

	$ sudo yum update

3、执行 Docker 安装脚本。

	$ curl -fsSL https://get.docker.com/ | sh

执行这个脚本会添加 docker.repo 源并安装 Docker。

4、启动 Docker 进程。

	$ sudo service docker start

5、验证 docker 是否安装成功并在容器中执行一个测试的镜像。

	$ sudo docker run hello-world

到此，docker 在 CentOS 系统的安装完成。


### Ubuntu Docker 安装 ###

Docker 支持以下的 Ubuntu 版本：

- Ubuntu Precise 12.04 (LTS)
- Ubuntu Trusty 14.04 (LTS)
- Ubuntu Wily 15.10
- 其他更新的版本……

前提条件

Docker 要求 Ubuntu 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的 Ubuntu 版本是否支持 Docker。

通过 `uname -r` 命令查看你当前的内核版本

#### 使用脚本安装 Docker ####

	wget -qO- https://get.docker.com/ | sh

当要以非root用户可以直接运行docker时，需要执行 sudo usermod -aG docker runoob 命令，然后重新登陆，否则会有报错.

启动docker 后台服务

	sudo service docker start

测试运行hello-world

	docker run hello-world

### Windows Docker 安装 ###

Docker 引擎使用的是 Linux 内核特性，所以我们需要在 Windows 上使用一个轻量级的虚拟机 (VM) 来运行 Docker。

我们通过 `Boot2Docker` 来安装虚拟机和运行 Docker

安装

1、安装Boot2Docker 

最新版 Boot2Docker 下载地址： https://github.com/boot2docker/windows-installer/releases/latest

目前最新版为v1.8.0, 下载地址为： https://github.com/boot2docker/windows-installer/releases/download/v1.8.0/docker-install.exe

2、运行安装文件

运行安装文件，它将会安装 virtualbox、MSYS-git boot2docker Linux 镜像和 Boot2Docker 的管理工具。

安装三步走直至安装完成。

从桌面上或者 Program Files 中运行 `Boot2Docker Start`。

Boot2Docker Start 将启动一个 Unix shell 来配置和管理运行在虚拟主机中的 Docker，我们可以通过运行 `docker version` 来查看它是否正常工作。

运行 Docker

使用`boot2docker.exe` `ssh` 连接到虚拟主机上，然后执行`docker run hello-world`

到此，docker在Windows系统的安装完成。