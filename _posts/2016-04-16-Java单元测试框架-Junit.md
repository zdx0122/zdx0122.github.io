---
layout: post
title:  "Java单元测试框架-Junit"
date:   2016-04-16 01
categories: Java
---



## Junit4简介 ##

1.什么是Junit

JUnit is a simple framework to write repeatable tests. It is an instance of the xUnit architecture for unit testing frameworks.

官网： http://junit.org/junit4/

xUnit是一套基于测试驱动开发的测试框架

家族成员有PythonUnit、CppUnit、JUnit。

Junit包含两个jar包，junit.jar和hamcrest-core.jar

Junit3中的所有方法都必须使用@Test注解。

JUnit3必须继承junit.framework.TestCase类，Just4不需要继承任何类。

2.为什么要使用JUnit

测试框架可以帮助我们对编写的程序进行有目的性的测试，能帮助我们最大程序的避免代码出bug，更使程序达到预期的效果。

JUnit使用断言机制，可以将预期结果和程序最终的结果进行比对，确保结果的一致性。

## 如何开发测试用例 ##

右键包，新建JUnit Test Case。

	package junit1;
	
	import static org.junit.Assert.*;
	
	import org.junit.Test;
	
	public class TestJunit {
		@Test
		public void testAdd(){
			String str = "Junit is working fine";
			assertEquals("Junit is working fine", str);
		}
	}

运行，如果是绿色的框，即代表测试通过。

## Junit详解 ##

1. 测试方法上必须使用@Test进行修饰
1. 测试方法必须使用public void进行修饰，不能带任何的参数
1. 新建一个源代码目录来存放我们的测试代码
1. 测试类的包应该和被测试类保持一致
1. 测试单元中的每个方法必须可以独立测试，测试方法间不能有任何的依赖
1. 测试类使用Test作为类名的后缀（不是必须）
1. 测试方法使用test作为方法名的前缀（不是必须）

可以在项目目录中，单独执行一个测试方法。

## 测试失败的两种情况 ##

测试用例用来达到你想要的预期结果，但对于逻辑错误无能为力。

Failures: 一般是指预期结果和实际结果不一致，断言方法判断失败引起的。

Errors: 一般是指程序代码异常，也可能是被测试代码中一个隐藏的bug。

## Junit运行流程 ##

	package com.imooc.util;
	
	import static org.junit.Assert.*;
	
	import org.junit.After;
	import org.junit.AfterClass;
	import org.junit.Before;
	import org.junit.BeforeClass;
	import org.junit.Test;
	
	public class JunitFlowTest {
	
		/*
		 * 1.@BeforeClass修饰的方法会在所有方法被调用前被执行，
		 * 而且该方法是静态的，所以当测试类被加载后接着就会运行它，
		 * 而且在内存中它只会存在一份实例，它比较适合加载配置文件。
		 * 2.@AfterClass所修饰的方法通常用来对资源的清理，如关闭数据库的连接
		 * 3.@Before和@After会在每个测试方法的前后各执行一次。
		 * 
		 */
		@BeforeClass
		public static void setUpBeforeClass() throws Exception {
			System.out.println("this is beforeClass...");
		}
	
		@AfterClass
		public static void tearDownAfterClass() throws Exception {
			System.out.println("this is afterClass...");
		}
	
		@Before
		public void setUp() throws Exception {
			System.out.println("this is before...");
		}
	
		@After
		public void tearDown() throws Exception {
			System.out.println("this is after");
		}
	
		@Test
		public void test1() {
			System.out.println("this is test1...");
		}
		
		@Test
		public void test2(){
			System.out.println("this is test2...");
		}
	
	}


## Junit中的常用注解 ##

- @Test: 将一个普通方法修饰成为一个测试方法
- @BeforeClass: 它会在所有的方法运行前被执行，static修饰
- @AfterClass: 它会在所有的方法运行结束后被执行，static修饰
- @Before: 会在每一个测试方法被运行前执行
- @After: 会在每一个测试方法运行后执行
- @Ignore: 所修饰的测试方法会被测试运行器忽略
- @RunWith: 可以更改测试运行器 org.junit.runner.Runner

官方文档： http://junit.org/junit4/javadoc/latest/index.html

	package com.imooc.util;
	
	import static org.junit.Assert.assertEquals;
	
	import org.junit.Ignore;
	import org.junit.Test;
	
	public class AnotationTest {
	
		/*
		 * @Test:将一个普通的方法修饰成为一个测试方法
		 * 		@Test(expected=XX.class)
		 * 		@Test(timeout=毫秒 )
		 * @BeforeClass：它会在所有的方法运行前被执行，static修饰
		 * @AfterClass:它会在所有的方法运行结束后被执行，static修饰
		 * @Before：会在每一个测试方法被运行前执行一次
		 * @After：会在每一个测试方法运行后被执行一次
		 * @Ignore:所修饰的测试方法会被测试运行器忽略
		 * @RunWith:可以更改测试运行器 org.junit.runner.Runner
		 */
		
		@Test(expected=ArithmeticException.class)
		public void testDivide() {
			assertEquals(3, new Calculate().divide(6, 0));
		}
		
		@Ignore("...")
		@Test(timeout=2000)
		public void testWhile() {
			while(true) {
				System.out.println("run forever...");
			}
		}
		
		@Test(timeout=3000)
		public void testReadFile(){
			try {
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}


## JUnit测试套件的使用 ##

	package com.imooc.util;
	
	import static org.junit.Assert.*;
	
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.junit.runners.Suite;
	
	@RunWith(Suite.class)
	@Suite.SuiteClasses({TaskTest1.class,TaskTest2.class,TaskTest3.class})
	public class SuiteTest {
		/*
		 * 1.测试套件就是组织测试类一起运行的
		 * 
		 * 写一个作为测试套件的入口类，这个类里不包含其他的方法
		 * 更改测试运行器Suite.class
		 * 将要测试的类作为数组传入到Suite.SuiteClasses（{}）
		 */
	
	}

上面的TaskTest1.class,TaskTest2.class,TaskTest3.class是三个测试用例。

## JUnit参数化设置 ##

	package com.imooc.util;
	
	import static org.junit.Assert.*;
	
	import java.util.Arrays;
	import java.util.Collection;
	
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.junit.runners.Parameterized;
	import org.junit.runners.Parameterized.Parameters;
	
	@RunWith(Parameterized.class)
	public class ParameterTest {
		/*
		 * 1.更改默认的测试运行器为RunWith(Parameterized.class)
		 * 2.声明变量来存放预期值 和结果值
		 * 3.声明一个返回值 为Collection的公共静态方法，并使用@Parameters进行修饰
		 * 4.为测试类声明一个带有参数的公共构造函数，并在其中为之声明变量赋值
		 */
		int expected =0;
		int input1 = 0;
		int input2 = 0;
		
		@Parameters
		public static Collection<Object[]> t() {
			return Arrays.asList(new Object[][]{
					{3,1,2},
					{4,2,2}
			}) ;
		}
		
		public ParameterTest(int expected,int input1,int input2) {
			this.expected = expected;
			this.input1 = input1;
			this.input2 = input2;
		}
		
		@Test
		public void testAdd() {
			assertEquals(expected, new Calculate().add(input1, input2));
		}
	
	}

