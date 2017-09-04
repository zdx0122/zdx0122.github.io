---
layout: post
title:  "Python的设计哲学"
date:   2017-09-04 01
categories: Python
tags:  Python
author: Dex
---

* content
{:toc}

Python的设计哲学：优雅、简洁、高效








## Python之禅 ##

python的设计哲学总结为六个字，就是：优雅、明确、简单。为了能够让广大python爱好者全面了解python的设计思想，python社区的人每天都在源源不断的贡献自己的智慧和精力。其中有一位叫Peter的开发者用及其精辟的话整理总结了python的特性，并将之加入到python的模块中，成为了一个小彩蛋。想见这个彩蛋，只需在Python交互式命令下`import this`即可。

	C:\Users\user>python
	Python 2.7.13 (v2.7.13:a06454b1afa1, Dec 17 2016, 20:42:59) [MSC v.1500 32 bit (
	Intel)] on win32
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import this
	The Zen of Python, by Tim Peters
	
	Beautiful is better than ugly.
	Explicit is better than implicit.
	Simple is better than complex.
	Complex is better than complicated.
	Flat is better than nested.
	Sparse is better than dense.
	Readability counts.
	Special cases aren't special enough to break the rules.
	Although practicality beats purity.
	Errors should never pass silently.
	Unless explicitly silenced.
	In the face of ambiguity, refuse the temptation to guess.
	There should be one-- and preferably only one --obvious way to do it.
	Although that way may not be obvious at first unless you're Dutch.
	Now is better than never.
	Although never is often better than *right* now.
	If the implementation is hard to explain, it's a bad idea.
	If the implementation is easy to explain, it may be a good idea.
	Namespaces are one honking great idea -- let's do more of those!
	>>>

翻译：

	>>> import this
	The Zen of Python, by Tim Peters
	
	Beautiful is better than ugly. 
	优美胜于丑陋
	Explicit is better than implicit.
	明确胜于晦涩
	Simple is better than complex.
	简单胜于复杂
	Complex is better than complicated.
	复杂胜于凌乱
	Flat is better than nested.
	扁平胜于嵌套
	Sparse is better than dense.
	稀疏胜于稠密
	Readability counts.
	可读性需要考虑
	Special cases aren't special enough to break the rules.
	即使情况特殊，也不应打破规则
	Although practicality beats purity.
	尽管使用胜于纯净
	Errors should never pass silently.
	错误不应该悄无声息的忽略
	Unless explicitly silenced.
	除非特意这么做
	In the face of ambiguity, refuse the temptation to guess.
	面对混淆是，拒绝猜测（深入搞明白问题）
	There should be one-- and preferably only one --obvious way to do it.
	总有一个，且仅有一个，明显的方法来处理问题
	Although that way may not be obvious at first unless you're Dutch.
	Now is better than never.
	现在开始胜过永远不开始
	Although never is often better than *right* now.
	尽管永远不开始经常比仓促立即开始好
	If the implementation is hard to explain, it's a bad idea.
	如果程序的实现很难解释，那么它不是一个很好的实现
	If the implementation is easy to explain, it may be a good idea.
	反之
	Namespaces are one honking great idea -- let's do more of those!
	命名空间是个绝好的注意，让我们多利用它

这就是python的设计之禅，python社区的这群人都是非常幽默的，将python的设计思想放在解释器中，让人怎么也想不到，这还真是一番人生哲学啊。短短的几行字，借用一个网友说的，每个点都千锤百炼；每一点都直指人内心的感觉；既有指导大是大非的一年，又有指导细节操作的原则；既有谆谆教诲的推荐，也有声色俱厉的禁止。看N遍，每一遍都会让人深思，这就是哲学。