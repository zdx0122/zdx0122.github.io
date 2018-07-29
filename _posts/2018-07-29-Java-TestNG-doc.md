---
layout: post
title:  "TestNG官网文档中文版"
date:   2018-07-29 01
categories: Java
tags:  Java TestNG
author: i.itest.ren
---

* content
{:toc}

TestNG是一个Java语言的测试框架，也是QA最常用的测试框架之一。

![TestNG](http://zdx0122.qiniudn.com/intro.png)





TestNG官网文档地址：https://testng.org/doc/documentation-main.html

## 1 - 简介 ##

TestNG是一个测试框架，旨在简化广泛的测试需求，从单元测试（单独测试一个类）到集成测试（测试由几个类，几个包甚至几个外部框架组成的整个系统，如应用服务器）。
编写测试通常需要三个步骤：

- 编写测试的业务逻辑并在代码中插入TestNG注解。
- 在`testng.xml`文件或`build.xml`中添加有关测试的信息（例如，类名，要运行的组等）。
- 运行`TestNG`。
- 
您可以在[欢迎页面](http://testng.org/doc/index.html)上找到一个快速示例。

本文档中使用的概念如下：

- 套件由一个XML文件表示。它可以包含一个或多个测试，并由`<suite>`标记定义。
- 测试由`<test>`表示，可以包含一个或多个`TestNG`类。
- TestNG类是一个包含至少一个TestNG注解的Java类。它由`<class>`标记表示，可以包含一个或多个测试方法。
- 测试方法是代码中的`@Test`注解的Java方法。
- 
可以通过`@BeforeXXX`和`@AfterXXX`注解来配置`TestNG`测试，该注解允许在某个点之前和之后执行某些Java逻辑，这些点是上面列出的项目之一。

本手册的其余部分将解释以下内容：

- 所有注解的列表以及简要说明。这将让您了解TestNG提供的各种功能，但您可能需要查阅专用于每个注解的部分以了解详细信息。
- `testng.xml`文件的说明，语法以及您可以在其中指定的内容。
- 各种功能的详细列表以及如何结合注解和`testng.xml`使用它们。


## 2 - 注解 ##

以下是TestNG中可用注解及其属性的快速概述。

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |