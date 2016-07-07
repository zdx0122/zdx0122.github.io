---
layout: post
title:  "Java解析XML"
date:   2016-07-07 01
categories: Java
---






## 初次邂逅xml ##

表现： 以`.xml`为文件名后缀的文件。

存储： 以树的形式存储。

节点: `<nodename></nodename>`, 节点名称区分大小写!

books.xml内容示例：

	<?xml version="1.0" encoding="UTF-8" ?>
	<bookstore>
		<book id = "1">
			<name>冰与火之歌</name>
			<author>乔治马丁</author>
			<year>2014</year>
			<price>89</price>
		</book>
		<book id = "2">
			<name>安徒生童话</name>
			<year>2004</year>
			<price>77</price>
			<language>English</language>
		</book>
	</bookstore>

### 为什么要使用XML？ ###

- 可以用于不同应用程序之间的通信
- 可以用于不同平台间的通信
- 可以用于不同平台间数据的共享



## 在Java程序中如何获取xml文件的内容 ##

在Java程序中读取xml文件的过程也称为解析xml文件。

解析的目的： 获取节点名、节点值、属性名、属性值。

四种解析方式： DOM, SAX, DOM4J, JDOM。

常用的节点类型：

![常用的节点类型](http://7fvd6e.com1.z0.glb.clouddn.com/java-xml-nodetype.jpg)

### Java解析books.xml文件示例 ###

#### DOM方式解析xml步骤 ####

准备工作：

1. 创建一个DocumentBuilderFactory对象
2. 创建一个一个DocumentBuilder的对象
3. 通过DocumentBuilder对象的parse(String fileName)方法解析xml文件

代码如下：

	package ren.itest.xmlstu.test;
	
	import java.io.IOException;
	
	import javax.xml.parsers.DocumentBuilder;
	import javax.xml.parsers.DocumentBuilderFactory;
	import javax.xml.parsers.ParserConfigurationException;
	
	import org.w3c.dom.Document;
	import org.w3c.dom.Element;
	import org.w3c.dom.NamedNodeMap;
	import org.w3c.dom.Node;
	import org.w3c.dom.NodeList;
	import org.xml.sax.SAXException;
	
	public class domtest {
	
		public static void main(String[] args) {
			//1. 创建一个DocumentBuilderFactory对象
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			
			try {
				//2. 创建一个一个DocumentBuilder的对象
				DocumentBuilder db = dbf.newDocumentBuilder();
				
				//3. 通过DocumentBuilder对象的parse(String fileName)方法加载books.xml文件到当前项目下
				Document document =  db.parse("books.xml");
				
				//获取所有的book节点的集合
				NodeList bookList = document.getElementsByTagName("book");
				
				//通过NodeList的getLength()方法获取bookList的长度
				System.out.println("一共有" + bookList.getLength() + "本书");
				
				//遍历每一个book节点
				for (int i = 0; i < bookList.getLength(); i++) {
					System.out.println("==========下面开始遍历第" + (i + 1) + "本书的内容==========");
					//通过item(i)方法获取一个book节点，NodeList的索引值从0开始
					Node book = bookList.item(i);
					//获取book节点的所有属性集合
					NamedNodeMap attrs = book.getAttributes();
					System.out.println("第" + (i + 1) + "本书共有" + attrs.getLength() + "个属性");
					//遍历book的属性
					for(int j = 0; j < attrs.getLength(); j++) {
						//通过item(j)方法获取book节点中的某一个属性值
						Node attr = attrs.item(j);
						//获取属性名
						System.out.print("属性名：" + attr.getNodeName());
						//获取属性值
						System.out.println("--属性值：" + attr.getNodeValue());
					}
					
					//另外一种方法获取属性值
	//				Element book = (Element) bookList.item(i);
	//				String attrValue = book.getAttribute("id");
	//				System.out.println("id属性的属性值为" + attrValue);
					
					//解析book节点的子节点
					NodeList childNodes = book.getChildNodes();
					//遍历childNodes获取每个节点的节点名和节点值
					System.out.println("第" + (i + 1) + "本书共有" + childNodes.getLength() + "个子节点");
					for (int k = 0; k < childNodes.getLength(); k++) {
						//区分出text类型的node以及element类型的node
						if (childNodes.item(k).getNodeType() == Node.ELEMENT_NODE) {
							//获取了element类型节点的节点名
							System.out.print("第" + (k + 1) + "个节点的节点名：" + childNodes.item(k).getNodeName());
							//获取了element类型节点的节点值
							System.out.println("--节点值是：" + childNodes.item(k).getFirstChild().getNodeValue());
						}
						
					}
					
					
					System.out.println("==========结束遍历第" + (i + 1) + "本书的内容==========");
				}
	
			} catch (ParserConfigurationException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (SAXException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
				}
			
		}
	
	}




## 4种解析方式PK(整体) ##

基础方法 ： DOM, SAX

- DOM是平台无关的官方解析方式。
- SAX是基于事件驱动的解析方式。

扩展方法 ： JDOM, DOM4J

在基础的方法上扩展出的， 只有Java中能够使用的解析方式。

### DOM解析图解 ###

![DOM解析图解](http://7fvd6e.com1.z0.glb.clouddn.com/java-xml-dom.jpg)

对内存要求高一些。

### SAX解析图解 ###

![SAX解析图解](http://7fvd6e.com1.z0.glb.clouddn.com/java-xml-sax.jpg)

### 选择DOM还是SAX ###

#### DOM ####

- 优点：
	- 形成了树结构，直观好理解，代码更易编写
	- 解析过程中树结构保留在内存中，方便修改
- 缺点：
	- 当xml文件较大时，对内存耗费比较大，容易影响解析性能并造成内存溢出

#### SAX ####

- 优点：
	- 采用事件驱动模式，对内存耗费比较小
	- 适合于只需要处理xml中数据时
- 缺点：
	- 不易编码
	- 很难同时访问同一个xml中的多处不同数据

### JDOM 与 DOM、DOM4J ###

#### JDOM ####

- 仅使用具体类而不使用接口
- API大量使用了Collections类

#### DOM4J ####

- JDOM的一种智能分支，它合并了许多超出基本xml文档表示的功能
- DOM4J使用接口和抽象基本类方法，是一个优秀的Java XML API
- 具有性能优异、灵活性好、功能强大和极端易于使用的特点
- 是一个开发源代码的软件

