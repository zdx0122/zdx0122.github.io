---
layout: post
title:  "Ant+Jmeter+Jenkins实现接口测试自动化"
date:   2016-06-14 01
categories: 接口测试
---



## 概述or流程 ##

用Jmeter编写接口测试用例，把用例保存到svn中，利用Jenkins上实现持续集成，Jenkins中用Ant进行构建操作，Jenkins中加入HTML报告、性能报告、邮件发送等功能。

以后每次可以用Jmeter写接口测试用例，上传svn，触发Jenkins，实现接口测试自动化。

## 环境搭建和本地Ant测试 ##

安装Ant，配置Ant环境变量；

安装Jmeter，配置Jmeter环境变量；

编写或录制要进行接口测试的脚本，保存为.jmx文件。.jmx文件也可以用命令行启动：`jmeter -n -t test.jmx -l test.jtl`

在本地进行ant测试，本地文件夹目录结构：

	-workspace
		-html
		-jtl
		-build.xml
		-jmeter_test_case.jmx

ant执行主要是靠build.xml文件，xml中设置Jmeter安装目录、生成报告的路径，定义生成报告的前缀；ant执行的是`target`中的内容，分别是`test`和`report`，`target`的名字是`all`，在配置Jenkins时会用到。

在Jmeter安装目录extras文件夹中有build.xml模版，此处是本项目的build.xml文件，内容如下(供参考)：

	<?xml version="1.0" encoding="utf-8"?>
	<project name="Ant-Jmeter-Test" default="all" basedir=".">
	    <tstamp>
	        <format property="time" pattern="yyyyMMddhhmm" />
	    </tstamp>
	    <!-- 需要改成自己本地的 Jmeter 目录-->
	    <property name="jmeter.home" value="C:\Program Files\apache-jmeter-2.13" />
	    <!-- jmeter生成jtl格式的结果报告的路径-->
	    <property name="jmeter.result.jtl.dir" value="C:\Program Files (x86)\Jenkins\jobs\Ant-Jmeter-Test\workspace\jtl" />
	    <!-- jmeter生成html格式的结果报告的路径-->
	    <property name="jmeter.result.html.dir" value="C:\Program Files (x86)\Jenkins\jobs\Ant-Jmeter-Test\workspace\html" />
	    <!-- 生成的报告的前缀 -->
	    <property name="ReportName" value="TestReport" />
	    <property name="jmeter.result.jtlName" value="${jmeter.result.jtl.dir}/${ReportName}${time}.jtl" />
	    <property name="jmeter.result.htmlName" value="${jmeter.result.html.dir}/${ReportName}${time}.html" />
	    <!-- 接收测试报告的邮箱 -->
	    <property name="mail_to" value="zhangdexi@maoyt.com" />
	    <!-- 电脑地址 -->
	    <property name="ComputerName" value="192.168.1.236" />
	    <target name="all">
	        <antcall target="test" />
	        <antcall target="report" />
	    </target>
	    <target name="test">
	        <taskdef name="jmeter" classname="org.programmerplanet.ant.taskdefs.jmeter.JMeterTask" />
	        <jmeter jmeterhome="${jmeter.home}" resultlog="${jmeter.result.jtlName}">
	            <!-- 声明要运行的脚本。"*.jmx"指包含此目录下的所有jmeter脚本 -->
	            <testplans dir="E:\work\jmeter\result" includes="*.jmx" />
	        </jmeter>
	    </target>
	    <target name="report">
	        <xslt in="${jmeter.result.jtlName}" out="${jmeter.result.htmlName}" style="${jmeter.home}/extras/jmeter-results-detail-report_21.xsl" />
	        <!-- 因为上面生成报告的时候，不会将相关的图片也一起拷贝至目标目录，所以，需要手动拷贝 -->
	        <copy todir="${jmeter.result.html.dir}">
	            <fileset dir="${jmeter.home}/extras">
	                <include name="collapse.png" />
	                <include name="expand.png" />
	            </fileset>
	        </copy>
	    </target>
	    <!-- 发送邮件 -->
	</project>

本地执行Ant，返回内容BUILDE SUCCESSFUL.

![本地执行Ant返回结果](http://7fvd6e.com1.z0.glb.clouddn.com/%E6%9C%AC%E5%9C%B0Ant.jpg)

## 配置Jenkins ##

搭建Jenkins，太多教程。。。自搜，Windows有双击傻瓜式安装就完事的安装包。

新建一个自由风格的Job,比如命名为Ant-Jmeter-Test。

**源码管理**，我是把.jmx文件的用例、build.xml、生成jtl格式报告目录、生成html格式报告放在了svn中，由Jenkins进行拉取。源码管理，第一次放svn链接会要求输入svn帐号和密码。
![](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins%E6%BA%90%E7%A0%81%E7%AE%A1%E7%90%86.jpg)

**构建触发器**，此处我暂时用手动点击“立即构建”。
![](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins%E6%9E%84%E5%BB%BA%E8%A7%A6%E5%8F%91%E5%99%A8.jpg)

**构建**，使用Ant构建，Build File直接填写当前项目目录中的build.xml，Target处可以不填写，如果填写，一定要写build.xml中的target name，本文中为"all"。
![](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins%E6%9E%84%E5%BB%BA.jpg)

**构建后操作**：
这里添加Publish Performance test result report和Publish HTML reports。插件可以在系统管理-管理插件中安装。插件名字分别为Performance plugin和HTML Publisher plugin。

![Performance plugin](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins-Publish-Performance-test-result-report.jpg)

![HTML Publisher plugin](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins-Publish-HTML-reports.jpg)

## 查看结果 ##

![](http://7fvd6e.com1.z0.glb.clouddn.com/Jenkins-console-output.jpg)



## max&min time为NaN的问题解决 ##

问题：

Ant+Jmeter生成的html报告，Min Time 和 Max Time 出现 NaN，如图：
![](http://7fvd6e.com1.z0.glb.clouddn.com/Min&MaxTime-NaN.jpg)

解决方法：

仅需要从Jmeter的lib包里把xalan-2.7.2.jar和serializer-2.7.2.jar copy到Ant的lib包里即可，不用修改build.xml。

修改后：

![](http://7fvd6e.com1.z0.glb.clouddn.com/Min&MaxTime-NaN.jpg)

## 报告生成报错 ##

在生成报告的时候，出现BUILD FAILED，报错信息如下：

![](http://7fvd6e.com1.z0.glb.clouddn.com/%E6%8A%A5%E5%91%8A%E5%87%BA%E9%94%99xml.jpg)

解决方法：

在Jmeter安装目录，Jmeter/bin 下将 jmeter.properties 中的：
jmeter.save.saveservice.output_format=csv
改成：
jmeter.save.saveservice.output_format=xml


