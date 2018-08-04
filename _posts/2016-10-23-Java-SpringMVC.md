---
layout: post
title:  "SpringMVC起步"
date:   2016-10-23 01
categories: Java
tags:  Java SpringMVC
author: i.itest.ren
---

* content
{:toc}

MVC的核心思想是业务数据抽取同业务数据呈现相分离





## MVC 简介 ##

### 前端控制器 ###

![FrontController](http://zdx0122.qiniudn.com/FrontController.png)

MVC的本质

- MVC的核心思想是业务数据抽取同业务数据呈现相分离

### MVC 概念 ###

Model-View-Controller

#### View ####

视图层：为用户提供UI，重点关注数据的呈现。

#### Model ####

模型层：业务数据的信息支撑，关注支撑业务的信息构成，通常是多个业务实体的组合。

#### Controller ####

控制层：调用业务逻辑产生合适的数据（Model），传递数据给视图层用于呈现。

#### 什么是MVC ####

- MVC是一种架构模式
	- 程序分层，分工合作，既相互独立，又协同工作。

- MVC是一种思考方式
	- 需要将什么信息展示给用户？如何布局？调用哪些业务逻辑？

## Spring MVC基本概念 ##

### Spring MVC 的静态概念 ###

- DispatcherServlet：
	- ![DispatcherServlet](http://zdx0122.qiniudn.com/DispatcherServlet.jpg)

- Controller：
	- ![Controller](http://zdx0122.qiniudn.com/Controller.jpg)

- HandlerAdapter：
	- ![HandlerAdapter](http://zdx0122.qiniudn.com/HandlerAdapter.jpg)

- HandlerInterceptor：
	- 一个接口

- HandlerMapping：
	- 1. HelpDispatcherServlet to get the right controller.
	- 2. Wrap controller with HandlerInterceptor.

- HandlerExecutionChain：
	- ![](http://zdx0122.qiniudn.com/ExecutionChain.jpg)

- ModelAndView：

- ViewResolver：
	- Help DispatcherServlet to Resolve the right View to render page.

- View
	- V in MVC
	- Responsible for page rendering

### Spring MVC 的动态概念 ###

![](http://zdx0122.qiniudn.com/SpringMVC%E5%8A%A8%E6%80%81%E6%A6%82%E5%BF%B5.jpg)

![](http://zdx0122.qiniudn.com/SpringMVC%E5%8A%A8%E6%80%81%E6%A6%82%E5%BF%B52.jpg)

## 配置Maven环境 ##

### Maven 介绍 ###

#### POM ####

- Project Object Model
- An xml file (pom.xml)
- Contains information
	- dependencies, developers, organizatin, licenses ...

#### Dependency ####

依赖

![](http://zdx0122.qiniudn.com/Dependency.jpg)

- /WEB-INF/lib：项目依赖的jar包目录
	- 一个项目依赖很多jar包，某些jar包仍然会依赖其他jar包 依赖的深度加深。。。
	- 用Maven就可以解决此种问题

#### Coordinates ####

坐标：唯一标识一个产品（包）

![](http://zdx0122.qiniudn.com/Coordinates.jpg)

### Maven 安装 ###

- 下载并解压Maven
- 配置环境变量（M2_HOME， Path）
- 配置Maven配置文件（本地仓库路径，镜像）


