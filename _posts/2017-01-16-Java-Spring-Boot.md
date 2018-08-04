---
layout: post
title:  "SpringBoot"
date:   2017-01-16 12:34:49 +0800
categories: Java
tags:  Java SpringBoot
author: i.itest.ren
---

* content
{:toc}






## SpringBoot 介绍 ##

### SpringBoot 和 SpringMVC的关系 ###

没有必然的联系。

### SpringBoot的特点 ###

- 化繁为简，简化配置
- 备受关注，是下一代框架
- 微服务的入门级微框架

### 注意 ###

- 具备必要的前置知识
	- 利用Maven构建项目
	- Spring注解
	- RESTful API
- 不需要去学SpringMVC
- Java、Maven等版本保持一致


## 第一个SpringBoot程序 ##

建立

阿里云Maven镜像配置

	<mirrors>
		<mirror>
			<id>nexus-aliyun</id>
			<mirrorOf>*</mirrorOf>
			<name>Nexus aliyun</name>
			<url>http://maven.aliyun.com/nexus/content/groups/public</url>
		</mirror>
	</mirrors>

pom.xml依赖：

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

三种启动方式


##项目属性配置 ##


## Controller的使用 ##


## 数据库操作 ##


## 事物管理 ##