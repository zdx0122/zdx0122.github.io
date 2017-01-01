---
layout: post
title:  "Java反射机制"
date:   2016-12-24 01
categories: Java
tags:  Java 反射
author: Dex
---

* content
{:toc}






## 反射的基本概念 ##

如果在正常的情况下，如果要使用一个类，则必须按照如下的步骤操作：

- 使用 import 导入类所在的包；
- 明确的使用类名称或接口名称定义对象；
- 通过关键字 new 进行类对象实例化（构造方法： java.lang.reflect.Constructor）；
- 产生对象可以使用“对象.属性”进行类中属性的调用（属性：java.lang.reflect.Field）;
- 通过“对象.方法()”调用类中方法（方法：java.lang.reflect.Method）;

而反射的过程呢？不需要有明确类型的对象，所有的对象使用Object表示：

- 可以直接使用Object与反射机制的混合调用类中的方法

### Class类 ###

Class类是整个反射操作源头，而这个类的定义如下：

	public final class Class<T>  //反射的泛型几乎无用，使用的时候就用“?”
	extends Object
	implements Serializable, GenericDeclaration, Type, AnnotatedElement

但是如果是想使用Class类进行操作，那么就必须首先产生Class类的实例化对象，而有如下三种方式可以取得Class类的实例化对象：

- 方法一： Object类提供了一个返回Class类对象的方法：public Class<?> getClass();
- 方法二： 利用“类.class”取得，日后见得最多的就是在Hibernate上；
- 方法三： 利用Class类的static方法取得：`public static Class<?> forName(String className) throws ClassNotFoundException`

如果是程序设计的人员，使用最多的方法一定是forName()方法，但是如果是使用者，肯定会使用“类.class”；
工厂设计模式最好利用反射机制解决耦合问题。

请解释Object类之中的所有方法以及每一个方法使用上的注意事项

- 对象克隆：`public Object clone() throws CloneNotSupportedException;`
	- 克隆对象所在的类一定要实现`java.lang.Cloneable`接口，而且子类只需要继续调用Object类的clone()方法就可以成功实现克隆操作;
- 对象输出：`public String toString();`
	- 直接输出对象时会默认调用`toString();`
- 对象比较：`public boolean equals(Object obj);`
	- 当保存Set集合时，会依靠哈市Code() 和 equals()判断是否为重复对象；
- 取得对象的hash码：`public int hashCode();`
	- 可以理解为每一个类对象的唯一编码，比较时会先判断编码是否相同，而后再调用equals()比较；
- 取得Class类对象：`public Class<?> getClass();`
	- 通过一个已经实例化好的对象进行对象的反射操作；
- 线程等待：`public void wait() InterceptorException;`
	- 执行到此代码时县城要等待执行，一直到执行notify()、notifyAll()方法后唤醒；
- 唤醒第一个等待线程：`public void notify();`
- 唤醒全部等待线程：`public void notifyAll();`
- 垃圾回收前释放：`public void finalize() throws Throwable;`
	- 当使用gc回收无用垃圾空间时默认调用；

### 利用反射实例化对象 ###

