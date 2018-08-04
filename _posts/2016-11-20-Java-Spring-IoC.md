---
layout: post
title:  "Spring-IoC容器"
date:   2016-11-20 03
categories: Java
tags:  Java Spring
author: i.itest.ren
---

* content
{:toc}

依赖倒置原则（DIP）：一种软件架构设计的原则（抽象概念）。

控制反转（IoC）：一种反转流、依赖和接口的方式（DIP的具体实现方式）。

依赖注入（DI）：IoC的一种实现方式，用来反转依赖（IoC的具体实现方式）。

IoC容器：依赖注入的框架，用来映射依赖，管理对象创建和生存周期（DI框架）。





## 依赖倒置原则（DIP） ##

> Bob Martins对DIP的定义：
> 
> 高层模块不应依赖于低层模块，两者应该依赖于抽象。
> 
> 抽象不不应该依赖于实现，实现应该依赖于抽象。

相信大部分取过钱的朋友都深有感触，只要有一张卡，随便到哪一家银行的ATM都能取钱。在这个场景中，ATM相当于高层模块，而银行卡相当于低层模块。ATM定义了一个插口（接口），供所有的银行卡插入使用。也就是说，ATM不依赖于具体的哪种银行卡。它只需定义好银行卡的规格参数（接口），所有实现了这种规格参数的银行卡都能在ATM上使用。现实生活如此，软件开发更是如此。依赖倒置原则，它转换了依赖，高层模块不依赖于低层模块的实现，而低层模块依赖于高层模块定义的接口。通俗的讲，就是高层模块定义接口，低层模块负责实现。

场景一  依赖无倒置（低层模块定义接口，高层模块负责实现）

场景二 依赖倒置（高层模块定义接口，低层模块负责实现）

由此，我们可以总结出使用DIP的优点：

- 系统更柔韧：可以修改一部分代码而不影响其他模块。

- 系统更健壮：可以修改一部分代码而不会让系统崩溃。

- 系统更高效：组件松耦合，且可复用，提高开发效率。


## 控制反转（IoC） ##

DIP是一种 软件设计原则，它仅仅告诉你两个模块之间应该如何依赖，但是它并没有告诉如何做。IoC则是一种 软件设计模式，它告诉你应该如何做，来解除相互依赖模块的耦合。控制反转（IoC），它为相互依赖的组件提供抽象，将依赖（低层模块）对象的获得交给第三方（系统）来控制，即依赖对象不在被依赖模块的类中直接通过new来获取。在图1的例子我们可以看到，ATM它自身并没有插入具体的银行卡（工行卡、农行卡等等），而是将插卡工作交给人来控制，即我们来决定将插入什么样的银行卡来取钱。同样我们也通过软件开发过程中场景来加深理解。

> 软件设计原则：原则为我们提供指南，它告诉我们什么是对的，什么是错的。它不会告诉我们如何解决问题。它仅仅给出一些准则，以便我们可以设计好的软件，避免不良的设计。一些常见的原则，比如DRY、OCP、DIP等。
> 
> 软件设计模式：模式是在软件开发过程中总结得出的一些可重用的解决方案，它能解决一些实际的问题。一些常见的模式，比如工厂模式、单例模式等等。

做过电商网站的朋友都会面临这样一个问题：订单入库。假设系统设计初期，用的是SQL Server数据库。通常我们会定义一个SqlServerDal类，用于数据库的读写。

	public class SqlServerDal
	{
	     public void Add()
	    {
	        Console.WriteLine("在数据库中添加一条订单!");
	    }
	}

然后我们定义一个Order类，负责订单的逻辑处理。由于订单要入库，需要依赖于数据库的操作。因此在Order类中，我们需要定义SqlServerDal类的变量并初始化。

	public class Order
	{
	        private readonly SqlServerDal dal = new SqlServerDal();//添加一个私有变量保存数据库操作的对象
	         public void Add()
	       {
	           dal.Add();
	       }
	}

最后，我们写一个控制台程序来检验成果。

	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	namespace DIPTest
	{
	    class Program
	    {
	        static void Main(string[] args)
	        {
	            Order order = new Order();
	            order.Add();
	            Console.Read();
	        }
	    }
	}

输出结果：

	在数据库中添加一条订单!

OK，结果看起来挺不错的！正当你沾沾自喜的时候，这时BOSS过来了。“小刘啊，刚客户那边打电话过来说数据库要改成Access”，“对你来说，应当小CASE啦！”BOSS又补充道。带着自豪而又纠结的情绪，我们思考着修改代码的思路。

由于换成了Access数据库，SqlServerDal类肯定用不了了。因此，我们需要新定义一个AccessDal类，负责Access数据库的操作。

	public class AccessDal
	{
	    public void Add()
	   {
	       Console.WriteLine("在ACCESS数据库中添加一条记录！");
	   }
	}

然后，再看Order类中的代码。由于，Order类中直接引用了SqlServerDal类的对象。所以还需要修改引用，换成AccessDal对象。

	public class Order
	{
	        private readonly AccessDal dal = new AccessDal();//添加一个私有变量保存数据库操作的对象
	         public void Add()
	       {
	           dal.Add();
	       }
	}

输出结果：

	在ACCESS数据库中添加一条记录！

费了九牛二虎之力，程序终于跑起来了！试想一下，如果下次客户要换成MySql数据库，那我们是不是还得重新修改代码？

显然，这不是一个良好的设计，组件之间高度耦合，可扩展性较差，它违背了DIP原则。高层模块Order类不应该依赖于低层模块SqlServerDal，AccessDal，两者应该依赖于抽象。那么我们是否可以通过IoC来优化代码呢？答案是肯定的。IoC有2种常见的实现方式：依赖注入和服务定位。其中，依赖注入使用最为广泛。下面我们将深入理解依赖注入（DI），并学会使用。


## 依赖注入（DI） ##

控制反转（IoC）一种重要的方式，就是将依赖对象的创建和绑定转移到被依赖对象类的外部来实现。在上述的实例中，Order类所依赖的对象SqlServerDal的创建和绑定是在Order类内部进行的。事实证明，这种方法并不可取。既然，不能在Order类内部直接绑定依赖关系，那么如何将SqlServerDal对象的引用传递给Order类使用呢？

