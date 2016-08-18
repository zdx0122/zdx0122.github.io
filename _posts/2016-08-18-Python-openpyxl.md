---
layout: post
title:  "Python操作excel文件"
date:   2016-08-18 01
categories: Python
---





## 0x00 ##

openpyxl支持excel2007及以后的版本；

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

