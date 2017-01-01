---
layout: post
title:  "MySQL-子查询和连接"
date:   2016-07-19 04
categories: 数据库
---





## 子查询简介 ##

子查询(Subquery)是指出现在其他SQL语句内的SELECT字句。

例如：

	SELECT * FROMt1 WHERE col1 = (SELECT col2 FROM t2);
	
	其中SELECT * FROM t1, 称为Outer Query/Outer Statement
	SELECT col2 FROM t2, 称为SubQuery

子查询指嵌套在查询内部，且必须始终出现在圆括号内。

子查询可以包含多个关键字或条件，如DISTINCT、GROUP BY、ORDER BY、LIMIT、函数等。

子查询的外层查询可以是： SELECT, INSERT, UPDATE, DELETE, SET 或 DO。

子查询可以返回标量、一行、一列或子查询。

## 由比较运算符引起的子查询 ##

使用比较运算符的子查询：

	=, >, <, >=, <=, <>, !=, <=>

语法结构：

	operand comparison_operator subquery

用ANY、SOME、或ALL修饰的比较运算符

	operand comparison_operator ANY (subquery)
	
	operand comparison_operator SOME (subquery)
	
	operand comparison_operator ALL (subquery)

## 由[NOT] IN/EXISTS引发的子查询 ##

语法结构

	operand comparison_operator [NOT] IN (subquery)
	=ANY 运算符与IN等效。
	！=ALL或<>ALL运算符与NOT IN等效。


如果子查询返沪任何行，EXISTS将返回TRUE;否则为FALSE。

## 使用INSERT...SELECT插入记录 ##

## 多表更新 ##

	UPDATE table_references SET col_name1={exp1 | DEFAULT} [, col_name2={expr | DEFAULT}] ...[WHERE where_condition]

表的参照关系

	table_reference
	{[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN}
	table_reference
	ON conditional_expr

## 多表更新之一步到位 ##

CREATE...SELECT

创建数据表同时将查询结果写入到数据表

	CREATE TABLE [IF NOT EXISTS] tbl_name [(create_definition,...)] select_statement


## 连接的语法结构 ##

MySQL在SELECT语句、多表更新、多表删除语句中支持JOIN操作。

	table_reference
	
	{[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN}
	
	table_reference
	
	ON conditional_expr

数据表参照

	table_reference

	tbl_name [[AS] alias] | table_subquery [AS] alias

数据表可以使用tbl_name AS alias_name 或tbl_name alias_name赋予别名。

table_subquery可以作为子查询使用在FROM子句中，这样的子查询必须为其赋予别名。

## 内连接 INNER JOIN ##

INNER JOIN,内连接

在MySQL中，JOIN, CROSS JOIN和INNER JOIN是等价的。

LEFT [OUTER] JOIN,左外连接

RIGHT [OUTER] JOIN,右外连接

连接条件：

使用ON关键字来设定连接条件，也可以使用WHERE来代替。

通常使用ON关键字来设定连接条件，使用WHERE关键字进行结果集记录的过滤。

内连接：显示左表及右表符合连接条件的记录

## 外连接 OUTER JOIN ##

左外连接：显示左表的全部记录及右表符合连接条件的记录

右外连接：显示右表的全部记录及左表符合连接条件的记录

## 关于连接的几点说明 ##

	A LEFT JOIN B join_condition

数据表B的结果集依赖数据表A。

数据表A的结果集根据左连接条件依赖所有数据表（B表除外）。

左外连接条件决定如何检索数据表B（在没有指定WHERE条件的情况下。）

如果数据表A的某条记录符合WHERE条件，但是在数据表B不存在符合连接条件的记录，将生成一个所有列为空的额外的B行。

如果使用内连接查找的记录在连接数据表中不存在，并且在WHERE子句中尝试以下操作：col_namd IS NULL时，如果col_name被定义为NOT NULL,MySQL将在找到符合连执着条件的记录后停止搜索更多的行。

## 无限级分类表设计 ##

自身连接

同一个数据表对自身进行连接。

## 多表删除 ##

	DELETE tbl_name[.*] [, tbl_name[.*]] ... FROM table_references [WHERE where_condition]

