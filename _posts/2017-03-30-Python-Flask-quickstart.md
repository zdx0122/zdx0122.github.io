---
layout: post
title:  "Flask-快速入门"
date:   2017-03-29 01
categories: Python
tags:   Python Flask
author: Dex
---

* content
{:toc}


Flask is a microframework for Python based on Werkzeug, Jinja 2 and good intentions. And before you ask: It's BSD licensed!







## 运行Flask ##

安装：

	pip install Flask

提示如下就是成功：

		Collecting MarkupSafe>=0.23 (from Jinja2>=2.4->Flask)
	  Downloading MarkupSafe-1.0.tar.gz
	Installing collected packages: itsdangerous, click, Werkzeug, MarkupSafe, Jinja2
	, Flask
	  Running setup.py install for itsdangerous ... done
	  Running setup.py install for MarkupSafe ... done
	Successfully installed Flask-0.12 Jinja2-2.9.5 MarkupSafe-1.0 Werkzeug-0.12.1 cl
	ick-6.7 itsdangerous-0.24

编写Hello World!

	from flask import Flask
	app = Flask(__name__)
	
	@app.route("/")
	def hello():
	    return "Hello World!"
	
	if __name__ == "__main__":
	    app.run()

运行Hello-World:
	
		python hello.py
	 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

浏览器打开http://127.0.0.1:5000/，即可访问！


那么，这段代码做了什么？

- 首先，我们导入了 `Flask` 类。这个类的实例将会是我 们的 `WSGI` 应用程序。
- 接下来，我们创建一个该类的实例，第一个参数是应用模块或者包的名称。 如果你使用单一的模块（如本例），你应该使用 `__name__` ，因为模块 的名称将会因其作为单独应用启动还是作为模块导入而有不同（ 也即是 '`__main__`' 或实际的导入名）。这是必须的，这样 `Flask` 才知道到哪去找模板、静态文件等等。详情见 Flask 的文档。
- 然后，我们使用 `route()` 装饰器告诉 `Flask` 什么样 的`URL` 能触发我们的函数。
- 这个函数的名字也在生成 `URL` 时被特定的函数采用，这个函数返回我们想 要显示在用户浏览器中的信息。
- 最后我们用 `run()` 函数来让应用运行在本地服务器上。 其中 `if __name__ == '__main__':` 确保服务器只会在该脚本被 `Python` 解释器直接执行的时候才会运行，而不是作为模块导入的时候。
- 欲关闭服务器，按 `Ctrl+C`。

### 外部可访问的服务器 ###

如果你运行了这个服务器，你会发现它只能从你自己的计算机上访问，网络 中其它任何的地方都不能访问。在调试模式下，用户可以在你的计算机上执 行任意 Python 代码。因此，这个行为是默认的。

如果你禁用了 `debug` 或信任你所在网络的用户，你可以简单修改调用 `run()` 的方法使你的服务器公开可用，如下:

    app.run(host='0.0.0.0')
这会让操作系统监听所有公网 IP。

## 调试模式 ##

虽然 `run()` 方法适用于启动本地的开发服务器，但是 你每次修改代码后都要手动重启它。这样并不够优雅，而且 `Flask` 可以做到更 好。如果你启用了调试支持，服务器会在代码修改后自动重新载入，并在发生 错误时提供一个相当有用的调试器。

有两种途径来启用调试模式。一种是直接在应用对象上设置:

    app.debug = True
    app.run()
另一种是作为 `run` 方法的一个参数传入:

    app.run(debug=True)
两种方法的效果完全相同。

**注意：**

尽管交互式调试器在允许 fork 的环境中无法正常使用（也即在生产服务器 上正常使用几乎是不可能的），但它依然允许执行任意代码。这使它成为一 个巨大的安全隐患，因此它 **绝对不能用于生产环境** 。

## 路由 ##
现代 Web 应用的 URL 十分优雅，易于人们辨识记忆，这一点对于那些面向使 用低速网络连接移动设备访问的应用特别有用。如果可以不访问索引页，而是 直接访问想要的那个页面，他们多半会笑逐颜开而再度光顾。

如上所见， route() 装饰器把一个函数绑定到对应的 URL 上。

这里是一些基本的例子:

@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello World'
但是，不仅如此！你可以构造含有动态部分的 URL，也可以在一个函数上附着 多个规则。