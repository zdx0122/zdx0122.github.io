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

### 注解： BeforeXXX & AfterXXX ###

TestNG类的配置信息： 

- `@BeforeSuite`：在此套件中的所有测试运行之前，将运行带注解的方法。 
- `@AfterSuite`：在此套件中的所有测试运行后，将运行带注解的方法。 
- `@BeforeTest`：在运行属于`<test>`标记内的类的任何测试方法之前，将运行带注解的方法。 
- `@AfterTest`：在运行了属于`<test>`标记内的类的所有测试方法之后，将运行带注解的方法。 
- `@BeforeGroups`：此配置方法之前将运行的组列表。保证在调用属于任何这些组的第一个测试方法之前不久运行此方法。 
- `@AfterGroups`：此配置方法将在之后运行的组列表。保证在调用属于任何这些组的最后一个测试方法后不久运行此方法。 
- `@BeforeClass`：在调用当前类中的第一个测试方法之前，将运行带注解的方法。 
- `@AfterClass`：在运行当前类中的所有测试方法之后，将运行带注解的方法。 
- `@BeforeMethod`：带注解的方法将在每个测试方法之前运行。 
- `@AfterMethod`：带注解的方法将在每个测试方法之后运行。

**TestNG类的超类中的注解行为**

当放置在TestNG类的超类上时，上述注解也将被兑现（继承）。例如，这对于在公共超类中集中多个测试类的测试设置非常有用。

在这种情况下，TestNG保证“`@Before`”方法以继承顺序执行（首先是最高超类，然后是继承链），而“`@After`”方法则按相反的顺序执行（上传继承链）。

#### Before和After注解的参数 ####

- `alwaysRun`	对于before方法（`beforeSuite`，`beforeTest`，`beforeTestClass`和`beforeTestMethod`，但不是`beforeGroups`）：如果设置为`true`，则无论它属于哪个组，都将运行此配置方法。 
对于`after`方法（`afterSuite`，`afterClass`，...）：如果设置为`true`，即使先前调用的一个或多个方法失败或被跳过，也将运行此配置方法。
- `dependsOnGroups`	此方法所依赖的组列表。
- `dependsOnMethods`	此方法所依赖的方法列表。
- 启用	是否启用此类/方法上的方法。
- 组	此类/方法所属的组列表。
- `inheritGroups`	如果为true，则此方法将属于类级别的@Test注解中指定的组。
- `onlyForGroups`	仅适用于@BeforeMethod和@AfterMethod。如果指定，则仅当相应的测试方法属于列出的组之一时，才会调用此setup / teardown方法。

### @DataProvider ###

将方法标记为为测试方法提供数据。带注解的方法必须返回一个`Object[][]`，其中每个`Object[]`都可以分配测试方法的参数列表。想要从此`DataProvider`接收数据的`@Test`方法需要使用`dataProvider`名称等于此注解的名称。

#### @DataProvider的参数 ####

- `name` 此数据提供者的名称。如果未提供，则此data provider的名称将自动设置为其方法的名称。
- `parallel` 如果设置为true，则使用此数据提供程序生成的测试将并行运行。默认值为false。

