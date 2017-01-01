---
layout: post
title:  "项目管理利器--Maven"
date:   2016-07-06 01
categories: Java
---






## 唯快不破-maven快速入门 ##

### maven介绍及环境搭建 ###

Maven是基于项目对象模型(POM), 可以通过一小段描述信息来管理项目的构建、报告和文档的软件项目管理工具。

可以帮助我们构建项目，也是一款强大的自动化构建工具，包括编译、复制、打包、运行的过程。

提供了一个仓库的概念，统一的管理第三方的jar包。

官网地址：https://maven.apache.org/

下载地址：https://maven.apache.org/download.cgi

目录介绍：

	- apache-maven-3.3.9
		- bin: 包含mvn的运行脚本，其中m2.conf是配置文件
		- boot: 包含一个类加载器的框架，maven使用此加载类库
		- conf: 配置文件目录，常用到setting.xml
		- lib: 包含maven和使用的第三方类库

配置环境变量：

1. 新建一个系统变量`M2_HOME`, 指向maven安装目录`C:\Java\apache-maven-3.3.9`

2. 修改Path，在最后的位置输入`;%M2_HOME%\bin`

3. 验证是否配置成功，打开cmd命令框，输入`mvn -v`, 显示出了Maven版本即是成功。如下：

	C:\Users\Administrator>mvn -v
	Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:4
	7+08:00)
	Maven home: C:\Java\apache-maven-3.3.9
	Java version: 1.8.0_51, vendor: Oracle Corporation
	Java home: C:\Program Files\Java\jdk1.8.0_51\jre
	Default locale: zh_CN, platform encoding: GBK
	OS name: "windows 7", version: "6.1", arch: "amd64", family: "dos"

## 第一个Maven案例 Hello Maven ##

新建一个Maven项目，目录结构如下：

	- src
		- main
			- java
				- package
		- test
			- java
				- package

流程： 新建项目 -> 编写程序文件和测试文件 -> 编写`pom.xml`文件 -> 根目录下运行 `mvn compile` 编译程序 -> 运行 `mav test` 测试程序 -> 运行 `mvn package` 打包程序


## 稳打稳扎-Maven基础知识 ##

### 常用的构建命令介绍 ###

	mvn -v : 查看Maven版本
	mvn compile : 编译
	mvn test : 测试
	mvn package : 打包，在项目根目录中会产出一个target目录
	mvn clean : 删除target
	mvn install : 安装jar包到本地仓库中

### 自动创建目录骨架 ###

archetype插件：用于创建符合Maven规定的目录骨架

Maven规定目录结构：

	- src
		- main
			- java
				- 主代码

		- test
			- 测试代码

进入项目根目录，执行 `mvn archetype:generate` 

创建目录的两种方式：

1. archetype:generate 按照提示进行选择
2. archetype:generate -DgroupId=z组织名, 公司网址的反写+项目名 -DartifactId=项目名-模块名 -Dversion=版本号 -Dpackage=代码所存在的包名


### Maven中的坐标和仓库 ###

任何一个依赖或者插件都可被**构件**；

构件通过**坐标**作为其唯一标识。

坐标在pom.xml中的代码：

	<dependency>
		<groupId>ren.itest.maven01</groupId>
		<artifactId>maven01-model</artifactId>
		<version>0.0.1SNAPSHOT</version>
	</dependency>

仓库用来管理项目中的依赖；仓库分为本地仓库和远程仓库。

镜像仓库：许多依赖包在国内下载不下来时，可以使用镜像仓库。

在conf\settings.xml中的<mirrors>标签中修改镜像仓库。

### 在Eclipse安装Maven插件以及创建Maven项目 ###

Pass

### Maven的生命周期和插件 ###

完整的项目构建过程包括：

清理、编译、测试、打包、集成测试、验证、部署

Maven生命周期：

- clean 清理项目
	- pre-clean 执行清理前的工作
	- clean 清理上一次构建生成的所有文件
	- post-clean 执行清理后的文件
- default 构建项目(最核心)
	- compile test package install
- site 生成项目站点
	- pre-site 在生成项目站点前要完成的工作
	- site 生成项目的站点文档
	- post-site 在生成项目站点后要完成的工作
	- site-deploy 发布生成的站点到服务器上


### pom.xml常用元素介绍 ###

	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

		<!--指定了当前的pom版本-->
		<modelVersion>4.0.0</modelVersion>
	
		<groupId>反写的公司网址+项目名</groupId>
		<artifactId>项目名+模块名</artifactId>

		<!--第一个0 表示大版本号
			第二个0 表示分支版本号
			第三个0 表示小版本号
			0.0.1
			snapshot快照
			alpha内部测试
			beta公测
			Release稳定
			GA正式发布
		-->
		<version></version>

		<!--默认是jar
			war zip pom
		-->
		<packaging></packaging>
		<!--项目描述名-->
		<name></name>
		<!--项目地址-->
		<url></url>
		<!--项目描述-->
		<description></description>
		<developers></developers>
		<licenses></licenses>
		<organization></organization>

		<dependencies>
			<dependency>
				<groupId></groupId>
				<artifactId></artifactId>
				<version></version>
				<type></type>
				<scope>test</scope>
				<!--设置依赖是否可选-->
				<optional></optional>
				<!--排除依赖传递列表-->
				<exclusions>
					<exclusion></exclusion>
				</exclusions>
			</dependency>
		</dependencies>

		<!--依赖的管理-->
		<dependencyManagement>
			<dependencies>
				<dependency></dependency>
			</dependencies>
		</dependencyManagement>
		
		<build>
			<!--插件列表>
			<plugin>
				<groupId></groupId>
				<artifactId><artifactId>
				<version></version>
			</plugin>
		</build>

		<parent></parent>
		<modules>
			<module></module>
		</modules>

	</project>

		
### 依赖范围 ###

	<scope>test</scope>

- compile : 默认的范围，编译测试运行都有效。
- provided : 在编译和测试时有效。
- runtime : 在测试和运行时有效。
- test : 只在测试时有效。
- system : 与本机系统相关联， 可移植性差。
- import : 导入的范围，它只使用在dependencyManagement中，标识从其它的pom中导入dependency的配置。

### 依赖传递 ###

依赖的pom.xml中加入被依赖的坐标，然后编译，即是依赖的操作步骤；遇到错误的话，要先对被依赖者进行打包，然后再对依赖者清理和编译。

	<dependency>
		<groupId>ren.itest.maven01</groupId>
		<artifactId>maven01-model</artifactId>
		<version>0.0.1SNAPSHOT</version>
	</dependency>

依赖传递类似于面向对象中的继承，例如：Maven项目C依赖项目B， 项目B依赖项目A，则C也依赖A。

如果C不想依赖A，只是依赖B的话，可以在C的pom.xml中加入`<exclusion>A的坐标</exclusion>`


### 依赖冲突 ###

1.短路优先

A -> B -> C -> X(jar)

A -> D ->X(jar) 优先依赖

2.先声明先优先

如果路径长度相同，则谁先声明，先解析谁


### 聚合和继承 ###

在一个新Maven项目的pom.xml中，加入以下标签即是实现聚合：

	<modules>
		<module>../A</module>
		<module>../B</module>
		<module>../C</module>
	</modules>

执行clean 和 install。

在父Maven项目中，定义要被继承引用的变量，在子Maven项目中，使用`<parent>parent的坐标</parent>`，即是继承。

End