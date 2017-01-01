---
layout: post
title:  "Python操作excel文件"
date:   2016-08-18 01
categories: Python
tags:  Python
author: Dex
---

* content
{:toc}






## 0x00 ##

openpyxl支持excel2007及以后的版本；

官网：https://openpyxl.readthedocs.io

## 0x01 Sample code ##

	#coding=utf-8

	from openpyxl import Workbook
	import datetime
	
	wb = Workbook()
	ws = wb.active
	ws['A1'] = 42
	ws.append([1, 2, 3])
	ws['A2'] = datetime.datetime.now()
	wb.save('sample.xlsx')

sample.xlsx结果：

	42		
	2016-08-18 20:47:13	2	3

## 0x02 创建workbook ##

创建一个Workbook：

	from openpyxl import Workbook
	wb = Workbook()

要想使用它，就用`openpyxl.workbook.Workbook.active()`

	ws = wb.active

使用`openpyxl.workbook.Workbook.create_sheet()`创建一个新的sheet页：

	ws1 = wb.create_sheet("Mysheet") # insert at the end (default)
	# or
	ws2 = wb.create_sheet("Mysheet", 0) # insert at first position

修改sheet页的名称：

	ws.title = "New Title"

修改sheet页的背景色：

	ws.sheet_properties.tabColor = "1072BA"

Once you gave a worksheet a name, you can get it as a key of the workbook:

	ws3 = wb["New Title"]

`openpyxl.workbook.Workbook.sheetnames()`检查所有worksheet的名称：

	print(wb.sheetnames)
	返回列表：
	['Sheet2', 'New Title', 'Sheet1']
	
	循环输出sheet名称：
	for sheet in wb:
		print(sheet.title)

创建一个副本：

	source = wb.active
	target = wb.copy_worksheet(source)
	# 只能复制表格和其风格，不能在工作簿中复制sheet页。


## 0x03 存取单个表格数据 ##

将`A4`的值直接赋值给变量`c`：

	c = ws['A4']

将`4`直接赋给`A4`：

	ws['A4'] = 4

用行列标记给赋值`openpyxl.worksheet.Worksheet.cell()`：

	d = ws.cell(row=4, column=2, value=10)