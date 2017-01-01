---
layout: post
title:  "Docker使用"
date:   2016-10-06 02
categories: Docker
---





## Docker Hello World ##

Docker 允许你在容器内运行应用程序， 使用 `docker run` 命令来在容器内运行一个应用程序。

输出Hello world

	[zdx@localhost ~]$ docker run hello-world
	docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
	See '/usr/bin/docker-current run --help'.
	[zdx@localhost ~]$ sudo docker run hello-world
	[sudo] password for zdx: 
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

