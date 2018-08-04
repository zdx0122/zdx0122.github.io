---
layout: post
title:  "TestNG官方文档中文版"
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

### @Factory ###

将方法标记为工厂，返回将由TestNG用作Test类的对象。该方法必须返回`Object[]`。

### @Listeners ###

在测试类上定义侦听器。

参数：

`value` 扩展`org.testng.ITestNGListener`的类数组。

### @Parameters ###

描述如何将参数传递给`@Test`方法。

参数：

`value` 用于填充此方法参数的变量列表。

### @Test ###

将类或方法标记为测试的一部分。

#### @Test的参数 ####

- `alwaysRun`	如果设置为true，则即使依赖于失败的方法，也始终会运行此测试方法。
- `dataProvider`	此测试方法的数据提供程序的名称。
- `dataProviderClass`	查找数据提供程序的类。如果未指定，则将在当前测试方法的类或其基类之一上查找数据提供程序。如果指定了此属性，则数据提供程序方法必须在指定的类上是静态的。
- `dependsOnGroups`	此方法所依赖的组列表。
- `dependsOnMethods`	此方法所依赖的方法列表。
- `description`	此方法的描述。
- `enabled`	是否启用此类/方法上的方法。
- `expectedExceptions`	预期测试方法抛出的异常列表。如果抛出此列表中没有异常或不同异常，则此测试将标记为失败。
- `groups`	此类/方法所属的组列表。
- `invocationCount`	应该调用此方法的次数。
- `invocationTimeOut`	此测试应对所有调用计数的累计时间应采用的最大毫秒数。如果未指定invocationCount，则将忽略此属性。
- `priority`	此测试方法的优先级。将优先安排较低的优先事项。
- `successPercentage`	此方法预期的成功百分比
- `singleThreaded`	如果设置为true，则此测试类上的所有方法都保证在同一个线程中运行，即使当前正在使用`parallel = "methods"`运行测试。此属性只能在类级别使用，如果在方法级别使用，它将被忽略。注意：此属性曾被称为`sequential`（现已弃用）。
- `timeOut`	此测试应采用的最大毫秒数。
- `threadPoolSize`	此方法的线程池大小。该方法将从invocationCount指定的多个线程调用。注意：如果未指定invocationCount，则忽略此属性

## 3 - testng.xml ##

您可以通过几种不同的方式调用TestNG：

- 使用`testng.xml`文件
- 用`ant`
- 从命令行

本节介绍testng.xml的格式（您将在下面找到有关ant和命令行的文档）。

可以在主网站上找到`testng.xml`的当前`DTD`:[testng-1.0.dtd](http://testng.org/testng-1.0.dtd) （为方便起见，您可能更喜欢浏览 [HTML版本](http://testng.org/dtd)）。

这是一个示例`testng.xml`文件：

```xml
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
  
<suite name="Suite1" verbose="1" >
  <test name="Nopackage" >
    <classes>
       <class name="NoPackageTest" />
    </classes>
  </test>
 
  <test name="Regression1">
    <classes>
      <class name="test.sample.ParameterSample"/>
      <class name="test.sample.ParameterTest"/>
    </classes>
  </test>
</suite>
```

您可以指定包名而不是类名：

```xml
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
 
<suite name="Suite1" verbose="1" >
  <test name="Regression1"   >
    <packages>
      <package name="test.sample" />
   </packages>
 </test>
</suite>
```

在此示例中，TestNG将查看包`test.sampl`e中的所有类， 并仅保留具有`TestNG`注解的类。

您还可以指定要包含和排除的组和方法：

```xml
<test name="Regression1">
  <groups>
    <run>
      <exclude name="brokenTests"  />
      <include name="checkinTests"  />
    </run>
  </groups>
  
  <classes>
    <class name="test.IndividualMethodsTest">
      <methods>
        <include name="testMethod" />
      </methods>
    </class>
  </classes>
</test>
```

您还可以在`testng.xml`中定义新组，并在属性中指定其他详细信息，例如是否并行运行测试，使用多少线程，是否运行JUnit测试等等... 

默认情况下，TestNG将按照XML文件中的顺序运行测试。如果希望此文件中列出的类和方法以不可预测的顺序运行，请将`preserve-order`属性设置为`false`

```xml
<test name="Regression1" preserve-order="false">
  <classes>
 
    <class name="test.Test1">
      <methods>
        <include name="m1" />
        <include name="m2" />
      </methods>
    </class>
 
    <class name="test.Test2" />
 
  </classes>
</test>
```

请参阅DTD以获取完整的功能列表，或继续阅读。

## 4 - 运行TestNG ##

可以通过不同方式调用TestNG：

- 命令行
- [ant](https://testng.org/doc/ant.html)
- [Eclipse](https://testng.org/doc/eclipse.html "Eclipse")
- [IntelliJ's IDEA](https://testng.org/doc/idea.html)

本节仅介绍如何从命令行调用TestNG。如果您对其他方式感兴趣，请点击上面的链接之一。

假设您的类路径中有TestNG，调用TestNG的最简单方法如下：

```bash
java org.testng.TestNG testng1.xml [testng2.xml testng3.xml ...]
```

您需要指定至少一个描述您尝试运行的TestNG套件的XML文件。此外，还提供以下命令行开关：

#### 命令行参数 ####

- `-configfailurepolicy`:跳过/继续
	- TestNG是否应该continue执行套件中的剩余测试，或者如果`@Before*`方法失败则skip它们。默认行为是skip。
- `-d`:一个目录
	- 生成报告的目录（默认为`test-output`）。
- `-dataproviderthreadcount`:并行运行测试时用于数据提供程序的默认线程数。
	- 这将设置并行运行测试时用于数据提供程序的默认最大线程数。只有选择了并行模式（例如，使用-parallel选项）时，它才会生效。这可以在套件定义中重写。
- `-excludegroups`:以逗号分隔的组列表。
	- 要从此运行中排除的组列表。
- `-groups`:以逗号分隔的组列表。
	- 要运行的组列表（e.g. "windows,linux,regression"）。
- `-listener`:可以在类路径中找到的以逗号分隔的Java类列表。
	- 允许您指定自己的测试侦听器。这些类需要实现`org.testng.ITestListener`
- `-usedefaultlisteners`:true/false
	- 是否使用默认侦听器
- `-methods`:	逗号分隔的完全限定类名和方法列表,例如 com.example.Foo.f1,com.example.Bar.f2.。
	- 允许您指定要运行的各个方法。
- `-methodselectors	`：以逗号分隔的Java类列表和定义方法选择器的方法优先级。
	- 允许您在命令行上指定方法选择器。例如：com.example.Selector1：3，com.example.Selector2：2
- `-parallel`：methods/tests/classes
	- 如果指定，则设置用于确定在运行测试时如何使用并行线程的默认机制。如果未设置，则默认机制根本不使用并行线程。这可以在套件定义中重写。
- `-reporter`:自定义报告侦听器的扩展配置。
	- 与-listener选项类似，不同之处在于它允许在报告器实例上配置JavaBeans样式的属性。 
	- 示例：`-reporter com.test.MyReporter:methodFilter=*insert*,enableFiltering=true`
	- 您可以拥有此选项的出现次数，每个需要添加一个reporter。
- `-sourcedir`:	以分号分隔的目录列表。
	- 您的javadoc注解测试源所在的目录。只有在使用javadoc类型注解时才需要此选项。 (e.g. `"src/test"` or `"src/test/org/testng/eclipse-plugin;src/test/org/testng/testng"`).
- `-suitename`：用于测试套件的默认名称。
	- 它指定在命令行上定义的测试套件的套件名称。如果suite.xml文件或源代码指定了不同的套件名称，则忽略此选项。如果用“双向”这样的双引号括起来，可以创建一个包含空格的套件名称。
- `-testclass`：可以在类路径中找到的以逗号分隔的类列表。	
	- 由逗号分隔的类文件列表(e.g. "org.foo.Test1,org.foo.test2").
- `-testjar`：一个jar文件。
	- 指定包含测试类的jar文件。如果在该jar文件的根目录中找到testng.xml文件，则将使用该文件，否则，此jar文件中找到的所有测试类将被视为测试类。
- `-testname`：用于测试的默认名称。
	- 它指定在命令行上定义的测试的名称。如果suite.xml文件或源代码指定不同的测试名称，则忽略此选项。如果用双引号“像这样”包围它，可以创建一个带有空格的测试名称。
- `-testnames`：逗号分隔的测试名称列表。
	- 只运行匹配其中一个名称的<test>标记中定义的测试。
- `-testrunfactory`：可以在类路径中找到的Java类。
	- 允许您指定自己的测试运行器。该类需要实现`org.testng.ITestRunnerFactory`。
- `-threadcount`：并行运行测试时使用的默认线程数。
	- 这将设置用于并行运行测试的默认最大线程数。只有选择了并行模式（例如，使用-parallel选项）时，它才会生效。这可以在套件定义中重写。
- `-xmlpathinjar`：jar文件中的XML文件的路径。
	- 此属性应包含测试jar中有效XML文件的路径(e.g. "resources/testng.xml")。默认值为"testng.xml"，表示jar文件根目录下的"testng.xml"文件。除非指定了-testjar，否则将忽略此选项。

可以通过不带任何参数调用TestNG来获取此文档。

您还可以将命令行开关放在文本文件中，例如c:\command.txt，并告诉TestNG使用该文件来检索其参数：

```bash
C:> more c:\command.txt
-d test-output testng.xml
C:> java org.testng.TestNG @c:\command.txt
```

另外，例如，TestNG可以在Java虚拟机的命令行上传递属性

```bash
java -Dtestng.test.classpath="c:/build;c:/java/classes;" org.testng.TestNG testng.xml
```

以下是TestNG理解的属性：

系统属性：
- `testng.test.classpath`：包含测试类的以分号分隔的一系列目录。
	- 如果设置了此属性，TestNG将使用它来查找测试类而不是类路径。如果您在XML文件中使用package标签并且类路径中有很多类，那么这很方便，其中大部分都不是测试类。

例：

```bash
java org.testng.TestNG -groups windows,linux -testclass org.test.MyTest
```

该ant任务和的testng.xml允许你带多个参数（方法，包括指定参数，等...）运行TestNG，所以你应该考虑使用命令行，只有当你试图了解TestNG的命令行，你想get up and running quickly.

要点：如果还指定了一个testng.xml文件（-includedgroups和-excludedgroups除外），它将覆盖testng.xml中找到的所有组包含/排除项，将忽略指定应运行哪些测试的命令行标志。

## 5 - Test methods, Test classes and Test groups ##

### 5.1 - 测试方法 ###

测试方法用`@Test`注解。除非在`testng.xml`中将`allow-return-values`设置为`true`，否则将忽略使用`@Test`注解恰好返回值的方法：

```xml
<suite allow-return-values="true">
 
or
 
<test allow-return-values="true">
```

### 5.2 - 测试组 ###

TestNG允许您执行复杂的测试方法分组。您不仅可以声明方法属于组，还可以指定包含其他组的组。然后可以调用TestNG并要求包括一组特定的组（或正则表达式），同时排除另一组。这为您分区测试提供了最大的灵活性，如果您想要连续运行两组不同的测试，则不需要重新编译任何内容。

组在testng.xml文件中指定，可以在<test>或<suite>标记下找到。<suite>标记中指定的组适用于下面的所有<test>标记。请注意，组在这些标记中是累积的：如果在<suite>中指定组“a”，在<test>中指定“b” ，则将包括“a”和“b”。

例如，至少有两类测试是很常见的

- 准入测试。应在提交新代码之前运行这些测试。它们通常应该很快，并确保没有基本功能被破坏。
- 功能测试。这些测试应涵盖您软件的所有功能，并且每天至少运行一次，尽管理想情况下您希望连续运行它们。

通常，准入测试是功能测试的子集。TestNG允许您以非常直观的方式使用测试组指定。例如，您可以通过说您的整个测试类属于“functest”组来构建测试，另外还有一些方法属于“checkintest”组：

```java
public class Test1 {
  @Test(groups = { "functest", "checkintest" })
  public void testMethod1() {
  }
 
  @Test(groups = {"functest", "checkintest"} )
  public void testMethod2() {
  }
 
  @Test(groups = { "functest" })
  public void testMethod3() {
  }
}
```

用以下方式调用TestNG：

```java
<test name="Test1">
  <groups>
    <run>
      <include name="functest"/>
    </run>
  </groups>
  <classes>
    <class name="example1.Test1"/>
  </classes>
</test>
```

将运行该类中的所有测试方法，而使用checkintest调用它将只运行 testMethod1（）和testMethod2（）。

这是另一个例子，这次使用正则表达式。假设您的某些测试方法不应该在Linux上运行，您的测试将如下所示：

```java
@Test
public class Test1 {
  @Test(groups = { "windows.checkintest" })
  public void testWindowsOnly() {
  }
 
  @Test(groups = {"linux.checkintest"} )
  public void testLinuxOnly() {
  }
 
  @Test(groups = { "windows.functest" )
  public void testWindowsToo() {
  }
}
```

您可以使用以下testng.xml仅启动Windows方法：

```java
<test name="Test1">
  <groups>
    <run>
      <include name="windows.*"/>
    </run>
  </groups>
 
  <classes>
    <class name="example1.Test1"/>
  </classes>
</test>
```

注意：TestNG使用正则表达式，而不是wildmats。请注意区别（例如，"anything" is matched by "`.*`" -- dot star -- and not "*"）。

**方法组**

您还可以排除或包含单个方法：

```java
<test name="Test1">
  <classes>
    <class name="example1.Test1">
      <methods>
        <include name=".*enabledTestMethod.*"/>
        <exclude name=".*brokenTestMethod.*"/>
      </methods>
     </class>
  </classes>
</test>
```

这可以派上用来停用单个方法而不必重新编译任何东西，但是我不建议过多地使用这种技术，因为如果你开始重构你的Java代码（正则表达式中使用的正则表达式），它会使你的测试框架崩溃。标签可能不再符合您的方法）。

### 5.3 - 群组 ###

组还可以包括其他组。这些组称为“MetaGroups”。例如，您可能希望定义包含“checkintest”和“functest”的组“all”。“functest”本身将包含“windows”和“linux”组，而“checkintest将只包含”windows“。以下是如何在属性文件中定义它：

```xml
<test name="Regression1">
  <groups>
    <define name="functest">
      <include name="windows"/>
      <include name="linux"/>
    </define>
  
    <define name="all">
      <include name="functest"/>
      <include name="checkintest"/>
    </define>
  
    <run>
      <include name="all"/>
    </run>
  </groups>
  
  <classes>
    <class name="test.sample.Test1"/>
  </classes>
</test>
```

### 5.4 - 排除组 ###

TestNG允许您包含组以及排除它们。

例如，由于最近的更改而暂时中断测试通常很常见，而您还没有时间修复破损。但是，您确实想要进行功能测试的干净运行，因此需要停用这些测试，但请记住需要重新激活它们。

解决此问题的一种简单方法是创建一个名为“broken”的组，并使这些测试方法属于它。例如，在上面的例子中，我知道testMethod2（）现在已经坏了所以我想禁用它：

```java
@Test(groups = {"checkintest", "broken"} )
public void testMethod2() {
}
```

我现在需要做的就是从运行中排除这个组：

```xml
<test name="Simple example">
  <groups>
    <run>
      <include name="checkintest"/>
      <exclude name="broken"/>
    </run>
  </groups>
  
  <classes>
    <class name="example1.Test1"/>
  </classes>
</test>
```
这样，我将获得一个干净的测试运行，同时跟踪哪些测试被破坏，需要稍后修复。

注意：您还可以使用@Test和@ Before / After注解上的“enabled”属性逐个禁用测试。

### 5.5 - 部分组 ###

测试方法不必是无参数的。您可以在每个测试方法上使用任意数量的参数，并指示TestNG使用@Parameters注解向您传递正确的参数。

有两种方法可以设置这些参数：使用testng.xml或以编程方式。

#### 5.6.1 - testng.xml中的参数

如果您对参数使用简单值，则可以在testng.xml中指定它们 ：

```java
@Parameters({ "first-name" })
@Test
public void testSingleString(String firstName) {
  System.out.println("Invoked testString " + firstName);
  assert "Cedric".equals(firstName);
}
```

在此代码中，我们指定Java方法的参数firstName应该接收名为first-name的XML参数的值。  此XML参数在testng.xml中定义：

```xml
<suite name="My suite">
  <parameter name="first-name"  value="Cedric"/>
  <test name="Simple example">
  <-- ... -->
```

`@ Before / After`和`@Factory`注解可以使用相同的技术：

```java
@Parameters({ "datasource", "jdbcDriver" })
@BeforeMethod
public void beforeTest(String ds, String driver) {
  m_dataSource = ...;                              // look up the value of datasource
  m_jdbcDriver = driver;
}
```

这次，两个Java参数ds 和驱动程序将分别接收赋予属性datasource 和jdbc-driver的值。 
可以使用`Optional`注解将参数声明为可选：

```
@Parameters("db")
@Test
public void testNonExistentParameter(@Optional("mysql") String db) { ... }
```

如果在testng.xml文件中找不到名为“db”的参数，则测试方法将接收`@Optional`注解中指定的默认值：“mysql”。

在`@Parameters`注解可以被放置在下列位置：

在任何已经有`@Test`，`@Before/After`或`@Factory`注解的方法上。

最多只有一个测试类的构造函数。在这种情况下，TestNG将调用此特定构造函数，并在需要实例化测试类时将参数初始化为testng.xml中指定的值。此功能可用于将类中的字段初始化为测试方法随后将使用的值。

笔记：

XML参数按照与注解中相同的顺序映射到Java参数，如果数字不匹配，TestNG将发出错误。

参数是作用域的。在testng.xml中，您可以在<suite>标记下或<test>下声明它们 。如果两个参数具有相同的名称，则它是<test>中定义的具有优先权的参数。如果您需要指定适用于所有测试的参数并仅为某些测试覆盖其值，这将非常方便。

#### 5.6.2 - 使用DataProviders的参数

如果需要传递复杂参数或需要从Java创建的参数（复杂对象，从属性文件或数据库读取的对象等等），则在testng.xml中指定参数可能不够。在这种情况下，您可以使用数据提供程序提供测试所需的值。数据提供程序是类上的一个方法，它返回一组对象数组。此方法使用`@DataProvider`注解：

```java
//This method will provide data to any test method that declares that its Data Provider
//is named "test1"
@DataProvider(name = "test1")
public Object[][] createData1() {
 return new Object[][] {
   { "Cedric", new Integer(36) },
   { "Anne", new Integer(37)},
 };
}
 
//This test method declares that its data should be supplied by the Data Provider
//named "test1"
@Test(dataProvider = "test1")
public void verifyData1(String n1, Integer n2) {
 System.out.println(n1 + " " + n2);
}
```
将打印

```java
Cedric 36
Anne 37
```

`@Test`方法指定了与数据提供数据提供程序属性。此名称必须与使用匹配名称的`@DataProvider(name="...")`注解的同一类上的方法相对应。

默认情况下，将在当前测试类或其中一个基类中查找数据提供程序。如果要将数据提供程序放在不同的类中，则需要使用静态方法或具有非arg构造函数的类，并指定可在dataProviderClass属性中找到的类：

```java
public class StaticProvider {
  @DataProvider(name = "create")
  public static Object[][] createData() {
    return new Object[][] {
      new Object[] { new Integer(42) }
    };
  }
}
 
public class MyTest {
  @Test(dataProvider = "create", dataProviderClass = StaticProvider.class)
  public void test(Integer n) {
    // ...
  }
}
```
数据提供者也支持注入。TestNG将使用测试上下文进行注射。Data Provider方法可以返回以下两种类型之一：

- 一组对象数组（`Object[][]`），其中第一个维度的大小是调用测试方法的次数，第二个维度大小包含必须与测试的参数类型兼容的对象数组方法。这是上述示例所示的情况。
- 一个`Iterator<Object[]> `。与`Object[][]`的唯一区别在于`Iterator`允许您懒惰地创建测试数据。TestNG将调用迭代器，然后使用此迭代器返回的参数逐个调用测试方法。如果您有许多参数集要传递给方法，并且您不想预先创建所有参数集，则此功能特别有用。

以下是此功能的示例：

```java
@DataProvider(name = "test1")
public Iterator<Object[]> createData() {
  return new MyIterator(DATA);
}
```

如果将`@DataProvider`声明为将`java.lang.reflect.Method` 作为第一个参数，TestNG将传递第一个参数的当前测试方法。当多个测试方法使用相同的`@DataProvider`并且您希望它根据为其提供数据的测试方法返回不同的值时，这尤其有用。

例如，以下代码在其`@DataProvider`中打印测试方法的名称：

```java
@DataProvider(name = "dp")
public Object[][] createData(Method m) {
  System.out.println(m.getName());  // print test method name
  return new Object[][] { new Object[] { "Cedric" }};
}
 
@Test(dataProvider = "dp")
public void test1(String s) {
}
 
@Test(dataProvider = "dp")
public void test2(String s) {
}
```

因此将显示：

```java
test1
test2
```

数据提供程序可以与并行属性并行运行：

```java
@DataProvider(parallel = true)
// ...
```

从XML文件运行的并行数据提供程序共享相同的线程池，默认情况下大小为10。您可以在XML文件的<suite>标记中修改此值：

```xml
<suite name="Suite1" data-provider-thread-count="20" >
...
```

如果要在不同的线程池中运行几个特定的​​数据提供程序，则需要从其他XML文件运行它们。

#### 5.6.3 - 报告中的参数

用于调用测试方法的参数显示在TestNG生成的HTML报告中。这是一个例子：

![](http://zdx0122.qiniudn.com/parameters.png)


### 5.7 - 依赖性 ###

有时，您需要按特定顺序调用测试方法。这里有一些例子：

- 在运行更多测试方法之前，确保已完成并成功执行一定数量的测试方法。
- 要初始化测试，同时希望这个初始化方法也是测试方法（使用`@Before/After`标记的方法不会成为最终报告的一部分）。

TestNG允许您使用注解或XML指定依赖项。

#### 5.7.1 - 带注解的依赖关系 ####

您可以使用属性`dependsOnMethods`或`dependsOnGroups`，对发现的`@Test`注解。