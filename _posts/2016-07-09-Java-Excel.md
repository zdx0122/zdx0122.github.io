---
layout: post
title:  "Java解析Excel"
date:   2016-07-09 01
categories: Java
tags:  Java
author: Dex
---

* content
{:toc}






## 读写Excel的几种方式 ##

### POI ###

Apache POI是Apache软件基金会的开放源码函式库，POI提供API给Java程序对Microsoft Office格式档案读和写的功能。

HSSF是Horrible SpreadSheet Format的缩写，也即“讨厌的电子表格格式”。通过HSSF，你可以用纯Java代码来读取、写入、修改Excel文件。

HSSF - 读写Microsoft Office格式档案的功能。

XSSF - 读写Microsoft Office OOXML格式档案的功能。

HWPF - 读写Microsoft Word格式档案的功能。

HSLF - 读写Microsoft PowerPoint格式档案的功能。

HDGF - 读写Microsoft Visio格式档案的功能。

**iText**

通过iText不仅可以生成PDF或rtf的文档，而且可以将XML、HTML文件转化为PDF文件。

下载iText.jar文件后，只需要在系统的CLASSPATH中加入iText.jar的路径，在程序中就可以使用iText类库了。

### JXL ###

Java Excel是一开放源码项目，可以读取Excel文件的内容、创建新的Excel文件、更新已经存在的Excel文件。

### POI和JXL对比 ###

- POI
	- 效率搞
	- 操作相对复杂
	- 支持公式，宏，图像图表一些企业应用上会非常使用
	- 能够修饰单元格属性
	- 支持字体、数字、日期操作

- JXL
	- 效率低
	- 操作简单
	- 部分支持
	- 能够修饰单元格属性，格式不如POI强大
	- 支持字体、数字、日期操作

### FastExcel ###

FastExcel是一个采用纯Java开发的Excel文件读写组件，支持Excel 97-2003文件格式。

FastExcel只能读取单元格的字符信息，而其他属性如颜色、字体等就不支持了，因此FastExcel只需要很小的内存。

