---
layout: post
title:  "设计模式-单例模式"
date:   2016-07-09 02
categories: Java
tags:  Java
author: i.itest.ren
---

* content
{:toc}





## 设计模式简介 ##

什么是设计模式？

设计模式（Design Pattern）是一套反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。

目的： 使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。

设计模式有多少种？

- 基本模式有23种：
	- 单例模式
	- 抽象工厂模式
	- 建造者模式
	- 工厂模式
	- 原型模式
	- ...

## 单例模式简介 ##

古代只能有一个皇帝，多了会出问题；现代只能拥有一个老婆，多了会出问题；

有些对象我们只需要一个，比如：配置文件、工具类、线程池、缓存、日志对象等。

如果创造出多个实例，就会导致许多问题，比如占用过多资源，不一致的结果等。

如何保证整个应用中某个实例或者对象有且只有一个呢？可以通过单例模式来实现！

常用的单例模式分为懒汉模式和饿汉模式。

## 单例模式的饿汉式实现 ##

重点是让构造方法私有化、实例私有静态化、提供一个公开静态的获取实例方法，保证实例有且只有一个。

只要类加载，实例就会加载，但没有吃饱，需要早些吃饱，形象的称之为饿汉模式。

### 0x01 ###

Singleton.java

	package com.itest.designpattern;
	/*
	 *单例模式Singleton
	 *应用场合：有些对象只需要一个就足够了，如古代皇帝、老婆
	 *作用：保证整个应用程序中某个实例有且只有一个
	 *类型：饿汉模式、懒汉模式
	 */
	public class Singleton {
		//1. 将构造方法私有化， 不允许外部直接创建对象
		private Singleton() {
	
		}
		//2. 创建类的唯一实例
		static Singleton instance = new Singleton();
		}
	}

test.java

	package com.itest.designpattern;
	
	public class test {
	
		public static void main(String[] args) {
			Singleton s1 = Singleton.instance;
			Singleton s2 = Singleton.instance;
			if (s1 == s2) {
				System.out.println("s1和s2是同一个实例");
			}else {
				System.out.println("s1和s2不是同一个实例");
			}
	
		}
	
	}

运行结果：`s1和s2是同一个实例`

### 0x02 ###

Singleton.java

	package com.itest.designpattern;
	/*
	 *单例模式Singleton
	 *应用场合：有些对象只需要一个就足够了，如古代皇帝、老婆
	 *作用：保证整个应用程序中某个实例有且只有一个
	 *类型：饿汉模式、懒汉模式
	 */
	public class Singleton {
		//1. 将构造方法私有化， 不允许外部直接创建对象
		private Singleton() {
	
		}
		//2. 创建类的唯一实例，使用private static修饰
		private static Singleton instance = new Singleton();
		
		//3. 提供一个用于获取实例的方法，使用public static修饰
		public static Singleton getInstance() {
			return instance;
		}
	}

test.java

	package com.itest.designpattern;
	
	public class test {
	
		public static void main(String[] args) {
			Singleton s1 = Singleton.getInstance();
			Singleton s2 = Singleton.getInstance();
			if (s1 == s2) {
				System.out.println("s1和s2是同一个实例");
			}else {
				System.out.println("s1和s2不是同一个实例");
			}
	
		}
	
	}

运行结果：`s1和s2是同一个实例`


## 单例模式的懒汉式实现 ##

在类加载的时候，实例并未加载，只会在第一次获取实例后，实例才会加载；后面的再次加载的实例就直接使用已加载的实例。相较于第一种模式，比较懒，所以形象的命名为懒汉模式。

Singleton2.java

	package com.itest.designpattern;
	/*
	 * 懒汉模式
	 */
	
	public class Singleton2 {
		//1. 将构造方法私有化，不允许外部直接创建对象
		private Singleton2() {
			
		}
		//2. 声明类的唯一实例，使用private static修饰
		private static Singleton2 instance;
		
		//3. 提供一个获取实例的方法，使用public static修饰
		public static Singleton2 getInstance() {
			if(instance == null){
				instance = new Singleton2();
			}
			return instance;
		}
	}

test.java

	package com.itest.designpattern;
	
	public class test {
	
		public static void main(String[] args) {
			Singleton s3 = Singleton.getInstance();
			Singleton s4 = Singleton.getInstance();
			if (s1 == s2) {
				System.out.println("s3和s4是同一个实例");
			}else {
				System.out.println("s3和s4不是同一个实例");
			}
	
		}
	
	}

运行结果：`s3和s4是同一个实例`


## 饿汉式 PK 懒汉式 ##

饿汉模式的特点是加载类时比较慢，但运行时获取对象的速度比较快。线程安全。

懒汉模式的特点是加载类时比较快，因为相对来说，在加载类时，并未加载类的对象，所以加载比较快。但是运行时获取对象的速度比较慢，因为在用户第一次获取的时候，由于类并没有去创建，它需要一个类创建的过程，所以相对来说速度较慢。线程不安全。

