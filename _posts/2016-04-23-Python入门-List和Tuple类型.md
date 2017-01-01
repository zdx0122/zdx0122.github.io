---
layout: post
title:  "Python入门(3)-List和Tuple类型"
date:   2016-04-23 01
categories: Python
tags:  Python
author: Dex
---

* content
{:toc}







## 创建List ##

Python内置的一种数据类型是列表：list。list是一种有序的集合，可以随时添加和删除其中的元素。

比如，列出班里所有同学的名字，就可以用一个list表示：

	>>> ['Michael', 'Bob', 'Tracy']
	['Michael', 'Bob', 'Tracy']

list是数学意义上的有序集合，也就是说，list中的元素是按照顺序排列的。

构造list非常简单，按照上面的代码，直接用 `[ ]` 把list的所有元素都括起来，就是一个list对象。通常，我们会把list赋值给一个变量，这样，就可以通过变量来引用list：

	>>> classmates = ['Michael', 'Bob', 'Tracy']
	>>> classmates # 打印classmates变量的内容
	['Michael', 'Bob', 'Tracy']

由于Python是动态语言，所以list中包含的元素并不要求都必须是同一种数据类型，我们完全可以在list中包含各种数据：

	>>> L = ['Michael', 100, True]

一个元素也没有的list，就是空list：

	>>> empty_list = []

代码：

	L = ['Adam', 95.5, 'Lisa', 85, 'Bart', 59]
	print L

## 按照索引访问list ##

由于list是一个有序集合，所以，我们可以用一个list按分数从高到低表示出班里的3个同学：

	>>> L = ['Adam', 'Lisa', 'Bart']

那我们如何从list中获取指定第 N 名的同学呢？方法是通过索引来获取list中的指定元素。

**需要特别注意的是**，索引从 0 开始，也就是说，第一个元素的索引是0，第二个元素的索引是1，以此类推。

因此，要打印第一名同学的名字，用 L[0]:

	>>> print L[0]
	Adam

要打印第二名同学的名字，用 L[1]:

	>>> print L[1]
	Lisa

要打印第三名同学的名字，用 L[2]:

	>>> print L[2]
	Bart

要打印第四名同学的名字，用 L[3]:

	>>> print L[3]
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	IndexError: list index out of range

报错了！IndexError意思就是索引超出了范围，因为上面的list只有3个元素，有效的索引是 0，1，2。

所以，使用索引时，**千万注意不要越界**。

	L = [95.5,85,59]
	print L[0]
	print L[1]
	print L[2]
	print L[-1]

## 倒序访问list ##

我们还是用一个list按分数从高到低表示出班里的3个同学：

	>>> L = ['Adam', 'Lisa', 'Bart']
	>
这时，老师说，请分数最低的同学站出来。

要写代码完成这个任务，我们可以先数一数这个 list，发现它包含3个元素，因此，最后一个元素的索引是2：

	>>> print L[2]
	Bart

有没有更简单的方法？

有！

Bart同学是最后一名，俗称倒数第一，所以，我们可以用 -1 这个索引来表示最后一个元素：

	>>> print L[-1]
	Bart

Bart同学表示躺枪。

类似的，倒数第二用 -2 表示，倒数第三用 -3 表示，倒数第四用 -4 表示：

	>>> print L[-2]
	Lisa
	>>> print L[-3]
	Adam
	>>> print L[-4]
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	IndexError: list index out of range

L[-4] 报错了，因为倒数第四不存在，一共只有3个元素。

使用倒序索引时，也要注意**不要越界**。

## 添加新元素 ##

现在，班里有3名同学：

	>>> L = ['Adam', 'Lisa', 'Bart']

今天，班里转来一名新同学 Paul，如何把新同学添加到现有的 list 中呢？

第一个办法是用 list 的 `append()` 方法，把新同学追加到 list 的末尾：

	>>> L = ['Adam', 'Lisa', 'Bart']
	>>> L.append('Paul')
	>>> print L
	['Adam', 'Lisa', 'Bart', 'Paul']

**append()**总是把新的元素添加到 list 的尾部。

如果 Paul 同学表示自己总是考满分，要求添加到第一的位置，怎么办？

方法是用list的 `insert()`方法，它接受两个参数，第一个参数是索引号，第二个参数是待添加的新元素：

	>>> L = ['Adam', 'Lisa', 'Bart']
	>>> L.insert(0, 'Paul')
	>>> print L
	['Paul', 'Adam', 'Lisa', 'Bart']

L.insert(0, 'Paul') 的意思是，'Paul'将被添加到索引为 0 的位置上（也就是第一个），而原来索引为 0 的Adam同学，以及后面的所有同学，都自动向后移动一位。

	L = ['Adam', 'Lisa', 'Bart']
	L.insert(2, 'Paul')
	print L

## 从list删除元素 ##

Paul同学刚来几天又要转走了，那么我们怎么把Paul 从现有的list中删除呢？

如果Paul同学排在最后一个，我们可以用list的`pop()`方法删除：

	>>> L = ['Adam', 'Lisa', 'Bart', 'Paul']
	>>> L.pop()
	'Paul'
	>>> print L
	['Adam', 'Lisa', 'Bart']

**pop()**方法总是删掉list的最后一个元素，并且它还返回这个元素，所以我们执行 L.pop() 后，会打印出 'Paul'。

如果Paul同学不是排在最后一个怎么办？比如Paul同学排在第三：

	>>> L = ['Adam', 'Lisa', 'Paul', 'Bart']

要把Paul踢出list，我们就必须先定位Paul的位置。由于Paul的索引是2，因此，用 `pop(2)`把Paul删掉：

	>>> L.pop(2)
	'Paul'
	>>> print L
	['Adam', 'Lisa', 'Bart']

代码：

	L = ['Adam', 'Lisa', 'Paul', 'Bart']
	L.pop(2)
	L.pop(2)
	print L


## 替换元素 ##

假设现在班里仍然是3名同学：

	>>> L = ['Adam', 'Lisa', 'Bart']

现在，Bart同学要转学走了，碰巧来了一个Paul同学，要更新班级成员名单，我们可以先把Bart删掉，再把Paul添加进来。

另一个办法是直接用Paul把Bart给替换掉：

	>>> L[2] = 'Paul'
	>>> print L
	L = ['Adam', 'Lisa', 'Paul']

对list中的某一个索引赋值，就可以直接用新的元素替换掉原来的元素，list包含的元素个数保持不变。

由于Bart还可以用 -1 做索引，因此，下面的代码也可以完成同样的替换工作：

	>>> L[-1] = 'Paul'

代码：

	L = ['Adam', 'Lisa', 'Bart']
	L[0] = 'Bart'
	L[-1] = 'Adam'
	print L

## 创建tuple ##

tuple是另一种有序的列表，中文翻译为“ 元组 ”。tuple 和 list 非常类似，但是，**tuple一旦创建完毕，就不能修改了。**

同样是表示班里同学的名称，用tuple表示如下：

	>>> t = ('Adam', 'Lisa', 'Bart')

创建tuple和创建list唯一不同之处是用( )替代了[ ]。

现在，这个 t 就不能改变了，tuple没有 append()方法，也没有insert()和pop()方法。所以，新同学没法直接往 tuple 中添加，老同学想退出 tuple 也不行。

获取 tuple 元素的方式和 list 是一模一样的，我们可以正常使用 t[0]，t[-1]等索引方式访问元素，但是不能赋值成别的元素，不信可以试试：

	>>> t[0] = 'Paul'
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: 'tuple' object does not support item assignment

## 创建单元素tuple ##

tuple和list一样，可以包含 0 个、1个和任意多个元素。

包含多个元素的 tuple，前面我们已经创建过了。

包含 0 个元素的 tuple，也就是空tuple，直接用 ()表示：

	>>> t = ()
	>>> print t
	()

创建包含1个元素的 tuple 呢？来试试：

	>>> t = (1)
	>>> print t
	1

好像哪里不对！t 不是 tuple ，而是整数1。为什么呢？

因为()既可以表示tuple，又可以作为括号表示运算时的优先级，结果 (1) 被Python解释器计算出结果 1，导致我们得到的不是tuple，而是整数 1。

正是因为用()定义单元素的tuple有歧义，所以 Python 规定，单元素 tuple 要多加一个逗号`“,”`，这样就避免了歧义：

	>>> t = (1,)
	>>> print t
	(1,)

Python在打印单元素tuple时，也自动添加了一个`“,”`，为了更明确地告诉你这是一个tuple。

多元素 tuple 加不加这个额外的“,”效果是一样的：

>>> t = (1, 2, 3,)
>>> print t
(1, 2, 3)

## "可变"的tuple ##

前面我们看到了tuple一旦创建就不能修改。现在，我们来看一个“可变”的tuple：

	>>> t = ('a', 'b', ['A', 'B'])

注意到 t 有 3 个元素：'a'，'b'和一个list：['A', 'B']。list作为一个整体是tuple的第3个元素。list对象可以通过 t[2] 拿到：

	>>> L = t[2]

然后，我们把list的两个元素改一改：

	>>> L[0] = 'X'
	>>> L[1] = 'Y'
	>
再看看tuple的内容：

	>>> print t
	('a', 'b', ['X', 'Y'])

表面上看，tuple的元素确实变了，但其实变的不是 tuple 的元素，而是list的元素。

tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

理解了“指向不变”后，要创建一个内容也不变的tuple怎么做？那就必须保证tuple的每一个元素本身也不能变。

	t = ('a', 'b', ('A', 'B'))
	print t

