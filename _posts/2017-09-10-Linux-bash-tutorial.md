---
layout: post
title:  "Bash教程"
date:   2017-09-10 01
categories: Linux
tags:  Linux Shell
author: Dex
---

* content
{:toc}

Bash == Bourne Again shell

Shell有很多种，比如ksh，csh,bsh等。我们使用的是bash，也是使用最多的。

Shell是一种解释性的语言，适用于基本的逻辑处理和不追求速度的应用。







## 查看本机⽀持的shell ##

	pi@raspberrypi:~ $ more /etc/shells
	# /etc/shells: valid login shells
	/bin/sh
	/bin/dash
	/bin/bash
	/bin/rbash


## 变量 ##

变量不需要定义，可以直接使用。

### 定义变量 ###

var=“xxx” `=` 左右不要有空格。需要的时候，必须用`”`或者`’`括起来。

也可以使用命令的输出作为变量的内容 var=`ls`。`$()` 与`反引号`类似。但是优先级要高。而且支持嵌套。

使用`$var` 或 `${var}`来访问变量。后者更为严谨。`$varx` `${var}x` 是不同的。

Shell变量不需要定义页可以使用。引用未定义的变量，默认为空值。

### 数组变量 ###

使用`（）`来定义数组变量，中间使用空格隔开。

举例:

	pi@raspberrypi:~/Templates $ array=(1 3 4 6);
	pi@raspberrypi:~/Templates $ echo ${array[2]}
	4


也可以通过命令来赋值。举例，列出当前目录所有文件或者子目录：

	array=(`ls`); 
	for i in ${array[*]};
	do echo $i;
	done

输出如下：

	pi@raspberrypi:~ $ array=(`ls`); 
	pi@raspberrypi:~ $ for i in ${array[*]};
	> do echo $i;
	> done
	Desktop
	Documents
	Downloads
	Music
	Pictures
	Public
	Templates
	Videos
	gj
	python
	python_games
	zdx


数组变量是稀疏的。

引用方式。${array[*]}返回所有的数组变量组成的字符串。

${array[2]}表示序号为2的变量。数组下标以0开始

### 特殊符号的应用 ###

`“”` 双引号用于括起⼀段字符串值，如果里面有特殊字符，就进行替换。比如$var形式的变量，\n 转义等。

`’` 单引号也表示其内容是字符串值。保持原义。

`反引号。代表命令的输出。

`\` 反斜线，某些情况下表示转义。

`$(ls)` 表示执行`ls`后的结果。与``类似。不过可以嵌套。

`$(())` 对变量进行操作。比如相加`$((a+b)) $((2+3))`

`(())` 是整数扩展。把里面的变量当作整数去处理。

`{1..10}` 等价于 seq 1 10。表示1到10

	pi@raspberrypi:~ $ echo {1..10}
	1 2 3 4 5 6 7 8 9 10

`()` 子shell中运行

`{}` 当前shell中执行

### 数字型变量操作 ###

通过`expr 2 + 3`。注意中间使用空格隔开，否则会输出这个字符串。

	pi@raspberrypi:~ $ expr 2 + 3
	5

其他程序，如awk 。i=2;j=3; echo "$i $j" |awk 'END{print 2+3;print $1 +$2;}'

	pi@raspberrypi:~ $ i=2;j=3; echo "$i $j" |awk 'END{print 2+3;print $1 +$2;}'
	5
	5

`i=1;((i=i+1));echo $i;` 推荐方式

	pi@raspberrypi:~ $ i=1;((i=i+1));echo $i
	2

    let x=2+3;echo $x

	pi@raspberrypi:~ $ let x=2+3;echo $x
	5

declare -i

	pi@raspberrypi:~ $ declare -i a
	pi@raspberrypi:~ $ a=56
	pi@raspberrypi:~ $ echo $a
	56

    declare [+/-][rxi][变量名称＝设置值] 或 declare -f
	+/- 　"-"可用来指定变量的属性，"+"则是取消变量所设的属性。
	-f 　仅显示函数。
	r 　将变量设置为只读。
	x 　指定的变量会成为环境变量，可供shell以外的程序来使用。
	i 　[设置值]可以是数值，字符串或运算式。

### 字符串变量的操作 ###

expr:

- 用空格隔开每个项；
- 用 / (反斜杠) 放在 shell 特定的字符前面；
- 对包含空格和其他特殊字符的字符串要用引号括起来

计算字串长度:

	> expr length “this is a test”
	 14

	> expr substr “this is a test” 3 5
	is is

	pi@raspberrypi:~ $ expr substr "abcd" 1 3
	abc

	 > expr 14 % 9
	 5
	 > expr 10 + 10
	 20
	 > expr 1000 + 900
	 1900
	 > expr 30 / 3 / 2
	 5
	 > expr 30 /* 3 (使用乘号时，必须用反斜线屏蔽其特定含义。因为shell可能会误解显示星号的意义)
	 90
	 > expr 30 * 3
	 expr: Syntax error

使用程序处理 sed ,awk ,cut，tr等

