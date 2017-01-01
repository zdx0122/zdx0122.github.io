---
layout: post
title:  "全面解析Java注解"
date:   2016-07-03 01
categories: Java
tags:  Java
author: Dex
---

* content
{:toc}





## Java注解概述 ##

表现形式：

	@Override
	protected void onCreated()

	@BeforeTest

	@Test

	@AfterTest

Java注解概念：

Java提供了一种原程序中的元素关联任何信息和任何元数据的途径和方法。

- Java中的常见注解
- 注解分类
- 自定义注解
- 注解应用实战


## Java中的常见注解 ##

JDK自带注解：

    - @Override
    - @Deprecated
    - @Suppvisewarnings

### @Override ###

写一个Person接口：

	package ren.itest;
	
	public interface Person {
		public String name();
		
		public int age();
		
		public void sing();
	
	}

写一个child类，实现Person接口的方法：

	package ren.itest;
	
	public class child implements Person{
	
		@Override
		public String name() {
			// TODO Auto-generated method stub
			return null;
		}
	
		@Override
		public int age() {
			// TODO Auto-generated method stub
			return 0;
		}
	
		@Override
		public void sing() {
			// TODO Auto-generated method stub
			
		}
		
	}

这里，@Override就是覆盖了父类的方法，也是重写；

### @Deprecated 和 @Suppvisewarnings###

	package ren.itest;
	
	public interface Person {
		public String name();
		
		public int age();
		
		@Deprecated
		public void sing();
	
	}

@Deprecated的作用是使其后面的方法或者属性失效。

如果仍然使用这个失效的方法和属性，可使用`@SuppressWarnings("deprecation")`, 表示忽略失效的方法和属性。


## 第三方注解 ##

常见第三方注解：

- Spring
	- @Autowired
	- @Service
	- @Repository

- Mybatis
	- @InsertProvider
	- @UpdateProvider
	- Options

## 注解的分类 ##

- 按照运行机制分类：
	- 源码注解： 注解只在源码中存在，编译成.class文件就不存在了。
	- 编译时注解： 注解在源码和.class文件中都存在。
	- 运行时注解： 在运行阶段还起作用，甚至会影响运行逻辑的注解。

- 按照来源分类：
	- 来自JDK的注解
	- 来自第三方的注解
	- 我们自己定义的注解

- 元注解： 注解的注解


## 自定义注解 ##

### 自定义注解的语法要求 ###

使用`@interface`关键字定义注解;

成员以无参无异常方式声明;

可以用default为成员指定一个默认值;

成员类型是受限的， 合法的类型包括基本类型及String, Class, Annotation, Enumeration.

如果注解只有一个成员， 则成员名必须取名为value(), 在使用时可以忽略成员名和赋值号(=).

注解类可以没有成员， 没有成员的注解称为标识注解.

代码示例：

	public @interface Description {
	
		String desc();
		String author();
		int age() default 18;
	
	}


### 注解的注解(元注解) ###

注解的作用范围：

![注解的作用范围](http://7fvd6e.com1.z0.glb.clouddn.com/Java-%E5%85%83%E6%B3%A8%E8%A7%A31.jpg)

注解的生命周期：

![注解的生命周期](http://7fvd6e.com1.z0.glb.clouddn.com/Java-%E5%85%83%E6%B3%A8%E8%A7%A32.jpg)

![元注解](http://7fvd6e.com1.z0.glb.clouddn.com/Java-%E5%85%83%E6%B3%A8%E8%A7%A33.jpg)

![元注解](http://7fvd6e.com1.z0.glb.clouddn.com/Java-%E5%85%83%E6%B3%A8%E8%A7%A34.jpg)


### 使用自定义注解 ###

使用注解的语法：

@<注解名>(<成员名1> = <成员值1>, <成员名2> = <成员值2>...)

	@Description(desc = "I am eyeColor", author = "Dex", age = 18)
	public String eyeColor() {
		return "red";
	}


### 解析注解 ###

解析注解概念：

通过反射获取类、函数或成员上的运行时注解信息，从而实现动态控制程序运行的逻辑。

