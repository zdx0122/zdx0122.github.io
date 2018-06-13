---
layout: post
title:  "TestNg官网文档"
date:   2018-06-12 01
categories: Java
tags:  Java TestNg
author: Dex
---

* content
{:toc}

TestNG是一个Java语言的测试框架，也是QA最常用的测试框架之一。









感谢原作者的奉献，原作者博客地址：[http://blog.csdn.net/zhu_ai_xin_520/article/details/6199194](https://link.jianshu.com?t=http://blog.csdn.net/zhu_ai_xin_520/article/details/6199194)。

本次使用markdown重新制作一次，带有些地方的修正。

# 1 - 简介

TestNG 是一个测试框架，它被设计为用来简化广泛的设计需求，从单元测试 (单独测试一个类) 到集成测试 (测试由多个类、报甚至外部框架，例如应用程序服务器，所组成的系统).

编写一个测试通常分为三步：

*   编写测试业务逻辑，并且在你的代码中插入`TestNG annotations` .
*   在`testng.xml`或 `build.xml` 添加你的测试信息。例如类名，希望运行的组等等.
*   运行TestNG.

你可以在欢迎

[页面](https://link.jianshu.com?t=http://testng.org/doc/index.html)上找到一个快速入门的例子。

文档中会使用到如下的概念：

*   一套测试（suite）由一个XML文件所表示。它能够包含一个或者多个测试， &lt;suite&gt; 标记来定义.
*   用 &lt;test&gt; 标记来表示一个测试，并且可以包含一个或者多个TestNG类.
*   所谓TestNG类，是一个Java类，它至少包含一个TestNG annotation.它由&lt;class&gt; 标记表示，并且可以包含一个或着多个测试方法。
*   测试方法，就是一个普通的Java方法，在由`@Test`标记.

TestNG测试可以通过`@BeforeXXX`和`@AfterXXX annotations`来标记，允许在特定的生命周期点上执行一些Java逻辑，这些点可以是上述列表中任意的内容。

本手册的其余部分将会解释如下内容：

*   所有`annotation`的列表并且给出扼要的解释。它会告诉你很多TestNG所提供的功能，但是你需要详细阅读其余部分来获得关于这些注解更详细的信息.
*   关于`testng.xml`文件的说明。它的语法以及你能在里面写什么.
*   上述内容的详细说明，同时也有结合了`annotation`和`testing.xml`文件的使用说明.

# 2 - Annotations

如下就是在TestNG中可以使用的annotation的速查预览，并且其中给出了属性：

<table>
<thead>
<tr>
<th style="text-align:left"></th>
<th style="text-align:left">TestNG 类的配置信息</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">**@BeforeSuite**</td>
<td style="text-align:left">
**@BeforeSuite**: 被注解的方法，会在当前`suite`中所有测试方法之 前 被调用。</td>
</tr>
<tr>
<td style="text-align:left">**@AfterSuite**</td>
<td style="text-align:left">
**@AfterSuite**: 被注解的方法，会在当前suite中所有测试方法之 后 被调用。</td>
</tr>
<tr>
<td style="text-align:left">**@BeforeTest**</td>
<td style="text-align:left">
**@BeforeTest**: 被注解的方法，会在测试（原文就是测试，不是测试方法）运行 前 被调用</td>
</tr>
<tr>
<td style="text-align:left">**@AfterTest**</td>
<td style="text-align:left">
**@AfterTest**: 被注解的方法，会在测试（原文就是测试，不是测试方法）运行后 被调用</td>
</tr>
<tr>
<td style="text-align:left">**@BeforeGroups**</td>
<td style="text-align:left">
**@BeforeGroups**: 被注解的方法会在组列表中之前被调用。这个方法会在每个组中第一个测试方法被调用之前被调用</td>
</tr>
<tr>
<td style="text-align:left">**@AfterGroups**</td>
<td style="text-align:left">
**@AfterGroups**: 被注解的方法会在组列表中之后被调用。这个方法会在每个组中最后一个测试方法被调用之后被调用</td>
</tr>
<tr>
<td style="text-align:left">**@BeforeClass**</td>
<td style="text-align:left">
**@BeforeClass**: 被注解的方法，会在当前类第一个测试方法运行前被调用</td>
</tr>
<tr>
<td style="text-align:left">**@AfterClass**</td>
<td style="text-align:left">
**@AfterClass**: 被注解的方法，会在当前类所有测试方法运行后被调用</td>
</tr>
<tr>
<td style="text-align:left">**@BeforeMethod**</td>
<td style="text-align:left">
**@BeforeMethod**: 被注解的方法，会在运行每个测试方法之前调用</td>
</tr>
<tr>
<td style="text-align:left">**@AfterMethod**</td>
<td style="text-align:left">
**@AfterMethod**: 被注解的方法，会在每个测试方法运行之后被调用</td>
</tr>
<tr>
<td style="text-align:left">_alwaysRun_</td>
<td style="text-align:left">对于在方法之前的调用(_beforeSuite, beforeTest, beforeTestClass 和 beforeTestMethod, 除了beforeGroups_): 若为`true`，这个配置方法无视其所属的组而运行.对于在方法之后的调用(_afterSuite, afterClass_, ...): 若为`true`， 这个配置方法会运行，即使其之前一个或者多个被调用的方法失败或者被跳过。</td>
</tr>
<tr>
<td style="text-align:left">dependsOnGroups</td>
<td style="text-align:left">方法所依赖的一组`group`列表</td>
</tr>
<tr>
<td style="text-align:left">dependsOnMethods</td>
<td style="text-align:left">方法所依赖的一组`method`列表</td>
</tr>
<tr>
<td style="text-align:left">enabled</td>
<td style="text-align:left">在当前`class/method`中被此`annotation`标记的方法是否参与测试（不启用则不在测试中运行）</td>
</tr>
<tr>
<td style="text-align:left">groups</td>
<td style="text-align:left">一组`group`列表，指明了这个`class/method`的所属。</td>
</tr>
<tr>
<td style="text-align:left">inheritGroups</td>
<td style="text-align:left">如果是true，则此方法会从属于在类级由`@Test`注解中所指定的组</td>
</tr>
<tr>
<td style="text-align:left">**@DataProvider**</td>
<td style="text-align:left">**把此方法标记为为测试方法提供数据的方法。被此注释标记的方法必须返回`Object[][]`，其中的每个Object[]可以被分配给测试方法列表中的方法当做参数。那些需要从DataProvider接受数据的`@Test`方法，需要使用一个`dataprovider`名字，此名称必须与这个注解中的名字相同。**</td>
</tr>
<tr>
<td style="text-align:left">name</td>
<td style="text-align:left">DataProvider的名字</td>
</tr>
<tr>
<td style="text-align:left">**@Factory**</td>
<td style="text-align:left">**把一个方法标记为工厂方法，并且必须返回被TestNG测试类所使用的对象们。 此方法必须返回 Object[]。**</td>
</tr>
<tr>
<td style="text-align:left">**@Parameters**</td>
<td style="text-align:left">说明如何给一个`@Test`方法传参。</td>
</tr>
<tr>
<td style="text-align:left">value</td>
<td style="text-align:left">方法参数变量的列表</td>
</tr>
<tr>
<td style="text-align:left">**@Test**</td>
<td style="text-align:left">**把一个类或者方法标记为测试的一部分。**</td>
</tr>
<tr>
<td style="text-align:left">alwaysRun</td>
<td style="text-align:left">如果为`true`，则这个测试方法即使在其所以来的方法为失败时也总会被运行。</td>
</tr>
<tr>
<td style="text-align:left">dataProvider</td>
<td style="text-align:left">这个测试方法的`dataProvider`
</td>
</tr>
<tr>
<td style="text-align:left">dataProviderClass</td>
<td style="text-align:left">指明去那里找`data provider`类。如果不指定，那么就当前测试方法所在的类或者它个一个基类中去找。如果指定了，那么`data provider`方法必须是指定的类中的静态方法。</td>
</tr>
<tr>
<td style="text-align:left">ependsOnGroups</td>
<td style="text-align:left">方法所依赖的一组`grou`p列表</td>
</tr>
<tr>
<td style="text-align:left">dependsOnMethods</td>
<td style="text-align:left">方法所以来的一组`method`列表</td>
</tr>
<tr>
<td style="text-align:left">description</td>
<td style="text-align:left">方法的说明</td>
</tr>
<tr>
<td style="text-align:left">enabled</td>
<td style="text-align:left">在当前`class/method`中被此`annotation`标记的方法是否参与测试（不启用则不在测试中运行）</td>
</tr>
<tr>
<td style="text-align:left">expectedExceptions</td>
<td style="text-align:left">此方法会抛出的异常列表。如果没有异常或者一个不在列表中的异常被抛出，则测试被标记为失败。</td>
</tr>
<tr>
<td style="text-align:left">groups</td>
<td style="text-align:left">一组`group`列表，指明了这个`class/method`的所属。</td>
</tr>
<tr>
<td style="text-align:left">invocationCount</td>
<td style="text-align:left">方法被调用的次数。</td>
</tr>
<tr>
<td style="text-align:left">invocationTimeOut</td>
<td style="text-align:left">当前测试中所有调用累计时间的最大毫秒数。如果`invocationCount`属性没有指定，那么此属性会被忽略。</td>
</tr>
<tr>
<td style="text-align:left">successPercentage</td>
<td style="text-align:left">当前方法执行所期望的`success`的百分比</td>
</tr>
<tr>
<td style="text-align:left">sequential</td>
<td style="text-align:left">如果是`true`，那么测试类中所有的方法都是按照其定义顺序执行，即使是当前的测试使用`parallel="methods"`。此属性只能用在类级别，如果用在方法级，就会被忽略。</td>
</tr>
<tr>
<td style="text-align:left">timeOut</td>
<td style="text-align:left">当前测试所能运行的最大毫秒数</td>
</tr>
<tr>
<td style="text-align:left">threadPoolSize</td>
<td style="text-align:left">此方法线程池的大小。 此方法会根据制定的`invocationCount`值，以多个线程进行调用。_注意：如果没有指定`invocationCount`属性，那么此属性就会被忽略_
</td>
</tr>
</tbody>
</table>

# 3 - testng.xml

调用TestNG有多种方式：

*   使用`testng.xml`文件
*   使用`ant`

*   通过命令行

本节对`testng.xml`的格式进行说明(你会在下文找到关于ant和命令行的相关文档)。

目前给testng.xml所使用的DTD可以在主页： [http://testng.org/testng-1.0.dtd](https://link.jianshu.com?t=http://testng.org/testng-1.0.dtd) 上找到（考虑到您能更方便，可以浏览 [HTML版](https://link.jianshu.com?t=http://testng.org/testng-1.0.dtd.php))。

下面是个`testng.xml`文件的例子：

    <span class="hljs-meta">&lt;!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" &gt;</span>

    <span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Suite1"</span> <span class="hljs-attr">verbose</span>=<span class="hljs-string">"1"</span> &gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Nopackage"</span> &gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
           <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"NoPackageTest"</span> /&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression1"</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.sample.ParameterSample"</span>/&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.sample.ParameterTest"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">suite</span>&gt;</span>
    `</pre>

    你也可以指定包名来替代类名：

    <pre class="hljs xml">`<span class="hljs-meta">&lt;!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" &gt;</span>

    <span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Suite1"</span> <span class="hljs-attr">verbose</span>=<span class="hljs-string">"1"</span> &gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression1"</span>   &gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">packages</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">package</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.sample"</span> /&gt;</span>
       <span class="hljs-tag">&lt;/<span class="hljs-name">packages</span>&gt;</span>
     <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">suite</span>&gt;</span>
    `</pre>

    在这个例子中，TestNG会查看所有在`test.sample`的类，并且只保留含有`TestNG annotations`的类。

    你也可以指定要包含和排除掉的组和方法：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">exclude</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"brokenTests"</span>  /&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"checkinTests"</span>  /&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.IndividualMethodsTest"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">methods</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"testMethod"</span> /&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">methods</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">class</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    你也可以在testng.xml定义新的group，并且在属性中指明额外的详细信息，例如是否并行运行，使用多少个线程，并且是否正在运行JUnit测试等等……

    默认情况下，TestNG按他们找到的XML文件中的顺序运行测试。如果你想在这个文件中列出的类和方法的无序顺序运行，可以设置`preserve-order`属性设置为`false`。

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression1"</span> <span class="hljs-attr">preserve-order</span>=<span class="hljs-string">"false"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.Test1"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">methods</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"m1"</span> /&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"m2"</span> /&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">methods</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">class</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.Test2"</span> /&gt;</span>

      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    # 4 - 运行

    TestNG可以使用多种方式调用：

*   命令行
*   ant
*   Eclipse
*   IntelliJ's IDEA

    本节将只介绍如何从命令行运行TestNG。如果您对其他方式感兴趣，那么就点击上面的链接查看更多信息。

    假设TestNG已经在你的类路径中，最简单的调用方式如下：

    `java org.testng.TestNG testng1.xml [testng2.xml testng3.xml ...]`

    你至少要指定一个XML文件，它描述了你要运行的TestNG suite。此外，还有如下命令行参数：命令行参数

    <table>
    <thead>
    <tr>
    <th style="text-align:left">选项</th>
    <th style="text-align:left">参数</th>
    <th style="text-align:left">说明</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left">-d</td>
    <td style="text-align:left">目录</td>
    <td style="text-align:left">报告会生成的目录 (默认是`test-output`).</td>
    </tr>
    <tr>
    <td style="text-align:left">-excludegroups</td>
    <td style="text-align:left">逗号分隔的组列表</td>
    <td style="text-align:left">要在当前运行中被排除的组列表.</td>
    </tr>
    <tr>
    <td style="text-align:left">-groups</td>
    <td style="text-align:left">逗号分隔的组列表</td>
    <td style="text-align:left">想要运行的组(`e.g. "windows,linux,regression"`).</td>
    </tr>
    <tr>
    <td style="text-align:left">-listener</td>
    <td style="text-align:left">逗号分隔的Java类列表，它们都可以在你的类路径中找到</td>
    <td style="text-align:left">让你指定你自己的监听器。这些类需要实现[org.testng.ITestListener](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/ITestListener.html)
    </td>
    </tr>
    <tr>
    <td style="text-align:left">-parallel</td>
    <td style="text-align:left">方法/测试</td>
    <td style="text-align:left">如果指定了，那么在运行测试的时候，所使用的默认的机制就会决定如何去使用并行 线程。反之则不会.这是可以在suite定义中被覆盖的</td>
    </tr>
    <tr>
    <td style="text-align:left">-reporter</td>
    <td style="text-align:left">自定义报告监听器的扩展配置</td>
    <td style="text-align:left">类似于`-listener`选项，允许在报告器实例中配置JavaBean的样式属性.`-reporter com.test.MyReporter:methodFilter=*insert*,enableFiltering=true`这个选项不限次数，根据需要一样一个。</td>
    </tr>
    <tr>
    <td style="text-align:left">-sourcedir</td>
    <td style="text-align:left">分号间隔的目录列表</td>
    <td style="text-align:left">使用了JavaDoc类型的`annotation`的源码所在的目录。这个选项只有你在使用JavaDoc类型的注解时才会有用。`(e.g. "src/test" or"src/test/org/testng/eclipse-plugin;src/test/org/testng/testng")`.</td>
    </tr>
    <tr>
    <td style="text-align:left">-suitename</td>
    <td style="text-align:left">
    `test suite`默认的名字</td>
    <td style="text-align:left">指明了在命令行中定义的`test suite`的名字。这个选项在suite.xml或源码指定了不同的名字时会被忽略。如果使用双引号括起来，就可在名字中使用空格。例如："like this".</td>
    </tr>
    <tr>
    <td style="text-align:left">-testclass</td>
    <td style="text-align:left">逗号分隔的类列表，它们必须能在类路径中被找到</td>
    <td style="text-align:left">逗号分隔的测试类的列表 `(e.g. "org.foo.Test1,org.foo.test2")`.</td>
    </tr>
    <tr>
    <td style="text-align:left">-testjar</td>
    <td style="text-align:left">一个jar文件</td>
    <td style="text-align:left">指定了一个包含了测试类的Jar文件。如果`testng.xml`在jar文件的根目录被找到，就使用之，反之，jar文件中所有的类都会被当成测试类。</td>
    </tr>
    <tr>
    <td style="text-align:left">-testname</td>
    <td style="text-align:left">测试所使用的默认名字</td>
    <td style="text-align:left">它为在命令行中定义的测试指定了名字。这个选项在suite.xml或源码指定了不同的名字时会被忽略。如果使用双引号括起来，就可在名字中使用空格。例如："like this"。</td>
    </tr>
    <tr>
    <td style="text-align:left">-testrunfactory</td>
    <td style="text-align:left">可以在类路径中找到的Java类</td>
    <td style="text-align:left">让你指定你自己的测试运行器，相关的类必须实现[org.testng.ITestRunnerFactory.](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/ITestRunnerFactory.html)
    </td>
    </tr>
    <tr>
    <td style="text-align:left">-threadcount</td>
    <td style="text-align:left">在并行测试的时候默认使用的线程数</td>
    <td style="text-align:left">并行运行中所使用的最大线程数。只在使用并行模式中有效（例如，使用-parallel选项）。它可以在suite定义中被覆盖。</td>
    </tr>
    </tbody>
    </table>

    上面的参数说明可以通过不带任何参数运行TestNG来获得。

    你也可以把命令行开关放到文件中，例如说`c:/command.txt`，之后告诉 TestNG 使用这个文件来解析其参数：

    <pre class="hljs bash">`C:&gt; more c:\command.txt
    -d <span class="hljs-built_in">test</span>-output testng.xml
    C:&gt; java org.testng.TestNG @c:\command.txt
    `</pre>

    此外TestNG也可以在命令行下向其传递JVM参数。例如：

    <pre class="hljs bash">`java -Dtestng.test.classpath=<span class="hljs-string">"c:/build;c:/java/classes;"</span> org.testng.TestNG testng.xml
    `</pre>

    如下是TestNG所能理解的属性：

    <table>
    <thead>
    <tr>
    <th style="text-align:left">属性</th>
    <th style="text-align:left">类型</th>
    <th>说明</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td style="text-align:left">testng.test.classpath</td>
    <td style="text-align:left">分号分的一系列目录，其中包含了你的测试类</td>
    <td>如果指定了这个属性，TestNG就会查找你的测试类而不是类路径。这在你的类路径中有很多类，而大多数又不是测试类，或者在xml文件中使用 package 标记的时候会很方便。</td>
    </tr>
    </tbody>
    </table>

    例子:

    `java org.testng.TestNG -groups windows,linux -testclass org.test.MyTest`

    注意ant task和[testng.xml](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#testng-xml)允许你使用更多的参数来运行TestNG（要包含的方法、指定的参数等等），所以你在学习TestNG的时候考虑使用命令行，因为这样能让你快速进步。

    # 5 - 测试方法、测试类和测试组

    ### 5.1 - 测试方法

    测试方法被注解为@Test。注释的方法与@Test碰巧返回值将被忽略，除非你设置允许回值设置为`true`在你的`testng.xml`：

    <pre class="hljs cpp">`&lt;suite allow-<span class="hljs-keyword">return</span>-values=<span class="hljs-string">"true"</span>&gt;
    <span class="hljs-keyword">or</span>
    &lt;test allow-<span class="hljs-keyword">return</span>-values=<span class="hljs-string">"true"</span>&gt;
    `</pre>

    ### 5.2 - 测试组

    TestNG 允许你将测试方法归类为不同的组。不仅仅是可以声明某个方法属于某个组，而且还可以让组包含其他的组。这样TestNG可以调用或者请求包含一组特定的组（或者正则表达式）而排除其他不需要组的集合。这样，如果你打算将测试分成两份的时候，就无需重新编译。这个特点，会给你在划分组的时候带来很大的灵活性。

    组在的testng.xml文件中指定，可以发现无论是`&lt;test&gt;`或`&lt;suite&gt;`标签。指定组`&lt;suite&gt;`标签适用于所有的`&lt;test&gt;`下方标签。需要注意的是群体标签：如果指定组的“a”在`&lt;suite&gt;`标签中和b在`&lt;test&gt;`标签中，那么这两个“a”和“b”将被包括在内。

    例如，通常将测试划分为两种类别是再常见不过的了：

*   检查性测试（Check-in test）：这些测试在你提交新代码之前就会运行。它们一般都是很快进行的，并且保证没有哪个基本的功能是不好使的。
*   功能性测试（Functional test）：这些测试涵盖你的软件中所有的功能，并且至少每天运行一次，不过你也可能希望他们持续的运行。

    典型的来说，检测性测试通常是功能性测试的一个子集。TestNG允许你根据个人感觉来进行组划分。例如，你可能希望把你所有的测试类都划归为`"functest"`组，并且额外的有几个方法输入`"checkintest"`组。

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Test1</span> </span>{
      <span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"functest"</span>, <span class="hljs-string">"checkintest"</span> })
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testMethod1</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-meta">@Test</span>(groups = {<span class="hljs-string">"functest"</span>, <span class="hljs-string">"checkintest"</span>} )
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testMethod2</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"functest"</span> })
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testMethod3</span><span class="hljs-params">()</span> </span>{
      }
    }
    `</pre>

    通过下面的内容调用TestNG

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Test1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"functest"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"example1.Test1"</span>/&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    以上会运行上面那个类中所有的测试，当药使用checkintest进行调用的时候，就仅仅运行testMethod1()和testMethod2()。

    下面是另外一个例子。这次使用正则表达式。假定有些测试方法不应该运行在Linux环境下，你的测试会看起来像：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Test1</span> </span>{
      <span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"windows.checkintest"</span> })
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testWindowsOnly</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-meta">@Test</span>(groups = {<span class="hljs-string">"linux.checkintest"</span>} )
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testLinuxOnly</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"windows.functest"</span> )
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testWindowsToo</span><span class="hljs-params">()</span> </span>{
      }
    }
    `</pre>

    然后你就可以使用下面这个 testng.xml 来只运行在Windows下的方法：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Test1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"windows.*"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"example1.Test1"</span>/&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    _注意：TestNG使用的是`正则表达式`，而不是`通配符`。注意这二者的区别（例如，"anything" 是匹配于 "._"代表点和星号，而不是星号 "_")._

    ### Method groups

    也可以单独排除和包含若干方法：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Test1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"example1.Test1"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">methods</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">".*enabledTestMethod.*"</span>/&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">exclude</span> <span class="hljs-attr">name</span>=<span class="hljs-string">".*brokenTestMethod.*"</span>/&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">methods</span>&gt;</span>
         <span class="hljs-tag">&lt;/<span class="hljs-name">class</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    这样就可以不用编译而处理任何不需要的方法了，但是我不推荐过分的使用这个技巧，因为如果你要重构你的代码，那么这有可能让你的测试框架出问题（在标签中使用的方法可能再也不会匹配你的方法名了）。

    ### 5.3 - 组中组

    测试组也可以包含其他组。这样的组叫做“元组`"（MetaGroups）"`。例如，你可能要定义一个组`"all"`来包含其他的组，`"chekcintest"`和`"functest"`。`"functest"`本身只包含了组windows和linux，而`"checkintest"`仅仅包含windows。你就可以在属性文件中这样定义：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">define</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"functest"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"windows"</span>/&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"linux"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">define</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">define</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"all"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"functest"</span>/&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"checkintest"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">define</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"all"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.sample.Test1"</span>/&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    ### 5.4 - 排除组

    TestNG 允许你包含组，当然也可以排除之。

    譬如说，因为最近的改动，导致当前的测试中断并且，你还没有时间修复这些问题都是司空见惯的。但是，你还需要自己的功能测试可以正确运行，所以，制药简单的让这些不需要的测试失效就可以了。但是别忘记在以后需要的时候，要重新让其生效。

    一个简单的办法来解决这个问题就是创建一个叫做`"broken"`组， 然后使得这些测试方法从属于那个组。例如上面的例子，假设我知道`testMethod2()`会中断，所以我希望使其失效：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>(groups = {<span class="hljs-string">"checkintest"</span>, <span class="hljs-string">"broken"</span>} )
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testMethod2</span><span class="hljs-params">()</span> </span>{
    }
    `</pre>

    而我所需要做的一切就是从运行中排除这个组：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Simple example"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"checkintest"</span>/&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">exclude</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"broken"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"example1.Test1"</span>/&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    通过这种办法，我们既可以得到整洁的测试运行，同时也能够跟踪那些需要稍后修正的中断的测试。

    _注意：你可以可以通过使用`"enabled"`属性来完成，这个属性适用于`@Test`和 `@Before/After annotation`。_

    ### 5.5 - 局部组

    可以在类级别定义组，之后还可以在方法级定义组：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"checkin-test"</span> })
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">All</span> </span>{

      <span class="hljs-meta">@Test</span>(groups = { <span class="hljs-string">"func-test"</span> )
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">method1</span><span class="hljs-params">()</span> </span>{ ... }

      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">method2</span><span class="hljs-params">()</span> </span>{ ... }
    }
    `</pre>

    在这个类中，`method2()`类级组`"checkin-test"`的一部分，而`method1()`即属于`"checkin-test"`也属于`"func-test"`组。

    ### 5.6 - 参数

    测试方法是可以带有参数的。每个测试方法都可以带有任意数量的参数，并且可以通过使用TestNG的`@Parameters`向方法传递正确的参数。

    设置方式有两种方法：使用`testng.xml`或者程序编码。

    ### 5.6.1 - 使用 testng.xml 设置参数

    如果只使用相对简单的参数，你可以在你的`testng.xml`文件中指定：

    <pre class="hljs java">`<span class="hljs-meta">@Parameters</span>({ <span class="hljs-string">"first-name"</span> })
    <span class="hljs-meta">@Test</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testSingleString</span><span class="hljs-params">(String firstName)</span> </span>{
      System.out.println(<span class="hljs-string">"Invoked testString "</span> + firstName);
      <span class="hljs-keyword">assert</span> <span class="hljs-string">"Cedric"</span>.equals(firstName);
    }
    `</pre>

    在这段代码中，我们让 firstName 参数能够接到XML文件中叫做 `first-name`参数的值。这个XML参数被定义在 `testng.xml`：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">parameter</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"first-name"</span>  <span class="hljs-attr">value</span>=<span class="hljs-string">"Cedric"</span>/&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Simple example"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">--</span> <span class="hljs-attr">...</span> <span class="hljs-attr">--</span>&gt;</span>
    `</pre>

    类似的，它也可以用在`@Before/After`和`@Factory`注解上：

    <pre class="hljs java">`<span class="hljs-meta">@Parameters</span>({ <span class="hljs-string">"datasource"</span>, <span class="hljs-string">"jdbcDriver"</span> })
    <span class="hljs-meta">@BeforeMethod</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">beforeTest</span><span class="hljs-params">(String ds, String driver)</span> </span>{
      m_dataSource = ...;                              <span class="hljs-comment">// 查询数据源的值</span>
      m_jdbcDriver = driver;
    }
    `</pre>

    这次有两个Java参数_`ds`_和_`driver`_会分别接收到来自属性`datasource``和`jdbc-driver`所指定的值。

    参数也可以通过 Optional 注释来声明：

    <pre class="hljs java">`<span class="hljs-meta">@Parameters</span>(<span class="hljs-string">"db"</span>)
    <span class="hljs-meta">@Test</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testNonExistentParameter</span><span class="hljs-params">(@Optional(<span class="hljs-string">"mysql"</span>)</span> String db) </span>{ ... }
    `</pre>

    如果在你的`testng.xml`文件中没有找到`"db"`，你的测试方法就会使用 `@Optional`中的值：`"mysql"`。

    `@Parameters`可以被放置到如下位置：

*   在任何已经被`@Test`, `@Before/After`或`@Factory`注解过的地方。
*   在测试类中至多被放到一个构造函数签。这样，TestNG才能在需要的时候使用 testng.xml 中特定的参数来实例化这个类。这个特性可以被用作初始化某些类中的值，以便稍后会被类中其他的方法所使用。

    注意：

*   XML中的参数会按照Java参数在注解中出现的顺序被映射过去，并且如果数量不匹配，TestNG会报错。
*   参数是有作用范围的。在`testng.xml`中，你即可以在&lt;suite&gt; 标签下声明，也可以在 &lt;test&gt;下声明。如果两个参数都有相同的名字，那么，定义在 &lt;test&gt; 中的有优先权。这在你需要覆盖某些测试中特定参数的值时，会非常方便。

    ### 5.6.2 - 使用DataProviders提供参数

    在 testng.xml 中指定参数可能会有如下的不足：

    如果你压根不用`testng.xml`.

    你需要传递复杂的参数，或者从Java中创建参数（复杂对象，对象从属性文件或者数据库中读取的etc...）

    这样的话，你就可以使用`Data Provider`来给需要的测试提供参数。所谓数据提供者，就是一个能返回对象数组的数组的方法，并且这个方法被@DataProvider注解标注：

    <pre class="hljs java">`<span class="hljs-comment">//这个方法会服务于任何把它（测试方法）的数据提供者的名字为"test1"方法</span>
    <span class="hljs-meta">@DataProvider</span>(name = <span class="hljs-string">"test1"</span>)
    <span class="hljs-keyword">public</span> Object[][] createData1() {
     <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Object[][] {
       { <span class="hljs-string">"Cedric"</span>, <span class="hljs-keyword">new</span> Integer(<span class="hljs-number">36</span>) },
       { <span class="hljs-string">"Anne"</span>, <span class="hljs-keyword">new</span> Integer(<span class="hljs-number">37</span>)},
     };
    }

    <span class="hljs-comment">//这个测试方法，声明其数据提供者的名字为“test1”</span>
    <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"test1"</span>)
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">verifyData1</span><span class="hljs-params">(String n1, Integer n2)</span> </span>{
     System.out.println(n1 + <span class="hljs-string">" "</span> + n2);
    }
    `</pre>

    结果会打印

    <pre class="hljs undefined">`Cedric 36 
    Anne 37
    `</pre>

    被`@Test`标注的方法通过`dataProvider`属性指明其数据提供商。这个名字必须与`@DataProvider(name="...")`中的名字相一致。

    默认的情况下，数据提供者会查找当前的测试类或者测试类的基类。如果你希望它能够被其他的类所使用，那么就要将其指定为`static`，并且通过 `dataProviderClass`属性指定要使用的类：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">StaticProvider</span> </span>{
      <span class="hljs-meta">@DataProvider</span>(name = <span class="hljs-string">"create"</span>)
      <span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> Object[][] createData() {
        <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Object[][] {
          <span class="hljs-keyword">new</span> Object[] { <span class="hljs-keyword">new</span> Integer(<span class="hljs-number">42</span>) }
        }
      }
    }

    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">MyTest</span> </span>{
      <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"create"</span>, dataProviderClass = StaticProvider.class)
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test</span><span class="hljs-params">(Integer n)</span> </span>{
        <span class="hljs-comment">// ...</span>
      }
    }
    `</pre>

    `Data Provider`方法可以返回如下两种类型中的一种：

*   含有多个对象的数组`(Object[][])`，其中第一个下标指明了测试方法要调用的次数，第二个下标则完全与测试方法中的参数类型和个数相匹配。上面的例子已经说明。
*   另外一个是迭代器`Iterator&lt;Object[]&gt;`。二者的区别是迭代器允许你延迟创建自己的测试数据。TestNG会调用迭代器，之后测试方法会一个接一个的调用由迭代器返回的值。在你需要传递很多参数组给测试组的时候，这样你无须提前创建一堆值。

    下面是使用这个功能的例子：

    <pre class="hljs java">`<span class="hljs-meta">@DataProvider</span>(name = <span class="hljs-string">"test1"</span>)
    <span class="hljs-keyword">public</span> Iterator&lt;Object[]&gt; createData() {
      <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> MyIterator(DATA);
    }
    `</pre>

    如果你声明的`@DataProvider`使用`java.lang.reflect.Method`作为第一个参数，TestNG 会把当前的测试方法当成参数传给第一个参数。这一点在你的多个测试方法使用相同的`@DataProvider`的时候，并且你想要依据具体的测试方法返回不同的值时，特别有用。

    例如，下面的代码它内部的`@DataProvider`中的测试方法的名字：

    <pre class="hljs java">`<span class="hljs-meta">@DataProvider</span>(name = <span class="hljs-string">"dp"</span>)
    <span class="hljs-keyword">public</span> Object[][] createData(Method m) {
      System.out.println(m.getName());  <span class="hljs-comment">// print test method name</span>
      <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Object[][] { <span class="hljs-keyword">new</span> Object[] { <span class="hljs-string">"Cedric"</span> }};
    }

    <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"dp"</span>)
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test1</span><span class="hljs-params">(String s)</span> </span>{
    }

    <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"dp"</span>)
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test2</span><span class="hljs-params">(String s)</span> </span>{
    }
    `</pre>

    所以会显示：

    <pre class="hljs undefined">`test1 
    test2
    `</pre>

    `Data provider`可以通过属性`parallel`实现并行运行：

    <pre class="hljs java">`<span class="hljs-meta">@DataProvider</span>(parallel = <span class="hljs-keyword">true</span>)
    <span class="hljs-comment">// ...</span>
    `</pre>

    使用XML文件运行的`data provider`享有相同的线程池，默认的大小是10.你可以通过修改该在`&lt;suite&gt;`标签中的值来更改：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Suite1"</span> <span class="hljs-attr">data-provider-thread-count</span>=<span class="hljs-string">"20"</span> &gt;</span>
    ...
    `</pre>

    如果你需要让指定的几个`data provider`运行在不同的线程中，那么就必须通过不同的xml文件来运行。

    ### 5.6.3 - 在报告中的参数

    在测试方法调用中所使用的参数，会在由TestNG中生成HTML报告里显示出来。下面是几个例子：

    
    ![](http://langhai.qiniudn.com/parameters.png)

    ### 5.7 - 依赖方法

    有些时候，需要按照特定顺序调用测试方法。对于下面的情况，这非常有用：

*   在运行更多测试方法之前确保一定数量的测试方法已经完成并成功
*   要初始化的测试，同时希望这个初始化方法是测试方法，以及（标有@Before/After的方法不会最终报告的一部分）。

    TestNG的允许您指定的依赖要么注解或XML格式。

    ### 5.7.1 - 依赖和注释

    确保在进行更多的方法测试之前，有一定数量的测试方法已经成功完成。

    在初始化测试的时候，同时希望这个初始化方法也是一个测试方法（`@Before/After`不会出现在最后生成的报告中）。

    为此，你可以使用`@Test`中的`dependsOnMethods`或`dependsOnGroups`属性。

    这两种依赖：

*   Hard dependencies（硬依赖）。所有的被依赖方法必须成功运行。只要有一个出问题，测试就不会被调用，并且在报告中被标记为SKIP。
*   Soft dependencies（软依赖）。 即便是有些依赖方法失败了，也一样运行。如果你只是需要保证你的测试方法按照顺序执行，而不关心他们的依赖方法是否成功。那么这种机制就非常有用。可以通过添加`"alwaysRun=true"`到`@Test`来实现软依赖。
    硬依赖的例子：
    <pre class="hljs java">`<span class="hljs-meta">@Test</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">serverStartedOk</span><span class="hljs-params">()</span> </span>{}

    <span class="hljs-meta">@Test</span>(dependsOnMethods = { <span class="hljs-string">"serverStartedOk"</span> })
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">method1</span><span class="hljs-params">()</span> </span>{}
    `</pre>

    此例中，`method1()`依赖于方法`serverStartedOk()`，从而保证 `serverStartedOk()`总是先运行。

    也可以让若干方法依赖于组：

    <pre class="hljs css">`@<span class="hljs-keyword">Test</span>(<span class="hljs-keyword">groups</span> = { "<span class="hljs-selector-tag">init</span>" })
    <span class="hljs-selector-tag">public</span> <span class="hljs-selector-tag">void</span> <span class="hljs-selector-tag">serverStartedOk</span>() {}

    @<span class="hljs-keyword">Test</span>(<span class="hljs-keyword">groups</span> = { "<span class="hljs-selector-tag">init</span>" })
    <span class="hljs-selector-tag">public</span> <span class="hljs-selector-tag">void</span> <span class="hljs-selector-tag">initEnvironment</span>() {}

    @<span class="hljs-keyword">Test</span>(<span class="hljs-keyword">dependsOnGroups</span> = { "<span class="hljs-selector-tag">init</span>.* })
    <span class="hljs-selector-tag">public</span> <span class="hljs-selector-tag">void</span> <span class="hljs-selector-tag">method1</span>() {}
    `</pre>

    本例中,`method1()``依赖于匹配正则表达式`"init.*"`的组，由此保证了`serverStartedOk()`和`initEnvironment()`总是先于`method1()`被调用。

    _注意：正如前面所说的那样，在相同组中的调用可是在夸测试中不保证顺序的。_

    如果你使用硬依赖，并且被依赖方法失败(`alwaysRun=false`，即默认是硬依赖)，依赖方法则不是被标记为`FAIL`而是`SKIP`。被跳过的方法会被在最后的报告中标记出来（HTML既不用红色也不是绿色所表示），主要是被跳过的方法不是必然失败，所以被标出来做以区别。

    无论`dependsOnGroups`还是`dependsOnMethods`都可以接受正则表达式作为参数。对于`dependsOnMethods`，如果被依赖的方法有多个重载，那么所有的重载方法都会被调用。如果你只希望使用这些重载中的一个，那么就应该使用 `dependsOnGroups`。

    更多关于依赖方法的例子，请参考这篇[文章](https://link.jianshu.com?t=http://beust.com/weblog/2004/08/19/dependent-test-methods/)，其中也包含了对多重依赖使用继承方式来提供一种优雅的解决方案。

    默认情况下，相关的方法是按类分组。例如，如果方法`a()`依赖于方法的`b()`和你有一个包含这些方法的类的几个实例（数据工厂提供的数据），这些方法的类的几个实例，然后调用顺序将如下：

    <pre class="hljs undefined">`a(1)
    a(2)
    b(2)
    b(2)
    `</pre>

    TestNG的不会运行`b()`，直到所有实例都调用它们的`a()`方法。

    这种行为可能不希望在某些情况下，例如在测试的标志并签署了Web浏览器的各个国家。在这种情况下，你希望下面的顺序：

    <pre class="hljs bash">`signIn(<span class="hljs-string">"us"</span>)
    signOut(<span class="hljs-string">"us"</span>)
    signIn(<span class="hljs-string">"uk"</span>)
    signOut(<span class="hljs-string">"uk"</span>)
    `</pre>

    对于这个顺序，你可以使用XML属性组`group-by-instances`。这个属性可以是在&lt;suite&gt;或&lt;test&gt;有效：

    <pre class="hljs xml">`  <span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Factory"</span> <span class="hljs-attr">order-by-instances</span>=<span class="hljs-string">"true"</span>&gt;</span>
    or
      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Factory"</span> <span class="hljs-attr">order-by-instances</span>=<span class="hljs-string">"true"</span>&gt;</span>
    `</pre>

    ### 5.7.2 - 在XML中依赖的关系

    另外，您也可以在的`testng.xml`文件中指定的组依赖关系。您可以使用&lt;dependencies&gt;标签来实现这一点：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">dependencies</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">group</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"c"</span> <span class="hljs-attr">depends-on</span>=<span class="hljs-string">"a  b"</span> /&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">group</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"z"</span> <span class="hljs-attr">depends-on</span>=<span class="hljs-string">"c"</span> /&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">dependencies</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    ### 5.8 - 工厂

    工厂允许你动态的创建测试。例如，假设你需要创建一个测试方法，并用它来多次访问一个web页面，而且每次都带有不同的参数：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">TestWebServer</span> </span>{
      <span class="hljs-meta">@Test</span>(parameters = { <span class="hljs-string">"number-of-times"</span> })
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">accessPage</span><span class="hljs-params">(<span class="hljs-keyword">int</span> numberOfTimes)</span> </span>{
        <span class="hljs-keyword">while</span> (numberOfTimes-- &gt; <span class="hljs-number">0</span>) {
         <span class="hljs-comment">// access the web page</span>
        }
      }
    }
    `</pre>
    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"T1"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">parameter</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"number-of-times"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"10"</span>/&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>= <span class="hljs-string">"TestWebServer"</span> /&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>

    <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"T2"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">parameter</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"number-of-times"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"20"</span>/&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>= <span class="hljs-string">"TestWebServer"</span>/&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>

    <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"T3"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">parameter</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"number-of-times"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"30"</span>/&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>= <span class="hljs-string">"TestWebServer"</span>/&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    `</pre>

    参数一旦多起来，就难以管理了，所以应该使用工厂来做：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">WebTestFactory</span> </span>{
      <span class="hljs-meta">@Factory</span>
      <span class="hljs-keyword">public</span> Object[] createInstances() {
       Object[] result = <span class="hljs-keyword">new</span> Object[<span class="hljs-number">10</span>]; 
       <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; <span class="hljs-number">10</span>; i++) {
          result[i] = <span class="hljs-keyword">new</span> WebTest(i * <span class="hljs-number">10</span>);
        }
        <span class="hljs-keyword">return</span> result;
      }
    }
    `</pre>

    新的测试类如下：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">WebTest</span> </span>{
      <span class="hljs-keyword">private</span> <span class="hljs-keyword">int</span> m_numberOfTimes;
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">WebTest</span><span class="hljs-params">(<span class="hljs-keyword">int</span> numberOfTimes)</span> </span>{
        m_numberOfTimes = numberOfTimes;
      }

      <span class="hljs-meta">@Test</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testServer</span><span class="hljs-params">()</span> </span>{
       <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i = <span class="hljs-number">0</span>; i &lt; m_numberOfTimes; i++) {
         <span class="hljs-comment">// access the web page</span>
        }
      }
    }
    `</pre>

    你的`testng.xml`只需要引用包含工厂方法的类，而测试实例自己会在运行时创建：

    <pre class="hljs java">`&lt;<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">name</span></span>=<span class="hljs-string">"WebTestFactory"</span> /&gt;
    `</pre>

    工厂方法可以接受诸如`@Test`和`@Before/After`所标记的参数，并且会返回 `Object[]`。这些返回的对象可以是任何类（不一定是跟工厂方法相同的类），并且他们甚至都不需要TestNG注解（在例子中会被TestNG忽略掉）

    工厂也可以被应用于数据提供者，并可以通过将`@Factory`注释无论是在普通的方法或构造函数利用此功能。这里是一个构造工厂的例子：

    <pre class="hljs java">`<span class="hljs-meta">@Factory</span>(dataProvider = <span class="hljs-string">"dp"</span>)
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-title">FactoryDataProviderSampleTest</span><span class="hljs-params">(<span class="hljs-keyword">int</span> n)</span> </span>{
      <span class="hljs-keyword">super</span>(n);
    }

    <span class="hljs-meta">@DataProvider</span>
    <span class="hljs-keyword">static</span> <span class="hljs-keyword">public</span> Object[][] dp() {
      <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Object[][] {
        <span class="hljs-keyword">new</span> Object[] { <span class="hljs-number">41</span> },
        <span class="hljs-keyword">new</span> Object[] { <span class="hljs-number">42</span> },
      };
    }
    `</pre>

    该示例将TestNG的创建两个测试类，这个构造函数将调用值为41和42的对像。

    ### 5.9 - 类级注解

    通常 @Test 也可以用来标注类，而不仅仅是方法：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Test1</span> </span>{
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test1</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test2</span><span class="hljs-params">()</span> </span>{
      }
    }
    `</pre>

    处于类级的`@Test`会使得类中所有的`public`方法成为测试方法，而不管他们是否已经被标注。当然，你仍然可以用`@Test`注解重复标注测试方法，特别是要为其添加一些特别的属性时。

    例如：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">Test1</span> </span>{
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test1</span><span class="hljs-params">()</span> </span>{
      }

      <span class="hljs-meta">@Test</span>(groups = <span class="hljs-string">"g1"</span>)
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">test2</span><span class="hljs-params">()</span> </span>{
      }
    }
    `</pre>

    上例中`test1()`和`test2()`都被处理，不过在此之上`test2()`现在还属于组 "g1"。

    ### 5.10 - 并行运行和超时

    您可以以各种方式指示TestNG单独线程运行测试。

    ### 5.10.1 - 并行套件

    如果你正在运行几个组件文件(`e.g. "java org.testng.TestNG testng1.xml testng2.xml"`)和想让这些组件以一个独立的线程运行这个是非常有用的。你可以使用以下命令行参数来指定线程池的大小。

    <pre class="hljs css">`<span class="hljs-selector-tag">java</span> <span class="hljs-selector-tag">org</span><span class="hljs-selector-class">.testng</span><span class="hljs-selector-class">.TestNG</span> <span class="hljs-selector-tag">-suitethreadpoolsize</span> 3 <span class="hljs-selector-tag">testng1</span><span class="hljs-selector-class">.xml</span> <span class="hljs-selector-tag">testng2</span><span class="hljs-selector-class">.xml</span> <span class="hljs-selector-tag">testng3</span><span class="hljs-selector-class">.xml</span>
    `</pre>

    相应的ant任务名称为_`suitethreadpoolsize`_。

    ### 5.10.2 - 并行测试，类和方法

    你可以通过在suite标签中使用 parallel 属性来让测试方法运行在不同的线程中。这个属性可以带有如下这样的值：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span> <span class="hljs-attr">parallel</span>=<span class="hljs-string">"methods"</span> <span class="hljs-attr">thread-count</span>=<span class="hljs-string">"5"</span>&gt;</span>
    `</pre>
    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span> <span class="hljs-attr">parallel</span>=<span class="hljs-string">"tests"</span> <span class="hljs-attr">thread-count</span>=<span class="hljs-string">"5"</span>&gt;</span>
    `</pre>
    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span> <span class="hljs-attr">parallel</span>=<span class="hljs-string">"classes"</span> <span class="hljs-attr">thread-count</span>=<span class="hljs-string">"5"</span>&gt;</span>
    `</pre>
    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"My suite"</span> <span class="hljs-attr">parallel</span>=<span class="hljs-string">"instances"</span> <span class="hljs-attr">thread-count</span>=<span class="hljs-string">"5"</span>&gt;</span>
    `</pre>

*   **parallel="methods"**: TestNG 会在不同的线程中运行测试方法，除非那些互相依赖的方法。那些相互依赖的方法会运行在同一个线程中，并且遵照其执行顺序。
*   **parallel="tests"**: TestNG 会在相同的线程中运行相同的&lt;test&gt;标记下的所有方法，但是每个&lt;test&gt;标签中的所有方法会运行在不同的线程中。这样就允许你把所有非线程安全的类分组到同一个&lt;test&gt;标签下，并且使其可以利用TestNG多线程的特性的同时，让这些类运行在相同的线程中。
*   **parallel="classes"**: TestNG 会在相同线程中相同类中的运行所有的方法，但是每个类都会用不同的线程运行。
*   **parallel="instances"**: TestNG会在相同线程中相同实例中运行所有的方法，但是两个不同的实例将运行在不同的线程中。

    此外，属性`thread-count`允许你为当前的执行指定可以运行的线程数量。

    _注意：@Test 中的属性`timeOut`可以工作在并行和非并行两种模式下。_

    你也可以指定`@Test`方法在不同的线程中被调用。你可以使用属性 `threadPoolSize`来实现：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>(threadPoolSize = <span class="hljs-number">3</span>, invocationCount = <span class="hljs-number">10</span>,  timeOut = <span class="hljs-number">10000</span>)
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">testServer</span><span class="hljs-params">()</span> </span>{
    `</pre>

    上例中，方法`testServer`会在3个线程中调用10次。此外，10秒钟的超时设定也保证了这三个线程中的任何一个都永远不会阻塞当前被调用的线程。

    ### 5.11 - 再次运行失败的测试

    每次测试`suite`出现失败的测试，TestNG 就会在输出目录中创建一个叫做 `testng-failed.xml`的文件。这个XML文件包含了重新运行那些失败测试的必要信息，使得你可以无需运行整个测试就可以快速重新运行失败的测试。所以，一个典型的会话看起来像：

    要注意的是，`testng-failed.xml`已经包含了所有失败方法运行时需要的依赖，所以完全可以保证上次失败的方法不会出现任何 SKIP。

    ### 5.12 - JUnit测试

    TestNG 能够运行 JUnit 测试。所有要做的工作就是在`testng.classNames`属性中设定要运行的JUnit测试类，并且把`testng.junit`属性设置为`true`：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Test1"</span> <span class="hljs-attr">junit</span>=<span class="hljs-string">"true"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-comment">&lt;!-- ... --&gt;</span>
    `</pre>

    TestNG 在这种情况下所表现的行为与 JUnit 相似：

*   junit3

        *   所有类中要运行的测试方法由 test* 开头
    *   如果类中有 setUp() 方法，则其会在每个测试方法执行前被调用
    *   如果类中有 tearDown() 方法，则其会在每个测试方法之后被调用
    *   如果测试类包含 suite() 方法，则所有的被这个方法返回的测试类都会被调用
*   junit4

        *   TestNG将使用`org.junit.runner.JUnitCore`运行器运行你的测试。

    ### 5.13 - 程序化运行TestNG

    你可以在程序中非常轻松的调用TestNG的测试：

    <pre class="hljs php">`TestListenerAdapter tla = <span class="hljs-keyword">new</span> TestListenerAdapter();
    TestNG testng = <span class="hljs-keyword">new</span> TestNG();
    testng.setTestClasses(<span class="hljs-keyword">new</span> <span class="hljs-class"><span class="hljs-keyword">Class</span>[] </span>{ Run2.class });
    testng.addListener(tla);
    testng.run();
    `</pre>

    本例中创建了一个`TestNG`对象，并且运行测试类`Run2`。它添加了一个 `TestListener`（这是个监听器） 。你既可以使用适配器类 `org.testng.TestListenerAdapter`来做，也可以实现`org.testng.ITestListener`接口。这个接口包含了各种各样的回调方法，能够让你跟踪测试什么时候开始、成功、失败等等。

    类似的你可以用`testng.xml`文件调用或者创建一个虚拟的`testng.xml`文件来调用。为此，你可以使用这个包`org.testng.xml`中的类：`XmlClass、XmlTest`等等。每个类都对应了其在xml中对等的标签。

    例如，假设你要创建下面这样的虚拟文件：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"TmpSuite"</span> &gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"TmpTest"</span> &gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.failures.Child"</span>  /&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">suite</span>&gt;</span>
    `</pre>

    你需要使用如下代码：

    <pre class="hljs php">`XmlSuite suite = <span class="hljs-keyword">new</span> XmlSuite();
    suite.setName(<span class="hljs-string">"TmpSuite"</span>);

    XmlTest test = <span class="hljs-keyword">new</span> XmlTest(suite);
    test.setName(<span class="hljs-string">"TmpTest"</span>);
    <span class="hljs-keyword">List</span>&lt;XmlClass&gt; classes = <span class="hljs-keyword">new</span> ArrayList&lt;XmlClass&gt;();
    classes.add(<span class="hljs-keyword">new</span> XmlClass(<span class="hljs-string">"test.failures.Child"</span>));
    test.setXmlClasses(classes) ;
    `</pre>

    之后你可以传递这个`XmlSuite`给 TestNG：

    <pre class="hljs php">`<span class="hljs-keyword">List</span>&lt;XmlSuite&gt; suites = <span class="hljs-keyword">new</span> ArrayList&lt;XmlSuite&gt;();
    suites.add(suite);
    TestNG tng = <span class="hljs-keyword">new</span> TestNG();
    tng.setXmlSuites(suites);
    tng.run();
    `</pre>

    请参阅[JavaDocs](https://link.jianshu.com?t=http://testng.org/doc/javadocs/org/testng/package-summary.html)来获取完整的API。

    ### 5.14 - BeanShell于高级组选择

    如果`&lt;include&gt;`和`&lt;exclude&gt;`不够用，那就是用`BeanShell`表达式来决定是否一个特定的测试方法应该被包含进来。只要在`&lt;test&gt;`标签下使用这个表达式就好了：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"BeanShell test"</span>&gt;</span>
       <span class="hljs-tag">&lt;<span class="hljs-name">method-selectors</span>&gt;</span>
         <span class="hljs-tag">&lt;<span class="hljs-name">method-selector</span>&gt;</span>
           <span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">language</span>=<span class="hljs-string">"beanshell"</span>&gt;</span><span class="handlebars"><span class="xml">&lt;![CDATA[
             groups.containsKey("test1")
           ]]&gt;</span></span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span>
         <span class="hljs-tag">&lt;/<span class="hljs-name">method-selector</span>&gt;</span>
       <span class="hljs-tag">&lt;/<span class="hljs-name">method-selectors</span>&gt;</span>
      <span class="hljs-comment">&lt;!-- ... --&gt;</span>
    `</pre>

    当在`testng.xml`文件中找到`&lt;script&gt;`标签后，TestNG 就会忽略在当前&lt;test&gt;标签内组和方法的的`&lt;include&gt;`和`&lt;exclude&gt;`标签：`BeanShell`就会成为测试方法是否被包含的唯一决定因素。

    下面是关于BeanShell 脚本的额外说明：

*   必须返回一个`boolean`值。除了这个约束以外，任何有效的`BeanShell`代码都是允许的 (例如，可能需要在工作日返回`true`而在周末返回`false`，这样就允许你可以依照不同的日期进行测试）。
*   TestNG 定义了如下的变量供你调用：
    **java.lang.reflect.Method method:**  当前的测试方法
    **org.testng.ITestNGMethod testngMethod:**  当前测试方法的描述
    **java.util.Map&lt;String, String&gt; groups:**  当前测试方法所属组的映射
*   你也可能需要使用 CDATA 声明来括起Beanshell表达式（就像上例）来避免对XML保留字冗长的引用

    ### 5.15 - 注解转换器

    TestNG 允许你在运行时修改所有注解的内容。在源码中注解大多数时候都能正常工作的时时非常有用的（这句原文就是这意思，感觉不太对劲），但是有几个情况你可能会改变其中的值。

    为此，你会用到注解转换器（ Annotation Transformer ）。

    所谓注解转换器，就是实现了下面接口的类：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">interface</span> <span class="hljs-title">IAnnotationTransformer</span> </span>{

      <span class="hljs-comment">/**
       * This method will be invoked by TestNG to give you a chance
       * to modify a TestNG annotation read from your test classes.
       * You can change the values you need by calling any of the
       * setters on the ITest interface.
       *
       * Note that only one of the three parameters testClass,
       * testConstructor and testMethod will be non-null.
       *
       * <span class="hljs-doctag">@param</span> annotation The annotation that was read from your
       * test class.
       * <span class="hljs-doctag">@param</span> testClass If the annotation was found on a class, this
       * parameter represents this class (null otherwise).
       * <span class="hljs-doctag">@param</span> testConstructor If the annotation was found on a constructor,
       * this parameter represents this constructor (null otherwise).
       * <span class="hljs-doctag">@param</span> testMethod If the annotation was found on a method,
       * this parameter represents this method (null otherwise).
       */</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">transform</span><span class="hljs-params">(ITest annotation, Class testClass,
          Constructor testConstructor, Method testMethod)</span></span>;
    }
    `</pre>

    像其他的TestNG 监听器，你可以指定在命令行或者通过`ant`来指定这个类：

    <pre class="hljs css">`<span class="hljs-selector-tag">java</span> <span class="hljs-selector-tag">org</span><span class="hljs-selector-class">.testng</span><span class="hljs-selector-class">.TestNG</span> <span class="hljs-selector-tag">-listener</span> <span class="hljs-selector-tag">MyTransformer</span> <span class="hljs-selector-tag">testng</span><span class="hljs-selector-class">.xml</span>
    `</pre>

    或者在程序中：

    <pre class="hljs cpp">`TestNG tng = <span class="hljs-keyword">new</span> TestNG();
    tng.setAnnotationTransformer(<span class="hljs-keyword">new</span> MyTransformer());
    <span class="hljs-comment">// ...</span>
    `</pre>

    当调用`transform()`的时候，你可以调用任何在 ITest test 参数中的设置方法来在进一步处理之前改变它的值。

    例如，这里是你如何覆盖属性`invocationCount`的值，但是只有测试类中的`invoke()`方法受影响：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">MyTransformer</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">IAnnotationTransformer</span> </span>{
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">transform</span><span class="hljs-params">(ITest annotation, Class testClass,
          Constructor testConstructor, Method testMethod)</span>
      </span>{
        <span class="hljs-keyword">if</span> (<span class="hljs-string">"invoke"</span>.equals(testMethod.getName())) {
          annotation.setInvocationCount(<span class="hljs-number">5</span>);
        }
      }
    }
    `</pre>

    `IAnnotationTransformer`只允许你修改一个`@Test`注解。如果你需要修改其他的（假设说配置注解`@Factory`或`@DataProvider`），使用 `IAnnotationTransformer2`。

    ### 5.16 - 方法拦截器

    一旦TestNG 计算好了测试方法会以怎样的顺序调用，那么这些方法就会分为两组：

*   _按照顺序运行的方法_。这里所有的方法都有相关的依赖，并且所有这些方法按照特定顺序运行。
*   _不定顺序运行的方法_。这里的方法不属于第一个类别。方法的运行顺序是随机的，下一个说不准是什么（尽管如此，默认情况下TestNG会尝试通过类来组织方法）。

    为了能够让你更好的控制第二种类别，TestNG定义如下接口：

    <pre class="hljs php">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">interface</span> <span class="hljs-title">IMethodInterceptor</span> </span>{

      <span class="hljs-keyword">List</span>&lt;IMethodInstance&gt; intercept(<span class="hljs-keyword">List</span>&lt;IMethodInstance&gt; methods, ITestContext context);

    }
    `</pre>

    方法中叫做methods的那个列表参数包含了所有以不定序运行的方法。你的 `intercept`方法也要返回一个`IMethodInstance`列表，它可能是下面情况之一：

*   内容与参数中接收的一致，但是顺序不同
*   一组`IMethodInstance`对象
*   更大的一组`IMethodInstance`对象

    一旦你定义了拦截器，就把它传递个TestNG，用下面的方式：

    <pre class="hljs bash">`java -classpath <span class="hljs-string">"testng-jdk15.jar:test/build"</span> org.testng.TestNG -listener test.methodinterceptors.NullMethodInterceptor
       -testclass test.methodinterceptors.FooTest
    `</pre>

    关于 ant 中对应的语法，参见`listeners`属性ant文档 中的说明。

    例如，下面是个方法拦截器会重新给方法排序，一遍`“fast”`组中的方法总是先执行：

    <pre class="hljs javascript">`public List&lt;IMethodInstance&gt; intercept(List&lt;IMethodInstance&gt; methods, ITestContext context) {
      List&lt;IMethodInstance&gt; result = <span class="hljs-keyword">new</span> ArrayList&lt;IMethodInstance&gt;();
      <span class="hljs-keyword">for</span> (IMethodInstance m : methods) {
        Test test = m.getMethod().getConstructorOrMethod().getAnnotation(Test.class);
        <span class="hljs-built_in">Set</span>&lt;<span class="hljs-built_in">String</span>&gt; groups = <span class="hljs-keyword">new</span> HashSet&lt;<span class="hljs-built_in">String</span>&gt;();
        <span class="hljs-keyword">for</span> (<span class="hljs-built_in">String</span> group : test.groups()) {
          groups.add(group);
        }
        <span class="hljs-keyword">if</span> (groups.contains(<span class="hljs-string">"fast"</span>)) {
          result.add(<span class="hljs-number">0</span>, m);
        }
        <span class="hljs-keyword">else</span> {
          result.add(m);
        }
      }
      <span class="hljs-keyword">return</span> result;
    }
    `</pre>

    ### 5.17 - TestNG 监听器

    有很多接口可以用来修改 TestNG 的行为。这些接口都被统称为 "TestNG 监听器"。下面是目前支持的监听器的列表：

*   **IAnnotationTransformer** ([doc](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#annotationtransformers), [javadoc](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/IAnnotationTransformer.html))
*   **IReporter** ([doc](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#logging-reporters), [javadoc](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/IReporter.html))
*   **ITestListener** ([doc](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#logging-listeners), [javadoc](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/ITestListener.html))
*   **IMethodInterceptor** ([doc](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#methodinterceptors), [javadoc](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/IMethodInterceptor.html))
*   **IInvokedMethodListener** ([doc](https://link.jianshu.com?t=http://testng.org/doc/documentation-main.html#invokedmethodlistener), [javadoc](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/IInvokedMethodListener.html))

    当你实现了这些接口，你可以让 TestNG 知道这些接口，有如下方式：

*   在命令行下使用 `-listener`
*   ant中使用`&lt;listeners&gt;`
*   在`testng.xml`中使用`&lt;listeners&gt;`标签
*   在测试类上述用`@Listeners`注释
*   使用`ServiceLoader`

    ### 5.17.1 - 在testng.xml或者java中制定监听

    下面就是你如何在testng.xml中定义监听：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">suite</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">listeners</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">listener</span> <span class="hljs-attr">class-name</span>=<span class="hljs-string">"com.example.MyListener"</span> /&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">listener</span> <span class="hljs-attr">class-name</span>=<span class="hljs-string">"com.example.MyMethodInterceptor"</span> /&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">listeners</span>&gt;</span>

    ...
    `</pre>

    或者你喜欢在java中定义的监听：

    <pre class="hljs java">`<span class="hljs-meta">@Listeners</span>({ com.example.MyListener.class, com.example.MyMethodInterceptor.class })
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">MyTest</span> </span>{
      <span class="hljs-comment">// ...</span>
    }
    `</pre>

    该`@Listeners`注释可以包含扩展`org.testng.ITestNGListener`除了`IAnnotationTransformer`和`IAnnotationTransformer2`任何类。其原因是，这些监听需要在过程中很早就知道以便TestNG可以使用它们来重写你的注解，因此，你需要在你的`testng.xml`文件中指定这些侦听器。

    请注意`@Listeners`注释将适用于整个套件的文件，就像如果你已经指定在一个的`testng.xml`文件。如果你想限制它的范围（例如，仅运行在当前类），在监听的代码可以先检查测试方法的运行，并决定该怎么做呢。

    ### 5.17.2 - 书写服务加载监听

    最后，JDK中提供了一个非常优雅的机制，通过ServiceLoader类的借口路径来实现监听。

    随着`ServiceLoader`，这一切你所需要做的就是创建一个包含你的监听和一些配置文件的JAR文件，把这个jar文件放在classpath中运行TestNG并TestNG会自动找到它们。

    下面是它如何工作的具体例子。

    让我们先创建一个侦听器（所有的TetstNG监听都应该响应）：

    <pre class="hljs java">`<span class="hljs-keyword">package</span> test.tmp;

    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">TmpSuiteListener</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">ISuiteListener</span> </span>{
      <span class="hljs-meta">@Override</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">onFinish</span><span class="hljs-params">(ISuite suite)</span> </span>{
        System.out.println(<span class="hljs-string">"Finishing"</span>);
      }

      <span class="hljs-meta">@Override</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">onStart</span><span class="hljs-params">(ISuite suite)</span> </span>{
        System.out.println(<span class="hljs-string">"Starting"</span>);
      }
    }
    `</pre>

    编译这个文件，然后在当前文件位置创建一个文件`META-INF/services/org.testng.ITestNGListener`这个名字就是你要实现的接口。

    最后您应该知道如下的目录结构，只有仅仅两个文件：

    <pre class="hljs ruby">`$ tree
    <span class="hljs-params">|____META-INF
    |</span> <span class="hljs-params">|____services
    |</span> <span class="hljs-params">| |</span>____org.testng.ITestNGListener
    <span class="hljs-params">|____test
    |</span> <span class="hljs-params">|____tmp
    |</span> <span class="hljs-params">| |</span>____TmpSuiteListener.<span class="hljs-keyword">class</span>

    $ cat META-INF/services/org.testng.ITestNGListener
    test.tmp.TmpSuiteListener
    `</pre>

    在这个目录创建一个jar文件如下：

    <pre class="hljs objectivec">`$ jar cvf ../sl.jar .
    added manifest
    ignoring entry META-INF/
    adding: META-INF/services/(<span class="hljs-keyword">in</span> = <span class="hljs-number">0</span>) (<span class="hljs-keyword">out</span>= <span class="hljs-number">0</span>)(stored <span class="hljs-number">0</span>%)
    adding: META-INF/services/org.testng.ITestNGListener(<span class="hljs-keyword">in</span> = <span class="hljs-number">26</span>) (<span class="hljs-keyword">out</span>= <span class="hljs-number">28</span>)(deflated <span class="hljs-number">-7</span>%)
    adding: test/(<span class="hljs-keyword">in</span> = <span class="hljs-number">0</span>) (<span class="hljs-keyword">out</span>= <span class="hljs-number">0</span>)(stored <span class="hljs-number">0</span>%)
    adding: test/tmp/(<span class="hljs-keyword">in</span> = <span class="hljs-number">0</span>) (<span class="hljs-keyword">out</span>= <span class="hljs-number">0</span>)(stored <span class="hljs-number">0</span>%)
    adding: test/tmp/TmpSuiteListener.class(<span class="hljs-keyword">in</span> = <span class="hljs-number">849</span>) (<span class="hljs-keyword">out</span>= <span class="hljs-number">470</span>)(deflated <span class="hljs-number">44</span>%)
    `</pre>

    接着，把你的jar文件放在当前你调用TestNG的`classpath`上：

    <pre class="hljs ruby">`$ java -classpath sl.<span class="hljs-symbol">jar:</span>testng.jar org.testng.TestNG testng-single.yaml
    Starting
    f2 <span class="hljs-number">11</span> <span class="hljs-number">2</span>
    <span class="hljs-symbol">PASSED:</span> f2(<span class="hljs-string">"2"</span>)
    Finishing
    `</pre>

    这种机制允许你在同一组侦听器适用于整个组织只需增加一个JAR文件添加到类路径，而不是要求每一个开发者要记得在他们的`testng.xml`文件中指定这些侦听器。

    ### 5.18 - 依赖注入

    TestNG的支持两种不同类型的依赖注入：本机（由TestNG的本身执行）和外部（通过依赖注入框架，如Guice）。

    ### 5.18.1 - 本地依赖注入

    TestNG 允许你在自己的方法中声明额外的参数。这时，TestNG会自动使用正确的值填充这些参数。依赖注入就使用在这种地方：

*   任何`@Before`或`@Test`方法可以声明一个类型为 `ITestContext`的参数。
*   任何`@After`都可以声明一个类型为`ITestResult`的单数，它代表了刚刚运行的测试方法。
*   任何`@Before`和`@After`方法都能够声明类型为`XmlTest` 的参数，它包含了当前的`&lt;test&gt;`参数。
*   任何`@BeforeMethod`可以声明一个类型为 `java.lang.reflect.Method`的参数。这个参数会接收 `@BeforeMethod`完成调用的时候马上就被调用的那个测试方法当做它的值。
*   任何`@BeforeMethod`可以声明一个类型为`Object[]`的参数。这个参数会包含要被填充到下一个测试方法中的参数的列表，它既可以又 TestNG 注入，例如`java.lang.reflect.Method`或者来自`@DataProvider`。
*   任何`@DataProvider`可以声明一个类型为`ITestContext` 或`java.lang.reflect.Method`的参数。后一种类型的参数，会收到即将调用的方法作为它的值。

    你可以使用@NoInjection关闭注释：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">NoInjectionTest</span> </span>{

      <span class="hljs-meta">@DataProvider</span>(name = <span class="hljs-string">"provider"</span>)
      <span class="hljs-keyword">public</span> Object[][] provide() <span class="hljs-keyword">throws</span> Exception {
          <span class="hljs-keyword">return</span> <span class="hljs-keyword">new</span> Object[][] { { CC.class.getMethod(<span class="hljs-string">"f"</span>) } };
      }

      <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"provider"</span>)
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">withoutInjection</span><span class="hljs-params">(@NoInjection Method m)</span> </span>{
          Assert.assertEquals(m.getName(), <span class="hljs-string">"f"</span>);
      }

      <span class="hljs-meta">@Test</span>(dataProvider = <span class="hljs-string">"provider"</span>)
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">withInjection</span><span class="hljs-params">(Method m)</span> </span>{
          Assert.assertEquals(m.getName(), <span class="hljs-string">"withInjection"</span>);
      }
    }
    `</pre>

    ### 5.18.2 - Guice依赖注入

    如果您使用Guice，TestNG给你一个简单的方法来与Guice模块注入测试对象：

    <pre class="hljs java">`<span class="hljs-meta">@Guice</span>(modules = GuiceExampleModule.class)
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">GuiceTest</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">SimpleBaseTest</span> </span>{

      <span class="hljs-meta">@Inject</span>
      ISingleton m_singleton;

      <span class="hljs-meta">@Test</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">singletonShouldWork</span><span class="hljs-params">()</span> </span>{
        m_singleton.doSomething();
      }

    }
    `</pre>

    在这个例子中，`GuiceExampleModule`被预期绑定到`ISingleton`借口的一些具体的类：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">GuiceExampleModule</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">Module</span> </span>{

      <span class="hljs-meta">@Override</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">configure</span><span class="hljs-params">(Binder binder)</span> </span>{
        binder.bind(ISingleton.class).to(ExampleSingleton.class).in(Singleton.class);
      }

    }
    `</pre>

    如果在指定的模块实例化你的测试类需要更多的灵活性，您可以指定一个模块工厂：

    <pre class="hljs java">`<span class="hljs-meta">@Guice</span>(moduleFactory = ModuleFactory.class)
    <span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">GuiceModuleFactoryTest</span> </span>{

      <span class="hljs-meta">@Inject</span>
      ISingleton m_singleton;

      <span class="hljs-meta">@Test</span>
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">singletonShouldWork</span><span class="hljs-params">()</span> </span>{
        m_singleton.doSomething();
      }
    }
    `</pre>

    模块工厂需要实现该[`IModuleFactory`借](https://link.jianshu.com?t=http://testng.org/javadocs/org/testng/IModuleFactory.html)口：

    <pre class="hljs php">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">interface</span> <span class="hljs-title">IModuleFactory</span> </span>{
     <span class="hljs-comment">/**
       * <span class="hljs-doctag">@param</span> context The current test context
       * <span class="hljs-doctag">@param</span> testClass The test class
       *
       * <span class="hljs-doctag">@return</span> The Guice module that should be used to get an instance of this
       * test class.
       */</span>
      Module createModule(ITestContext context, <span class="hljs-class"><span class="hljs-keyword">Class</span>&lt;?&gt; <span class="hljs-title">testClass</span>);
    }
    </span>`</pre>

    你的工厂将会通过了测试环境和TestNG需要实例化测试类的一个实例。你的`createModule`方法应该返回一个Guice模块，就会知道如何实例化这个测试类。您可以使用测试环境，以了解您的环境的详细信息，如在规定的`testng.xml`设置参数等...

    ### 5.19 - 监听方法调用

    无论何时TestNG即将调用一个测试（被`@Test`注解的）或者配置（任何使用`@Before`or`@After`注解标注的方法），监听器 `IInvokedMethodListener`都可以让你得到通知。你所要做的就是实现如下接口：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">interface</span> <span class="hljs-title">IInvokedMethodListener</span> <span class="hljs-keyword">extends</span> <span class="hljs-title">ITestNGListener</span> </span>{
      <span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">beforeInvocation</span><span class="hljs-params">(IInvokedMethod method, ITestResult testResult)</span></span>;
      <span class="hljs-function"><span class="hljs-keyword">void</span> <span class="hljs-title">afterInvocation</span><span class="hljs-params">(IInvokedMethod method, ITestResult testResult)</span></span>;
    }
    `</pre>

    并且就像在 关于TestNG监听器一节 中所讨论的那样，将其声明为一个监听器。

    ### 5.20 - 重写测试方法

    TestNG的允许你覆盖和跳过测试方法的调用。在那里，这是非常有用的一个例子，如果你需要你的测试方法需要一个特定的安全管理。通过提供实现`IHookable`一个监听器，你就可以做到这一点。

    下面是用JAAS(Java验证和授权API)一个例子：

    <pre class="hljs java">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">MyHook</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">IHookable</span> </span>{
      <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">run</span><span class="hljs-params">(<span class="hljs-keyword">final</span> IHookCallBack icb, ITestResult testResult)</span> </span>{
        <span class="hljs-comment">// Preferably initialized in a @Configuration method</span>
        mySubject = authenticateWithJAAs();

        Subject.doAs(mySubject, <span class="hljs-keyword">new</span> PrivilegedExceptionAction() {
          <span class="hljs-function"><span class="hljs-keyword">public</span> Object <span class="hljs-title">run</span><span class="hljs-params">()</span> </span>{
            icb.callback(testResult);
          }
        };
      }
    }
    `</pre>

    ## 6 - 测试结果

    ### 6.1 - 成功、失败和断言

    如果一个测试没有抛出任何异常就完成运行或者说抛出了期望的异常（参见`@Test`注解的`expectedExceptions`属性文档），就说，这个测试是成功的。

    测试方法的组成常常包括抛出多个异常，或者包含各种各样的断言（使用Java `"assert"`关键字）。一个`"assert"`失败会触发一个`AssertionErrorException`，结果就是测试方法被标记为失败（记住，如果你看不到断言错误，要在加上`-ea`这个JVM参数）。

    下面是个例子：

    <pre class="hljs java">`<span class="hljs-meta">@Test</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">verifyLastName</span><span class="hljs-params">()</span> </span>{
      <span class="hljs-keyword">assert</span> <span class="hljs-string">"Beust"</span>.equals(m_lastName) : <span class="hljs-string">"Expected name Beust, for"</span> + m_lastName;
    }
    `</pre>

    TestNG 也包括 JUnit 的`Assert`类，允许你对复杂的对象执行断言：

    <pre class="hljs java">`<span class="hljs-keyword">import</span> <span class="hljs-keyword">static</span> org.testng.AssertJUnit.*;
    <span class="hljs-comment">//...</span>
    <span class="hljs-meta">@Test</span>
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">verify</span><span class="hljs-params">()</span> </span>{
      assertEquals(<span class="hljs-string">"Beust"</span>, m_lastName);
    }
    `</pre>

    注意，上述代码使用了静态导入，以便能够使用`assertEquals` 方法，而无需加上它的类前缀。

    ### 6.2 - 日志与结果

    当运行`SuiteRunner`的时候会指定一个目录，之后测试的结果都会保存在一个在那个目录中叫做`index.html`的文件中。这个`index`文件指向其他多个HTML和文本文件，被指向的文件包含了整个测试的结果。你可以再 这里 看到一个例子。

    通过使用监听器和报表器，可以很轻松的生成自己的TestNG报表：

*   监听器 实现接口`org.testng.ITestListener`，并且会在测试开始、通过、失败等时刻实时通知
*   报告器 实现接口`org.testng.IReporter`，并且当整个测试运行完毕之后才会通知。`IReporter`接受一个对象列表，这些对象描述整个测试运行的情况

    例如，如果你想要生成一个PDF报告，那么就不需要实时通知，所以用`IReporter`。如果需要写一个实时报告，例如用在GUI上，还要在每次测试时（下面会有例子和解释）有进度条或者文本报告显示点 (".")，那么`ITestListener`是你最好的选择。

    #### 6.2.1 - 日志监听器

    这里是对每个传递进来的测试显示"."的监听器，如果测试失败则显示 "F" ，跳过则是"S"：

    <pre class="hljs cpp">`<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">DotTestListener</span> <span class="hljs-title">extends</span> <span class="hljs-title">TestListenerAdapter</span> {</span>
      <span class="hljs-keyword">private</span> <span class="hljs-keyword">int</span> m_count = <span class="hljs-number">0</span>;

      @<span class="hljs-function">Override
      <span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">onTestFailure</span><span class="hljs-params">(ITestResult tr)</span> </span>{
        <span class="hljs-built_in">log</span>(<span class="hljs-string">"F"</span>);
      }

      @<span class="hljs-function">Override
      <span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">onTestSkipped</span><span class="hljs-params">(ITestResult tr)</span> </span>{
        <span class="hljs-built_in">log</span>(<span class="hljs-string">"S"</span>);
      }

      @<span class="hljs-function">Override
      <span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">onTestSuccess</span><span class="hljs-params">(ITestResult tr)</span> </span>{
        <span class="hljs-built_in">log</span>(<span class="hljs-string">"."</span>);
      }

      <span class="hljs-function"><span class="hljs-keyword">private</span> <span class="hljs-keyword">void</span> <span class="hljs-title">log</span><span class="hljs-params">(String <span class="hljs-built_in">string</span>)</span> </span>{
        System.out.print(<span class="hljs-built_in">string</span>);
        <span class="hljs-keyword">if</span> (++m_count % <span class="hljs-number">40</span> == <span class="hljs-number">0</span>) {
          System.out.println(<span class="hljs-string">""</span>);
        }
      }
    }
    `</pre>

    上例中，我们选择扩展`TestListenerAdapter`，它使用空方法实现了`ITestListener`。所以我不需要去重写那些我不需要的方法。如果喜欢可以直接实现接口。

    这里是我使用这个新监听器调用TestNG的例子：

    <pre class="hljs css">`<span class="hljs-selector-tag">java</span> <span class="hljs-selector-tag">-classpath</span> <span class="hljs-selector-tag">testng</span><span class="hljs-selector-class">.jar</span>;%<span class="hljs-selector-tag">CLASSPATH</span>% <span class="hljs-selector-tag">org</span><span class="hljs-selector-class">.testng</span><span class="hljs-selector-class">.TestNG</span> <span class="hljs-selector-tag">-listener</span> <span class="hljs-selector-tag">org</span><span class="hljs-selector-class">.testng</span><span class="hljs-selector-class">.reporters</span><span class="hljs-selector-class">.DotTestListener</span> <span class="hljs-selector-tag">test</span>\<span class="hljs-selector-tag">testng</span><span class="hljs-selector-class">.xml</span>
    `</pre>

    输出是：

    <pre class="hljs undefined">`........................................
    ........................................
    ........................................
    ........................................
    ........................................
    .........................
    ===============================================
    TestNG JDK 1.5
    Total tests run: 226, Failures: 0, Skips: 0
    ===============================================
    `</pre>

    _注意，当你使用`-listener`的时候，TestNG 会自动的检测你所使用的监听器类型。_

    #### 6.2.2 - 日志报表

    `org.testng.IReporter`接口只有一个方法：

    <pre class="hljs cpp">`<span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">generateReport</span><span class="hljs-params">(List&lt;ISuite&gt; suites, String outputDirectory)</span>
    </span>`</pre>

    这个方法在 TestNG 中的所有测试都运行完毕之后被调用，这样你可以方法这个方法的参数，并且通过它们获得刚刚完成的测试的所有信息。

    #### 6.2.3 - JUnit 报表

    TestNG 包含了一个可以让TestNG的结果和输出的XML能够被`JUnitReport`所使用的监听器。这里 有个例子，并且ant任务创建了这个报告：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">target</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"reports"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">junitreport</span> <span class="hljs-attr">todir</span>=<span class="hljs-string">"test-report"</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">fileset</span> <span class="hljs-attr">dir</span>=<span class="hljs-string">"test-output"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">include</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"*/*.xml"</span>/&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">fileset</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">report</span> <span class="hljs-attr">format</span>=<span class="hljs-string">"noframes"</span>  <span class="hljs-attr">todir</span>=<span class="hljs-string">"test-report"</span>/&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">junitreport</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">target</span>&gt;</span>
    `</pre>

    #### 6.2.4 - 报表 API

    如果你要在HTML报告中显示日志信息，那么就要用到类 `org.testng.Reporter：Reporter.log("M3 WAS CALLED");`


    ![](http://langhai.qiniudn.com/show-output1.png)


    ![](http://langhai.qiniudn.com/show-output2.png)

    #### 6.2.5 - XML 报表

    TestNG 提供一种XML报表器，使得能够捕捉到只适用于TestNG而不适用与JUnit报表的那些特定的信息。这在用户的测试环境必须要是用TestNG特定信息的XML，而JUnit又不能够提供这些信息的时候非常有用。下面就是这种报表器生成XML的一个例子：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">testng-results</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Suite1"</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">group</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"group1"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">method</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"com.test.TestOne.test2()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test2"</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"com.test.TestOne"</span>/&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">method</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"com.test.TestOne.test1()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test1"</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"com.test.TestOne"</span>/&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">group</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">group</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"group2"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">method</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"com.test.TestOne.test2()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test2"</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"com.test.TestOne"</span>/&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">group</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test1"</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"com.test.TestOne"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">test-method</span> <span class="hljs-attr">status</span>=<span class="hljs-string">"FAIL"</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"test1()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test1"</span> <span class="hljs-attr">duration-ms</span>=<span class="hljs-string">"0"</span>
                  <span class="hljs-attr">started-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span> <span class="hljs-attr">description</span>=<span class="hljs-string">"someDescription2"</span>
                  <span class="hljs-attr">finished-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span>&gt;</span>
              <span class="hljs-tag">&lt;<span class="hljs-name">exception</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"java.lang.AssertionError"</span>&gt;</span>
                <span class="hljs-tag">&lt;<span class="hljs-name">short-stacktrace</span>&gt;</span>java.lang.AssertionError
                  ... Removed 22 stack frames
                <span class="hljs-tag">&lt;/<span class="hljs-name">short-stacktrace</span>&gt;</span>
              <span class="hljs-tag">&lt;/<span class="hljs-name">exception</span>&gt;</span>
            <span class="hljs-tag">&lt;/<span class="hljs-name">test-method</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">test-method</span> <span class="hljs-attr">status</span>=<span class="hljs-string">"PASS"</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"test2()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test2"</span> <span class="hljs-attr">duration-ms</span>=<span class="hljs-string">"0"</span>
                  <span class="hljs-attr">started-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span> <span class="hljs-attr">description</span>=<span class="hljs-string">"someDescription1"</span>
                  <span class="hljs-attr">finished-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span>&gt;</span>
            <span class="hljs-tag">&lt;/<span class="hljs-name">test-method</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">test-method</span> <span class="hljs-attr">status</span>=<span class="hljs-string">"PASS"</span> <span class="hljs-attr">signature</span>=<span class="hljs-string">"setUp()"</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"setUp"</span> <span class="hljs-attr">is-config</span>=<span class="hljs-string">"true"</span> <span class="hljs-attr">duration-ms</span>=<span class="hljs-string">"15"</span>
                  <span class="hljs-attr">started-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span> <span class="hljs-attr">finished-at</span>=<span class="hljs-string">"2007-05-28T12:14:37Z"</span>&gt;</span>
            <span class="hljs-tag">&lt;/<span class="hljs-name">test-method</span>&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">class</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">suite</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">testng-results</span>&gt;</span>

这个报表器是随着默认监听器一起诸如的，所以你默认的情况下就可以得到这种类型的输出。这个监听器提供了一些属性，可以修改报表来满足你的需要。下表包含了这些属性，并做简要说明：

<table>
<thead>
<tr>
<th style="text-align:left">属性</th>
<th style="text-align:left">说明</th>
<th style="text-align:left">Default value</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left">outputDirectory</td>
<td style="text-align:left">一个`String`指明了XML文件要存放的目录</td>
<td style="text-align:left">TestNG 输出目录</td>
</tr>
<tr>
<td style="text-align:left">timestampFormat</td>
<td style="text-align:left">指定报表器生成日期字段的格式</td>
<td style="text-align:left">yyyy-MM-dd'T'HH:mm:ss'Z'</td>
</tr>
<tr>
<td style="text-align:left">fileFragmentationLevel</td>
<td style="text-align:left">值为1，2或3的整数，指定了XML文件的生成方式：1 - 在一个文件里面生成所有的结果。2 - 每套测试都单独生成一个XML文件，这些文件被链接到一个主文件。3 - 同2，不过添加了引用那些套测试的测试用例的XML文件。</td>
<td style="text-align:left">1</td>
</tr>
<tr>
<td style="text-align:left">splitClassAndPackageNames</td>
<td style="text-align:left">
`boolean`值，指明了对`&lt;class&gt;`元素的生成方式。例如，你在`false`的时候得到`&lt;class class="com.test.MyTest"&gt;`，在`true的`时候`&lt;class class="MyTest" package="com.test"&gt;`。</td>
<td style="text-align:left">false</td>
</tr>
<tr>
<td style="text-align:left">generateGroupsAttribute</td>
<td style="text-align:left">
`boolean`值指定了对`&lt;test-method&gt;`元素是否生成`groups` 属性。这个功能的目的是，对那些无需遍历整个`&lt;group&gt;`元素，且只含有一个测试方法的组来说，可以提供一种更直接的解析组的方式。</td>
<td style="text-align:left">false</td>
</tr>
<tr>
<td style="text-align:left">stackTraceOutputMethod</td>
<td style="text-align:left">指定在发生异常的时候生成异常，追踪弹栈信息的类型，有如下可选值：0 - 无弹栈信息 (只有异常类和消息)。1 - 尖端的弹栈信息，从上到下只有几行。2 - 带有所有内部异常的完整的弹栈方式。3 - 短长两种弹栈方式</td>
<td style="text-align:left">2</td>
</tr>
<tr>
<td style="text-align:left">generateDependsOnMethods</td>
<td style="text-align:left">对于`&lt;test-method&gt;`元素，使用这个属性来开启/关闭 `depends-on-methods`属性。</td>
<td style="text-align:left">true</td>
</tr>
<tr>
<td style="text-align:left">generateDependsOnGroups</td>
<td style="text-align:left">对于`&lt;test-method&gt;`属性开启`depends-on-groups`属性。</td>
<td style="text-align:left">true</td>
</tr>
</tbody>
</table>

为了配置报表器，你可以在命令行下使用`-reporter`选项，或者在 Ant 任务中嵌入`&lt;reporter&gt;`元素。对于每个情况，你都必须指明类`org.testng.reporters.XMLReporter`。但是要注意你不能够配置内建的报表器，因为这个只使用默认配置。如果你的确需要XML报表，并且使用自定义配置，那么你就不得不手工完成。可以通过自己添加一两个方法，并且禁用默认监听器达到目的。




* * *

## 接上面的文章：

## 7 - YAML

TestNG的支持YAML作为指定的套件文件的另一种方法。例如，下面的XML文件：

    <span class="hljs-tag">&lt;<span class="hljs-name">suite</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"SingleSuite"</span> <span class="hljs-attr">verbose</span>=<span class="hljs-string">"2"</span> <span class="hljs-attr">thread-count</span>=<span class="hljs-string">"4"</span>&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">parameter</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"n"</span> <span class="hljs-attr">value</span>=<span class="hljs-string">"42"</span> /&gt;</span>

      <span class="hljs-tag">&lt;<span class="hljs-name">test</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"Regression2"</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">groups</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">run</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">exclude</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"broken"</span> /&gt;</span>
          <span class="hljs-tag">&lt;/<span class="hljs-name">run</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">groups</span>&gt;</span>

        <span class="hljs-tag">&lt;<span class="hljs-name">classes</span>&gt;</span>
          <span class="hljs-tag">&lt;<span class="hljs-name">class</span> <span class="hljs-attr">name</span>=<span class="hljs-string">"test.listeners.ResultEndMillisTest"</span> /&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">classes</span>&gt;</span>
      <span class="hljs-tag">&lt;/<span class="hljs-name">test</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">suite</span>&gt;</span>
    `</pre>
    <pre class="hljs css">`<span class="hljs-selector-tag">name</span>: <span class="hljs-selector-tag">SingleSuite</span>
    <span class="hljs-selector-tag">threadCount</span>: 4
    <span class="hljs-selector-tag">parameters</span>: { <span class="hljs-attribute">n</span>: <span class="hljs-number">42</span> }

    <span class="hljs-selector-tag">tests</span>:
      <span class="hljs-selector-tag">-</span> <span class="hljs-selector-tag">name</span>: <span class="hljs-selector-tag">Regression2</span>
        <span class="hljs-selector-tag">parameters</span>: { <span class="hljs-attribute">count</span>: <span class="hljs-number">10</span> }
        <span class="hljs-selector-tag">excludedGroups</span>: <span class="hljs-selector-attr">[ broken ]</span>
        <span class="hljs-selector-tag">classes</span>:
          <span class="hljs-selector-tag">-</span> <span class="hljs-selector-tag">test</span><span class="hljs-selector-class">.listeners</span><span class="hljs-selector-class">.ResultEndMillisTest</span>
    `</pre>

    这里是[TestNG的自己的一套文件](https://link.jianshu.com?t=https://github.com/cbeust/testng/blob/master/src/test/resources/testng.xml)，YAML的对应的[版本](https://link.jianshu.com?t=https://github.com/cbeust/testng/blob/master/src/test/resources/testng.yaml)。

    您可能会发现YAML文件格式更容易阅读和维护。 YAML文件也Eclipse插件的插件认可。您可以在[这篇博客文章](https://link.jianshu.com?t=http://beust.com/weblog/2010/08/15/yaml-the-forgotten-victim-of-the-format-wars/)找到有关YAML和TestNG更详细的信息。

    ## TestNG的Maven插件

    #### 目录

*   Maven2 插件
*   原型

    ##### Maven 2

    Maven2 本身就支持 TestNG 而无需下载任何额外的插件（除了TestNG自己）。建议您使用2.4或以上版本的Surefire插件（这是在近期所有的Maven版本的情况下）。。你可以参考这里 [Surefire网站](https://link.jianshu.com?t=http://maven.apache.org/surefire/maven-surefire-plugin/) ,这里是 [TestNG 特别指南](https://link.jianshu.com?t=http://maven.apache.org/surefire/maven-surefire-plugin/examples/testng.html)。

    指定你的`pom.xml`

    在你的项目中依赖应该如下所示：

    <pre class="hljs xml">`<span class="hljs-tag">&lt;<span class="hljs-name">dependency</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">groupId</span>&gt;</span>org.testng<span class="hljs-tag">&lt;/<span class="hljs-name">groupId</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">artifactId</span>&gt;</span>testng<span class="hljs-tag">&lt;/<span class="hljs-name">artifactId</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">version</span>&gt;</span>6.1.1<span class="hljs-tag">&lt;/<span class="hljs-name">version</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-name">scope</span>&gt;</span>test<span class="hljs-tag">&lt;/<span class="hljs-name">scope</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">dependency</span>&gt;</span>
    `</pre>

    报告样例

    使用TestNG 的`surefire`报告可以看[这里](https://link.jianshu.com?t=http://testng.org/doc/samplereport/index.html) 。

    ##### Maven TestNG 原型 (Martin Gilday)

    Martin Gilday 已经为 Maven2 用户添加了新的原型，使其更容易使用TestNG。你可以在他的blog here 中找到更多内容。 但是基本的配置我已经在下面列出来了。

    要创建一个使用原型的项目，你只要简单的制定我的代码库和原型id。

    <pre class="hljs cpp">`mvn archetype:create -DgroupId=org.martingilday -DartifactId=test1 -DarchetypeGroupId=org.martingilday -DarchetypeArtifactId=testng-archetype
      -DarchetypeVersion=<span class="hljs-number">1.0</span>-SNAPSHOT -DremoteRepositories=http:<span class="hljs-comment">//www.martingilday.org/repository/</span>

当然了，你可以替换为自己的`groudId`和`artifactId`。
