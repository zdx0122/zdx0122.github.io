---
layout: post
title:  "Java测试框架-TestNG入门"
date:   2016-05-06 01
categories: Java
tags:  Java 单元测试 TestNG
author: Dex
---

* content
{:toc}




## TestNG简介 ##

TestNG即Testing,Next Generation,下一代测试技术，是一套根据JUnit和NUint思想而创建的利用注释来强化测试功能的下一代测试框架，既可以用来做单元测试，也可以用来做继承测试。

### 安装 ###

eclipse中，选择`Help->Install New Software`，输入`http://beust.com/eclipse`下载安装。

一直下一步安装后会重启eclipse。

### 使用 ###

第一种方法：直接执行

1. 新建一个demo项目、包；
1. 右键包，选择TestNG->Create TestNG Class;
1. 填写类名，勾选注解；
1. 补全测试程序代码，右击程序，选择Run As->TestNG Test执行测试。

第二种方法：通过testng.xml执行

1. 新建一个demo项目、包；
1. 右键包，选择TestNG->Convert to TestNG;
1. 下一步操作，自动生成xml文件；
1. 补全xml文件内容，右击xml文件，选择Run As->TestNG Suite执行测试。

## TestNG基本注解 ##
<table border="1" cellpadding="0" cellspacing="0" class="src" style="border-collapse:collapse; border:1px solid silver; font-family:black verdana,arial,helvetica,sans-serif; line-height:21px; word-break:break-word; word-wrap:break-word">
	<tbody>
		<tr>
			<th>注解</th>
			<th>描述</th>
		</tr>
		<tr>
			<td><strong>@BeforeSuite</strong></td>
			<td>注解的方法将只运行一次，运行所有测试前此套件中。</td>
		</tr>
		<tr>
			<td><strong>@AfterSuite</strong></td>
			<td>注解的方法将只运行一次此套件中的所有测试都运行之后。</td>
		</tr>
		<tr>
			<td><strong>@BeforeClass</strong></td>
			<td>注解的方法将只运行一次先行先试在当前类中的方法调用。</td>
		</tr>
		<tr>
			<td><strong>@AfterClass</strong></td>
			<td>注解的方法将只运行一次后已经运行在当前类中的所有测试方法。</td>
		</tr>
		<tr>
			<td><strong>@BeforeTest</strong></td>
			<td>注解的方法将被运行之前的任何测试方法属于内部类的 &lt;test&gt;标签的运行。</td>
		</tr>
		<tr>
			<td><strong>@AfterTest</strong></td>
			<td>注解的方法将被运行后，所有的测试方法，属于内部类的&lt;test&gt;标签的运行。</td>
		</tr>
		<tr>
			<td><strong>@BeforeGroups</strong></td>
			<td>组的列表，这种配置方法将之前运行。此方法是保证在运行属于任何这些组第一个测试方法，该方法被调用。</td>
		</tr>
		<tr>
			<td><strong>@AfterGroups</strong></td>
			<td>组的名单，这种配置方法后，将运行。此方法是保证运行后不久，最后的测试方法，该方法属于任何这些组被调用。</td>
		</tr>
		<tr>
			<td><strong>@BeforeMethod</strong></td>
			<td>注解的方法将每个测试方法之前运行。</td>
		</tr>
		<tr>
			<td><strong>@AfterMethod</strong></td>
			<td>被注释的方法将被运行后，每个测试方法。</td>
		</tr>
		<tr>
			<td><strong>@DataProvider</strong></td>
			<td>
			<div>标志着一个方法，提供数据的一个测试方法。注解的方法必须返回一个Object[] []，其中每个对象[]的测试方法的参数列表中可以分配。</div>
			该@Test 方法，希望从这个DataProvider的接收数据，需要使用一个dataProvider名称等于这个注解的名字。</td>
		</tr>
		<tr>
			<td><strong>@Factory</strong></td>
			<td>作为一个工厂，返回TestNG的测试类的对象将被用于标记的方法。该方法必须返回Object[]。</td>
		</tr>
		<tr>
			<td><strong>@Listeners</strong></td>
			<td>定义一个测试类的监听器。</td>
		</tr>
		<tr>
			<td><strong>@Parameters</strong></td>
			<td>介绍如何将参数传递给@Test方法。</td>
		</tr>
		<tr>
			<td><strong>@Test</strong></td>
			<td>标记一个类或方法作为测试的一部分。</td>
		</tr>
	</tbody>
</table>


## 注解执行顺序 ##

- 执行 @BeforeSuite
- 执行 @BeforeTest
- 执行 @BeforeClass
- 执行 @DataProvider
- 执行 @BeforeMethod
- 执行 f()
- 执行 @AfterMethod
- 执行 @BeforeMethod
- 执行 f1()
- 执行 @AfterMethod
- 执行 @AfterClass
- 执行 @AfterTest
- 执行 @AfterSuite

f()和f1()为两条Case。

## TestNG的Case执行顺序 ##

如果方法Case过多的话，方法Case的执行顺序是按方法名首字母的ASCII码顺序来执行的。

如何自定义Case的执行顺序呢？

@Test(priority = 1)

	  @Test(priority = 1)
	  public void one() {
		  System.out.println("执行 one()");
	  }
	  @Test(priority = 2)
	  public void two() {
		  System.out.println("执行 two()");
	  }
	  @Test(priority = 3)
	  public void three() {
		  System.out.println("执行 three()");
	  }
	  @Test(priority = 4)
	  public void four() {
		  System.out.println("执行 four()");
	  }

## TestNG忽略执行Case ##

如果不想第二条Case执行，可以添加`enable = false`

	  @Test(priority = 2, enabled = false)
	  public void two() {
		  System.out.println("执行 two()");
	  }

则第二个Case就被跳过了，不会执行。

## TestNG依赖测试 ##

有时候，我们需要按顺序来调用测试用例，那么测试用例之间就存在依赖关系。 TestNG支持测试用例之间的依赖

	public class DependsTest {
	    
	    @Test
	    public void setupEnv(){
	        System.out.println("this is setup Env");
	    }
	    
	    @Test(dependsOnMethods = {"setupEnv"})
	    public void testMessage(){
	        System.out.println("this is test message");
	    }
	}

## TestNG组测试 ##

TestNG中可以把测试用例分组，这样可以按组来执行测试用例；比如：

	public class GroupTest {
	    
	    @Test(groups = {"systemtest"})
	    public void testLogin(){
	        System.out.println("this is test login");
	    }
	    
	    @Test(groups = {"functiontest"})
	    public void testOpenPage(){
	        System.out.println("this is test Open Page");
	    }
	}

然后在testng.xml中 按组执行测试用例

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
	<suite name="Suite1">
	    <test name="test1">
	        <groups>
	        <run>
	        <include name="functiontest" />
	        </run>
	    </groups>
	    </test>
	</suite>

## TestNG处理测试中的异常 ##

测试中，有时候我们期望某些代码抛出异常。

TestNG通过@Test(expectedExceptions)  来判断期待的异常， 并且判断Error Message
	
	public class ExceptionTest {
	    
	    @Test(expectedExceptions = IllegalArgumentException.class, expectedExceptionsMessageRegExp="NullPoint")
	    public void testException(){
	        throw new IllegalArgumentException("NullPoint");
	    }
	}

## TestNG参数化测试 ##

软件测试中，经常需要测试大量的数据集。 测试代码的逻辑完全一样，只是测试的参数不一样。  这样我们就需要一种 “传递测试参数的机制”。 避免写重复的测试代码

TestNG提供了2种传递参数的方式。

第一种: testng.xml 方式使代码和测试数据分离，方便维护

第二种：@DataProvider能够提供比较复杂的参数。 (也叫data-driven testing)

方法一：　通过testng.xml 传递参数给测试代码

	public class ParameterizedTest1 {
	    
	    @Test
	    @Parameters("test1")
	    public void ParaTest(String test1){
	        System.out.println("This is " + test1);
	    }
	}

testng.xml

	<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
	<suite name="Suite1">
	        <parameter name="test1" value="Tank" />
	        <parameter name="test1" value="Xiao" />
	    <test name="test12">
	        <classes>
	            <class name="TankLearn2.Learn.ParameterizedTest1" />
	        </classes>
	    </test>
	</suite>

方式二:   通过DataProvider传递参数

	public class DataProviderLearn {
	    
	    @DataProvider(name="user")
	    public Object[][] Users(){
	        return new Object[][]{
	                {"root","passowrd"},
	                {"admin", "123"},
	                {"user","456"}
	        };
	    }
	    
	    @Test(dataProvider="user")
	    public void verifyUser(String userName, String password){
	        System.out.println("Username: "+ userName + " Password: "+ password);
	    }
	}

## TestNG测试结果报告 ##

测试报告是测试非常重要的部分.  

TestNG默认情况下，会生产两种类型的测试报告HTML的和XML的。 测试报告位于 "test-output" 目录下.

当然我们也可以设置测试报告的内容级别. 

verbose="2" 标识的就是记录的日志级别，共有0-10的级别，其中0表示无，10表示最详细

	<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
	<suite name="Suite1">
	    <test name="test12" verbose="2">
	        <classes>
	            <class name="TankLearn2.Learn.TestNGLearn1" />
	        </classes>
	    </test>
	</suite>

部分参考自： http://www.cnblogs.com/TankXiao/p/3888070.html