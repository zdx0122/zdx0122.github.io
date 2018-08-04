---
layout: post
title:  "JavaScript"
date:   2016-09-24 01
categories: JavaScript
tags:  JavaScript
author: i.itest.ren
---

* content
{:toc}





## 请做好准备 ##

### 为什么学习JavaScript ###

一、你知道，为什么JavaScript非常值得我们学习吗？

1. 所有主流浏览器都支持JavaScript。

2. 目前，全世界大部分网页都使用JavaScript。

3. 它可以让网页呈现各种动态效果。

4. 做为一个Web开发师，如果你想提供漂亮的网页、令用户满意的上网体验，JavaScript是必不可少的工具。

二、易学性

1.学习环境无外不在，只要有文本编辑器，就能编写JavaScript程序。

2.我们可以用简单命令，完成一些基本操作。

三、从哪开始学习呢？

学习JavaScript的起点就是处理网页，所以我们先学习基础语法和如何使用DOM进行简单操作。


代码：

	<!DOCTYPE HTML>
	<html> 
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>热身</title>
	</head>
	<body>
	  <p id="p1">我是第一段文字</p>
	  <p id="p2">我是第二段文字</p>
	  
	  <script type="text/javascript">
		document.write("hello");
		document.getElementById("p1").style.color="blue"; 
	  </script>
	</body>
	</html>

展示：

	我是第一段文字
	
	我是第二段文字
	
	hello

### 新朋友你在哪里？（如何插入JS） ###

我们来看看如何写入JS代码？你只需一步操作,使用`<script>`标签在HTML网页中插入JavaScript代码。注意， `<script>`标签要成对出现，并把JavaScript代码写在`<script></script>`之间。

`<script type="text/javascript">`表示在`<script></script>`之间的是文本类型(text),javascript是为了告诉浏览器里面的文本是属于JavaScript语言。

	<!DOCTYPE HTML>
	<html>
	    <head>
	        <meta http-equiv="Content-Type" content="text/html; charset=gb18030">
	        <title>插入js代码</title>
	    <script type = "text/javascript">
	        document.write("开启JS之旅!");
	    </script>    
	
	    
	    </head>
	    <body>
	    </body>
	</html>

### 我也可以独立（引用JS外部文件） ###

JavaScript代码只能写在HTML文件中吗?当然不是，我们可以把HTML文件和JS代码分开,并单独创建一个JavaScript文件(简称JS文件),其文件后缀通常为.js，然后将JS代码直接写在JS文件中。

注意: **在JS文件中，不需要`<script>`标签,直接编写JavaScript代码就可以了。**

JS文件不能直接运行，需嵌入到HTML文件中执行，我们需在HTML中添加如下代码，就可将JS文件嵌入HTML文件中。

	<script src="script.js"></script>

html代码：

	<!DOCTYPE HTML>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>引用JS文件</title>
	<script src="script.js"></script>
	</head>
	<body>
	</body>
	</html>

js代码：

	document.write("引用JS文件！");

输出：

	引用JS文件！

### 找到你的位置（JS在页面中的位置） ###

我们可以将JavaScript代码放在html文件中任何位置，但是我们一般放在网页的head或者body部分。

- 放在`<head>`部分

最常用的方式是在页面中head部分放置`<script>`元素，浏览器解析head部分就会执行这个代码，然后才解析页面的其余部分。

- 放在`<body>`部分

JavaScript代码在网页读取到该语句的时候就会执行。

注意: javascript作为一种脚本语言可以放在html页面中任何位置，但是浏览器解释html时是按先后顺序的，所以前面的script就先被执行。比如进行页面显示初始化的js必须放在head里面，因为初始化都要求提前进行（如给页面body设置css等）；而如果是通过事件调用执行的function那么对位置没什么要求的。

	<!DOCTYPE HTML>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>JS代码的位置</title>
	<script type="text/javascript">
	  document.write("I love");
	</script>
	</head>
	<body>
	<script type="text/javascript">
	  document.write("javascript")
	</script>
	</body>
	</html>

输出：

	I love javascript

### 认识语句和符号 ###

JavaScript语句是发给浏览器的命令。这些命令的作用是告诉浏览器要做的事情。

每一句JavaScript代码格式: `语句;`

先来看看下面代码

	<script type="text/javascript">
	   alert("hello!");
	</script>

例子中的`alert("hello!");`就是一个JavaScript语句。

一行的结束就被认定为语句的结束，通常在结尾加上一个分号`";"`来表示语句的结束。

看看下面这段代码,有三条语句，每句结束后都有`";"`，按顺序执行语句。

	<script type="text/javascript">
	   document.write("I");
	   document.write("love");
	   document.write("JavaScript");
	</script>

注意:

1. `“;”`分号要在英文状态下输入，同样，JS中的代码和符号都要在英文状态下输入。

2. 虽然分号`“;”`也可以不写，但我们要养成编程的好习惯，记得在语句末尾写上分号。

代码：

	<!DOCTYPE HTML>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>认识语句</title>
	<script type="text/javascript">
	    document.write("Hello");
	    document.write("World");
	</script>
	</head>
	<body>
	</body>
	</html>

输出：

	HelloWorld

### 注释很重要 ###

注释的作用是提高代码的可读性，帮助自己和别人阅读和理解你所编写的JavaScript代码，注释的内容不会在网页中显示。注释可分为单行注释与多行注释两种。

我们为了方便阅读，注释内容一般放到需要解释语句的结尾处或周围。

- 单行注释，在注释内容前加符号 `“//”`。

	<script type="text/javascript">
	  document.write("单行注释使用'//'");  // 我是注释，该语句功能在网页中输出内容
	</script>

- 多行注释以`"/*"`开始，以`"*/"`结束。

	<script type="text/javascript">
	   document.write("多行注释使用/*注释内容*/");
	   /*
	    多行注释
	    养成书写注释的良好习惯
	   */
	</script>

### 什么是变量 ###

什么是变量? 从字面上看，变量是可变的量；从编程角度讲，变量是用于存储某种/某些数值的存储器。我们可以把变量看做一个盒子，为了区分盒子，可以用BOX1,BOX2等名称代表不同盒子，BOX1就是盒子的名字（也就是变量的名字）。

定义变量使用关键字`var`,语法如下：

	var 变量名

变量名可以任意取名，但要遵循命名规则:

    1.变量必须使用字母、下划线(_)或者美元符($)开始。

    2.然后可以使用任意多个英文字母、数字、下划线(_)或者美元符($)组成。

    3.不能使用JavaScript关键词与JavaScript保留字。

变量要先声明再赋值，如下：

	var mychar;
	mychar="javascript";
	var mynum = 6;

变量可以重复赋值，如下：

	var mychar;
	mychar="javascript";
	mychar="hello";

注意:

1. 在JS中区分大小写，如变量mychar与myChar是不一样的，表示是两个变量。

2. 变量虽然也可以不声明，直接使用，但不规范，需要先声明，后使用。

### 判断语句（if...else） ###

`if...else`语句是在指定的条件成立时执行代码，在条件不成立时执行else后的代码。

语法:

	if(条件)
	{ 条件成立时执行的代码 }
	else
	{ 条件不成立时执行的代码 }

假设我们通过年龄来判断是否为成年人，如年龄大于等于18岁，是成年人，否则不是成年人。代码表示如下:

	<script type="text/javascript">
	   var myage = 18;
	   if(myage>=18)  //myage>=18是判断条件
	   { document.write("你是成年人。");}
	   else  //否则年龄小于18
	   { document.write("未满18岁，你不是成年人。");}
	</script>

### 什么是函数 ###

函数是完成某个特定功能的一组语句。如没有函数，完成任务可能需要五行、十行、甚至更多的代码。这时我们就可以把完成特定功能的代码块放到一个函数里，直接调用这个函数，就省重复输入大量代码的麻烦。

如何定义一个函数呢？基本语法如下:

	function 函数名()
	{
	     函数代码;
	}

说明:

1. function定义函数的关键字。

2. "函数名"你为函数取的名字。

3. "函数代码"替换为完成特定功能的代码。

我们来编写一个实现两数相加的简单函数,并给函数起个有意义的名字：“add2”，代码如下：

	function add2(){
	   var sum = 3 + 2;
	   alert(sum);
	}

函数调用:

函数定义好后，是不能自动执行的，所以需调用它,只需直接在需要的位置写函数就ok了.

代码：

	<!DOCTYPE HTML>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>函数调用</title>
	   <script type="text/javascript">
	      function contxt() //定义函数
	      {
	         alert("哈哈，调用函数了!");
	      }
	   </script>
	</head>
	<body>
	   <form>
	      <input type="button"  value="点击我" onclick="contxt()" />  
	   </form>
	</body>
	</html>

