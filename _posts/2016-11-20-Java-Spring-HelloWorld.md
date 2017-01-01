---
layout: post
title:  "Spring-HelloWorld"
date:   2016-11-20 02
categories: Java
tags:  Java Spring
author: Dex
---

* content
{:toc}





## 第 1 步：创建 Java 项目 ##

Ellipse创建一个Java项目

## 第 2 步：添加必需的库 ##

Build Path -> Configure Build Path

把Spring的jar包和common-logging的jar包添加进来。

## 第 3 步：创建源文件 ##

创建一个love.daojia的包；

创建两个HelloWorld.java 和 MainApp.java 文件。

HelloWorld.java ：

	package love.daojia;
	
	public class HelloWorld {
		private String message;
		public void setMessage(String message){
			this.message = message;
		}
		public void getMessage() {
			System.out.println("Your Message:" + message);
		}
	}

MainApp.java：

	package love.daojia;
	
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;
	
	public class MainApp {
		public static void main(String[] args){
			ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
			HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
			obj.getMessage();
		}
	}

第一步是我们使用框架 API  `ClassPathXmlApplicationContext()` 来创建应用程序的上下文。

这个 API 加载 beans 的配置文件并最终基于所提供的 API，它处理创建并初始化所有的对象，即在配置文件中提到的 beans。

第二步是使用已创建的上下文的 `getBean()` 方法来获得所需的 bean。

这个方法使用 bean 的 ID 返回一个最终可以转换为实际对象的通用对象。一旦有了对象，你就可以使用这个对象调用任何类的方法。

## 第 4 步：创建 bean 的配置文件 ##

创建一个 Bean 的配置文件，该文件是一个 XML 文件，并且作为粘合 bean 的粘合剂即类。这个文件需要在 src 目录下创建.

Beans.xml:

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
	
	<bean id="helloWorld-beanId" class="love.daojia.HelloWorld">
	<property name="message" value="Hello World!"/>
	</bean>
	</beans>

通常开发人员保存该文件的名称为 Beans.xml 文件，但是你可以单独选择你喜欢的任何名称。

你必须确保这个文件在 CLASSPATH 中是可用的，并在主应用程序中使用相同的名称，而在 MainApp.java 文件中创建应用程序的上下文.

Beans.xml 用于给不同的 bean 分配唯一的 ID，并且控制不同值的对象的创建，而不会影响 Spring 的任何源文件。

当 Spring 应用程序被加载到内存中时，框架利用了上面的配置文件来创建所有已经定义的 beans，并且按照标签的定义为它们分配一个唯一的 ID。你可以使用标签来传递在创建对象时使用不同变量的值。

## 第 5 步：运行程序 ##

使用 Ctrl + F11 编译并运行你的应用程序 MainApp.java。

如果你的应用程序一切都正常，将在 Eclipse IDE 控制台打印以下信息：

	十一月 20, 2016 5:15:40 下午 org.springframework.context.support.ClassPathXmlApplicationContext prepareRefresh
	信息: Refreshing org.springframework.context.support.ClassPathXmlApplicationContext@5910e440: startup date [Sun Nov 20 17:15:40 CST 2016]; root of context hierarchy
	十一月 20, 2016 5:15:40 下午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
	信息: Loading XML bean definitions from class path resource [Bean.xml]
	Your Message:Hello World!

祝贺，你已经成功地创建了你的第一个 Spring 应用程序。通过更改 “message” 属性的值并且保持两个源文件不变，你可以看到上述 Spring 应用程序的灵活性。