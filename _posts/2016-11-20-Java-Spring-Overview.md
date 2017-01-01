---
layout: post
title:  "Spring概述"
date:   2016-11-20 01
categories: Java
tags:  Java Spring
author: Dex
---

* content
{:toc}

Spring 框架是一个开源的 Java 平台，它为容易而快速的开发出耐用的 Java 应用程序提供了全面的基础设施。





## 概述 ##

### 前言 ###

Spring 框架是一个开源的 Java 平台，它为容易而快速的开发出耐用的 Java 应用程序提供了全面的基础设施。

Spring 框架最初是由 Rod Johnson 编写的，并且 2003 年 6 月首次在 Apache 2.0 许可下发布。

Spring 是最受欢迎的企业级 Java 应用程序开发框架。数以百万的来自世界各地的开发人员使用 Spring 框架来
创建好性能、易于测试、可重用的代码。

Spring 框架是一个开源的 Java 平台，它最初是由 Rod Johnson 编写的，并且 2003 年 6 月首次在 Apache
2.0 许可下发布。

当谈论到大小和透明度时， Spring 是轻量级的。 Spring 框架的基础版本是在 2 MB 左右的。

Spring 框架的核心特性可以用于开发任何 Java 应用程序，但是在 Java EE 平台上构建 web 应用程序是需要扩
展的。 

Spring 框架的目标是使 J2EE 开发变得更容易使用，通过启用基于 POJO 编程模型来促进良好的编程实
践。

### 使用Spring 框架的好处 ###

下面列出的是使用 Spring 框架主要的好处：
• Spring 可以使开发人员使用 POJOs 开发企业级的应用程序。只使用 POJOs 的好处是你不需要一个 EJB
容器产品，比如一个应用程序服务器，但是你可以选择使用一个健壮的 servlet 容器，比如 Tomcat 或者一
些商业产品。

• Spring 在一个单元模式中是有组织的。即使包和类的数量非常大，你必须并且只需要但是你需要的，而忽略
剩余的那部分。

• Spring 不会让你白费力气坐重复工作，它真正的利用了一些现有的技术，像几个 ORM 框架、日志框架、J
EE、Quartz 和 JDK 计时器，其他视图技术。

• 测试一个用 Spring 编写的应用程序很容易，因为 environment-dependent 代码被放进了这个框架中。此
外，通过使用 JavaBean-style POJOs，它在使用依赖注入注入测试数据时变得更容易。

• Spring 的 web 框架是一个设计良好的 web MVC 框架，它为 web 框架，比如 Structs 或者其他工程上的
或者很少受欢迎的 web 框架，提供了一个很好的供替代的选择。

• 为将特定技术的异常（例如，由 JDBC、Hibernate，或者 JDO 抛出的异常）翻译成一致的， Spring 提供
了一个方便的 API，而这些都是未经检验的异常。

• 轻量级的 IOC 容器往往是轻量级的，例如，特别是当与 EJB 容器相比的时候。这有利于在内存和 CPU 资
源有限的计算机上开发和部署应用程序。

• Spring 提供了一个一致的事务管理界面，该界面可以缩小成一个本地事务（例如，使用一个单一的数据
库）和扩展成一个全局事务（例如，使用 JTA）。


## 依赖注入(DI) ##

Spring 最认同的技术是控制反转的 依赖注入（DI）模式。

到底什么是依赖注入？让我们将这两个词分开来看一看。这里将依赖关系部分转化为两个类之间的关联。例
如，类 A 依赖于类 B。现在，让我们看一看第二部分，注入。所有这一切都意味着类 B 将通过 IoC 被注入到类
A 中。

依赖注入可以以向构造函数传递参数的方式发生，或者通过使用 setter 方法 post-construction。

### 面向方面的程序设计（AOP） ###

Spring 框架的一个关键组件是 面向方面的程序设计（AOP）框架。一个程序中跨越多个点的功能被称为 横切关
注点，这些横切关注点在概念上独立于应用程序的业务逻辑。有各种各样常见的很好的关于方面的例子，比如日
志记录、声明性事务、安全性，和缓存等等。

在 OOP 中模块化的关键单元是类，而在 AOP 中模块化的关键单元是方面。AOP 帮助你将横切关注点从它们所
影响的对象中分离出来，然而依赖注入帮助你将你的应用程序对象从彼此中分离出来。

Spring 框架的 AOP 模块提供了面向方面的程序设计实现，允许你定义拦截器方法和切入点，可以实现将应该被分开的代码干净的分开功能。


## 体系结构 ##

Spring 有可能成为所有企业应用程序的一站式服务点，然而，Spring 是模块化的，允许你挑选和选择适用于你的模块，不必要把剩余部分也引入。

Spring 框架提供约 20 个模块，可以根据应用程序的要求来使用。

![](http://zdx0122.qiniudn.com/SpringFramework.png)

### 核心容器 ###

核心容器由核心，Bean，上下文和表达式语言模块组成，它们的细节如下：

• 核心模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。

• Bean 模块提供 BeanFactory，它是一个工厂模式的复杂实现。

• 上下文模块建立在由核心和 Bean 模块提供的坚实基础上，它是访问定义和配置的任何对象的媒介。ApplicationContext 接口是上下文模块的重点。

• 表达式语言模块在运行时提供了查询和操作一个对象图的强大的表达式语言。

### 数据访问/集成 ###

数据访问/集成层包括 JDBC，ORM，OXM，JMS 和事务处理模块，它们的细节如下：

• JDBC 模块提供了删除冗余的 JDBC 相关编码的 JDBC 抽象层。

• ORM 模块为流行的对象关系映射 API，包括 JPA，JDO，Hibernate 和 iBatis，提供了集成层。

• OXM 模块提供了抽象层，它支持对 JAXB，Castor，XMLBeans，JiBX 和 XStream 的对象/XML 映射实现。

• Java 消息服务  JMS 模块包含生产和消费的信息的功能。

• 事务模块为实现特殊接口的类及所有的 POJO 支持编程式和声明式事务管理。

### Web ###

Web 层由 Web，Web-MVC，Web-Socket 和 Web-Portlet 组成，它们的细节如下：

• Web 模块提供了基本的面向 web 的集成功能，例如多个文件上传的功能和使用 servlet 监听器和面向 web应用程序的上下文来初始化 IoC 容器。

• Web-MVC 模块包含 Spring 的模型-视图-控制器（MVC），实现了 web 应用程序。

• Web-Socket 模块为 WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。

• Web-Portlet 模块提供了在 portlet 环境中实现 MVC，并且反映了 Web-Servlet 模块的功能。

### 其他 ###

还有其他一些重要的模块，像 AOP，Aspects，Instrumentation，Web 和测试模块，它们的细节如下：

• AOP 模块提供了面向方面的编程实现，允许你定义方法拦截器和切入点对代码进行干净地解耦，它实现了应该分离的功能。

• Aspects 模块提供了与 J AspectJ 的集成，这是一个功能强大且成熟的面向切面编程（AOP）框架。

• Instrumentation 模块在一定的应用服务器中提供了类 instrumentation 的支持和类加载器的实现。

• Messaging 模块为 STOMP 提供了支持作为在应用程序中 WebSocket 子协议的使用。它也支持一个注解编程模型，它是为了选路和处理来自 WebSocket 客户端的 STOMP 信息。

• 测试模块支持对具有 JUnit 或 TestNG 框架的 Spring 组件的测试。

## 环境设置 ##

第 1 步：安装 Java 开发工具包（JDK）

第 2 步：安装 Apache Commons Logging API

http://commons.apache.org/logging/ 下载 Apache Commons Logging API 的最新版本。

确保你在这个目录上正确的设置 CLASSPATH 变量，否则你将会在运行应用程序时遇到问题。

第 3 步：安装 Eclipse IDE

第 4 步：安装 Spring 框架库

http://repo.spring.io/release/org/springframework/spring 下载最新版本的 Spring 框架的二进制文
件。