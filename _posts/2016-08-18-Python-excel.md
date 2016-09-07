---
layout: post
title:  "Python操作excel文件"
date:   2016-08-18 01
categories: Python
---





## 0x00 ##

`xlrd`支持excel2003的文档，格式为`.xls`.

官网: http://pypi.python.org/pypi/xlrd

`openpyxl`支持excel2007及以后的版本, 格式为`.xlsx`；

官网：https://openpyxl.readthedocs.io

`xls` 和 `xlsx` 是两种完全不同的格式，其本质的差别相比字面的区别要多很多。`xls` 的核心结构是复合文档类型的结构，而 `xlsx` 的核心结构是 `XML` 类型的结构，采用的是基于XML的压缩方式，使其占用的空间更小。`xlsx` 中最后一个 `x` 的意义就在于此。

## 0x01 xlrd模块 ##

安装

- 官网下载安装
	- http://pypi.python.org/pypi/xlrd
	- 然后执行 `python setup.py install`
- `easy_install xlrd`
- `pip install xlrd`

导入模块： `import xlrd`

打开excel文件读取数据： `data = xlrd.open_workbook('excelFile.xls')`

获取一个工作表：

    table = data.sheets()[0]          #通过索引顺序获取

    table = data.sheet_by_index(0)    #通过索引顺序获取 

    table = data.sheet_by_name(u'Sheet1')  #通过名称获取

获取整行和整列的值(数组)：

     table.row_values(i)

     table.col_values(i)

获取行数和列数：
　　
    nrows = table.nrows

    ncols = table.ncols

循环行列表数据：

	for i in range(nrows):
		print table.row_values(i)
 
单元格：

	cell_A1 = table.cell(0,0).value
	 
	cell_C4 = table.cell(2,3).value
 
使用行列索引：

	cell_A1 = table.row(0)[0].value
	 
	cell_A2 = table.col(1)[0].value
 
简单的写入：

	row = 0
	 
	col = 0
 
	//类型 0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error

	ctype = 1 value = '单元格的值'
	 
	xf = 0 # 扩展的格式化
	 
	table.put_cell(row, col, ctype, value, xf)
	 
	table.cell(0,0)  #单元格的值'
	 
	table.cell(0,0).value #单元格的值'

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