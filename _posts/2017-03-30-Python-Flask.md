---
layout: post
title:  "Flask"
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


