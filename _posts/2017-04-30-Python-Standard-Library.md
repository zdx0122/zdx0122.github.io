---
layout: post
title:  "Python常用标准库"
date:   2017-04-30 01
categories: Python
tags:   Python
author: i.itest.ren
---

* content
{:toc}


记录Python常用标准库







## 操作系统接口 ##

`os` 模块提供了很多与操作系统交互的函数:

	>>> import os
	>>> os.getcwd()      # Return the current working directory
	'C:\\Python27'
	>>> os.chdir('/server/accesslogs')   # Change current working directory
	>>> os.system('mkdir today')   # Run the command mkdir in the system shell
	0

应该用 `import os` 风格而非 `from os import *` 。这样可以保证随操作系统不同而有所变化的 `os.open()` 不会覆盖内置函数 `open()`

在使用一些像 `os` 这样的大型模块时内置的 `dir()` 和 `help()` 函数非常有用:

    >>> import os
    >>> dir(os)
    <returns a list of all module functions>
    >>> help(os)
    <returns an extensive manual page created from the module's docstrings>
针对日常的文件和目录管理任务，shutil 模块提供了一个易于使用的高级接口:

    >>> import shutil
    >>> shutil.copyfile('data.db', 'archive.db')
    >>> shutil.move('/build/executables', 'installdir')

## 文件通配符 ##

`glob` 模块提供了一个函数用于从目录通配符搜索中生成文件列表:

    >>> import glob
    >>> glob.glob('*.py')
    ['primes.py', 'random.py', 'quote.py']

## 命令行参数 ##

用工具脚本经常调用命令行参数。这些命令行参数以链表形式存储于 `sys` 模块的 `argv` 变量。例如在命令行中执行` python demo.py one two three `后可以得到以下输出结果:

    >>> import sys
    >>> print sys.argv
    ['demo.py', 'one', 'two', 'three']
`getopt` 模块使用` Unix getopt()` 函数处理 `sys.argv` 。更多的复杂命令行处理由 `argparse` 模块提供。

## 错误输出重定向和程序终止 ##

`sys` 还有 `stdin` ， `stdout` 和 `stderr` 属性，即使在 `stdout` 被重定向时，后者也可以用于显示警告和错误信息:
    
    >>> sys.stderr.write('Warning, log file not found starting a new one\n')
    Warning, log file not found starting a new one
大多脚本的定向终止都使用 `sys.exit()` 。

## 字符串正则匹配 ##


`re` 模块为高级字符串处理提供了正则表达式工具。对于复杂的匹配和处理，正则表达式提供了简洁、优化的解决方案:

    >>> import re
    >>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
    ['foot', 'fell', 'fastest']
    >>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
    'cat in the hat'
只需简单的操作时，字符串方法最好用，因为它们易读，又容易调试:

    >>> 'tea for too'.replace('too', 'two')
    'tea for two'

## 数学 ##

`math` 模块为浮点运算提供了对底层 C 函数库的访问:

    >>> import math
    >>> math.cos(math.pi / 4.0)
    0.70710678118654757
    >>> math.log(1024, 2)
    10.0
`random` 提供了生成随机数的工具:

    >>> import random
    >>> random.choice(['apple', 'pear', 'banana'])
    'apple'
    >>> random.sample(xrange(100), 10)   # sampling without replacement
    [30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
    >>> random.random()# random float
    0.17970987693706186
    >>> random.randrange(6)# random integer chosen from range(6)
    4

## 互联网访问 ##

有几个模块用于访问互联网以及处理网络通信协议。其中最简单的两个是用于处理从 `urls` 接收的数据的 `urllib2` 以及用于发送电子邮件的 `smtplib`:

    >>> from urllib2
    >>> for line in urllib2.urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl'):
    ... line = line.decode('utf-8')  # Decoding the binary data to text.
    ... if 'EST' in line or 'EDT' in line:  # look for Eastern Time
    ... print line
    
    <BR>Nov. 25, 09:43:32 PM EST
    
    >>> import smtplib
    >>> server = smtplib.SMTP('localhost')
    >>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
    ... """To: jcaesar@example.org
    ... From: soothsayer@example.org
    ...
    ... Beware the Ides of March.
    ... """)
    >>> server.quit()
(注意第二个例子需要在 localhost 运行一个邮件服务器。)

`requests`库比`urllib2`要好用。[http://docs.python-requests.org/zh_CN/latest/](http://docs.python-requests.org/zh_CN/latest/)

## 日期和时间 ##

`datetime` 模块为日期和时间处理同时提供了简单和复杂的方法。支持日期和时间算法的同时，实现的重点放在更有效的处理和格式化输出。该模块还支持时区处理:

    >>> # dates are easily constructed and formatted
    >>> from datetime import date
    >>> now = date.today()
    >>> now
    datetime.date(2003, 12, 2)
    >>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
    '12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'
    
    >>> # dates support calendar arithmetic
    >>> birthday = date(1964, 7, 31)
    >>> age = now - birthday
    >>> age.days
    14368

## 多线程 ##

线程是一个分离无顺序依赖关系任务的技术。在某些任务运行于后台的时候应用程序会变得迟缓，线程可以提升其速度。一个有关的用途是在 I/O 的同时其它线程可以并行计算。

下面的代码显示了高级模块 `threading` 如何在主程序运行的同时运行任务:

	import threading, zipfile
	
	class AsyncZip(threading.Thread):
	    def __init__(self, infile, outfile):
	        threading.Thread.__init__(self)
	        self.infile = infile
	        self.outfile = outfile
	    def run(self):
	        f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
	        f.write(self.infile)
	        f.close()
	        print 'Finished background zip of:', self.infile
	
	background = AsyncZip('mydata.txt', 'myarchive.zip')
	background.start()
	print 'The main program continues to run in foreground.'
	
	background.join()    # Wait for the background task to finish
	print 'Main program waited until background was done.'
多线程应用程序的主要挑战是协调线程，诸如线程间共享数据或其它资源。 为了达到那个目的，线程模块提供了许多同步化的原生支持，包括：锁，事件，条件变量和信号灯。

尽管这些工具很强大，微小的设计错误也可能造成难以挽回的故障。 因此，任务协调的首选方法是把对一个资源的所有访问集中在一个单独的线程中，然后使用 `queue` 模块用那个线程服务其他线程的请求。 为内部线程通信和协调而使用 `Queue` 对象的应用程序更易于设计，更可读，并且更可靠。


## 日志 ##

`logging` 模块提供了完整和灵活的日志系统。它最简单的用法是记录信息并发送到一个文件或 `sys.stderr`:

	import logging
	logging.debug('Debugging information')
	logging.info('Informational message')
	logging.warning('Warning:config file %s not found', 'server.conf')
	logging.error('Error occurred')
	logging.critical('Critical error -- shutting down')
输出如下:

	WARNING:root:Warning:config file server.conf not found
	ERROR:root:Error occurred
	CRITICAL:root:Critical error -- shutting down
默认情况下捕获信息和调试消息并将输出发送到标准错误流。其它可选的路由信息方式通过 email，数据报文，`socket` 或者 `HTTP Server`。基于消息属性，新的过滤器可以选择不同的路由： `DEBUG`, `INFO`, `WARNING`, `ERROR`, 和 `CRITICAL` 。

日志系统可以直接在 Python 代码中定制，也可以不经过应用程序直接在一个用户可编辑的配置文件中加载。


## 列表工具 ##

很多数据结构可能会用到内置列表类型。然而，有时可能需要不同性能代价的实现。

`array` 模块提供了一个类似列表的 `array()` 对象，它仅仅是存储数据，更为紧凑。以下的示例演示了一个存储双字节无符号整数的数组（类型编码 "`H`" ）而非存储 `16` 字节 `Python` 整数对象的普通正规列表:

	>>> from array import array
	>>> a = array('H', [4000, 10, 700, 22222])
	>>> sum(a)
	26932
	>>> a[1:3]
	array('H', [10, 700])
`collections` 模块提供了类似列表的 `deque()` 对象，它从左边添加（`append`）和弹出（`pop`）更快，但是在内部查询更慢。这些对象更适用于队列实现和广度优先的树搜索:

	>>> from collections import deque
	>>> d = deque(["task1", "task2", "task3"])
	>>> d.append("task4")
	>>> print "Handling", d.popleft()
	Handling task1
	
	unsearched = deque([starting_node])
	def breadth_first_search(unsearched):
	    node = unsearched.popleft()
	    for m in gen_moves(node):
	        if is_goal(m):
	            return m
	        unsearched.append(m)
除了链表的替代实现，该库还提供了 `bisect` 这样的模块以操作存储链表:

	>>> import bisect
	>>> scores = [(100, 'perl'), (200, 'tcl'), (400, 'lua'), (500, 'python')]
	>>> bisect.insort(scores, (300, 'ruby'))
	>>> scores
	[(100, 'perl'), (200, 'tcl'), (300, 'ruby'), (400, 'lua'), (500, 'python')]
`heapq` 提供了基于正规链表的堆实现。最小的值总是保持在 0 点。这在希望循环访问最小元素但是不想执行完整堆排序的时候非常有用:

	>>> from heapq import heapify, heappop, heappush
	>>> data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
	>>> heapify(data)                      # rearrange the list into heap order
	>>> heappush(data, -5)                 # add a new entry
	>>> [heappop(data) for i in range(3)]  # fetch the three smallest entries
	[-5, 0, 1]

## 十进制浮点数算法 ##

`decimal` 模块提供了一个 `Decimal` 数据类型用于浮点数计算。相比内置的二进制浮点数实现 float ，这个类型有助于

- 金融应用和其它需要精确十进制表达的场合，
- 控制精度，
- 控制舍入以适应法律或者规定要求，
- 确保十进制数位精度，或者
- 用户希望计算结果与手算相符的场合。


例如，计算 70 分电话费的 5% 税计算，十进制浮点数和二进制浮点数计算结果的差别如下。如果在分值上舍入，这个差别就很重要了:

	>>> from decimal import *
	>>> round(Decimal('0.70') * Decimal('1.05'), 2)
	Decimal('0.74')
	>>> round(.70 * 1.05, 2)
	0.73
`Decimal` 的结果总是保有结尾的 0，自动从两位精度延伸到4位。 `Decimal` 重现了手工的数学运算，这就确保了二进制浮点数无法精确保有的数据精度。

高精度使 `Decimal` 可以执行二进制浮点数无法进行的模运算和等值测试:

	>>> Decimal('1.00') % Decimal('.10')
	Decimal('0.00')
	>>> 1.00 % 0.10
	0.09999999999999995
	
	>>> sum([Decimal('0.1')]*10) == Decimal('1.0')
	True
	>>> sum([0.1]*10) == 1.0
	False
`decimal` 提供了必须的高精度算法:

	>>> getcontext().prec = 36
	>>> Decimal(1) / Decimal(7)
	Decimal('0.142857142857142857142857142857142857')

