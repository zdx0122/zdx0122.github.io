---
layout: post
title:  "自动发送上线计划/过程"
date:   2017-03-03 01
categories: Python
tags:  Python
author: Dex
---

* content
{:toc}

工作中，用Python实现自动发送上线计划/过程脚本。





## idea 背景 目的 ##

idea来源：木子白

背景： 在有项目上线时，测试人员需要提前一天在Confluense上写入明天的上线计划，然后第二天把上线计划邮件发送到相关人。

需求&目的： 测试人员提前填写上线计划后，由程序检测是否有当天上线计划，如果有，则抓取当天的上线计划，生成上线计划邮件，并发送到相关人邮箱。

## 实现思路 ##

实现方案： Python + Jenkins/crontab

实现模块：

- 爬虫抓取上线计划网页html
	- 需要登录confluence，通过session解决
- 抓取具体的当天的上线计划
	- 抓取特定的当天的上线计划：BeautifulSoup实现
- 生成邮件内容
	- 把抓取出的上线计划内容，拼接为邮件内容。
- 发送邮件
	- 由Python实现发送邮件
- 定时任务
	- 部署到Jenkins中，或者借助crontab


## 具体模块的关键代码 ##

用到的python模块：

	requests, ConfigParser, os, datetime, re, smtplib, lxml, bs4, email.mime.text, email.mime.text, email.utils

### 爬虫抓取上线计划网页html ###

	# 填写confluence登录地址、上线计划URL、登录帐号密码信息
	confluenceLoginUrl = 'http://confluence.daojia-inc.com/dologin.action'
	confluenceOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr201609'
	paramLogin = {'os_username': 'zhangdexi', 'os_password': '123456xx'}
	 
	# 抓取html
	s = requests.session()
	r_login = s.post(confluenceLoginUrl, paramLogin)
	r_OnlinePlan = s.get(confluenceOnlinePlanUrl)
	pageHtml = r_OnlinePlan.content

### 抓取具体的当天的上线计划 ###

	# 抓取整个html中的table标签内容，然后抓取table中的tr标签内容
	r_OnlinePlan_soup = BeautifulSoup(pageHtml, 'lxml')
	r_OnlinePlan_soup_table = r_OnlinePlan_soup.table
	tr = r_OnlinePlan_soup_table.find_all('tr')
	 
	# 遍历抓取到的tr标签中的内容，查找到当天的数据，提取出来
	currentTime = datetime.datetime.now().strftime('%Y%m%d')
	planNum = 0
	for r in tr:
	    if r.find(text=currentTime) is not None:
	        print r
	        planNum = planNum + 1
	        msg_content = msg_content + str(r)
	if planNum > 0:
	    msg_content = msg_content + '</tbody></table></body></html>'
	    print '共' + str(planNum) + '条上线计划'
	else:
	    msg_content = None

### 生成邮件内容 ###

把提取出的内容作为邮件内容，包括首行的列名和当日的上线计划。
由于抓取的内容为html, 没有css，所以导致邮件内容是页面飞掉的，解决方法是： 在<head>部分手动添加css。

	table_css = '''
	    <html>
	    <head>
	    <style type="text/css">
	    table, td, tr
	      {
	      border:1px solid grey;
	      border-collapse: collapse;
	      }
	    </style>
	    </head>
	    <body>
	    hi, all<br>
	    以下是丽人业务线上线计划，请阅：<br>
	    <table class="relative-table wrapped confluenceTable" style="width: 100.0%;">
	    <colgroup>
	        <col style="width: 30px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 300px;"/>
	        <col style="width: 60px;"/>
	        <col style="width: 150px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 30px;"/>
	    </colgroup>
	    <tbody>
	    '''
	    msg_content = table_css + str(tr[0])
	    planNum = 0
	    for r in tr:
	        if r.find(text=currentTime) is not None:
	            print r
	            planNum = planNum + 1
	            msg_content = msg_content + str(r)
	    # 添加判断条件，如果抓取的内容没有当日的上线计划，则把邮件内容置为None。
	    if planNum > 0:
	    msg_content = msg_content + '</tbody></table></body></html>'
	else:
	    msg_content = None

### 定时任务 ###

两种方式：

- 添加到Linux的定时任务中
	- crontab
- 新建Jenkins的Job启动构建
	- 源码管理： svn 获取.py代码
	- 构建： python AutoSendOnlinePlan.py

## 不足和限制 ##

- 不足：
	- 是程序都有bug
	- 美观性不足

- 限制：
	1. Confluence上线计划页面的标题只能为“业务线名称 + 年月”，例如丽人业务线的上线计划页面标题为“lr201609”,Confluence生成的链接为：http://confluence.daojia-inc.com/display/proprietary/lr201609
	2. 默认只会抓取页面中的第一个表格为上线计划，抓取第二个表格为上线过程。
	3. 上线日期填写格式为20160927，暂不支持其他格式，如2016-09-27，2016927，2016年9月27日等。
	4. 遇到一个需求分多天上线的情况，不能出现合并单元格的情况，合并单元格会导致第二天的数据抓不到表格中的内容，导致邮件内容飞掉。只需要另写一行上线计划，即可解决。




## 自动发送上线计划 ##

代码：

	# coding=utf-8
	
	import requests, ConfigParser, os, datetime, re, smtplib
	from lxml import etree
	from bs4 import BeautifulSoup
	from email.mime.text import MIMEText
	from email.header import Header
	from email.utils import parseaddr, formataddr
	
	
	def get_conf():
	    conf_file = ConfigParser.ConfigParser()
	    conf_file.read(os.path.join(os.getcwd(), 'sendOnlinePlan.ini'))
	
	def getContent(confluenceLoginUrl, confluenceOnlinePlanUrl, paramLogin):
	    s = requests.session()
	    r_login = s.post(confluenceLoginUrl, paramLogin)
	    r_OnlinePlan = s.get(confluenceOnlinePlanUrl)
	    pageHtml = r_OnlinePlan.content
	    return pageHtml
	
	def genMSGtext(pageHtml):
	    r_OnlinePlan_soup = BeautifulSoup(pageHTML, 'lxml')
	    r_OnlinePlan_soup_table = r_OnlinePlan_soup.find_all('table')
	    table1_soup = BeautifulSoup(str(r_OnlinePlan_soup_table[0]), 'lxml')
	    tr = table1_soup.find_all('tr')
	    print '表格已抓到~'
	    table_css = '''
	    <html>
	    <head>
	    <style type="text/css">
	    table, td, tr
	      {
	      border:1px solid grey;
	      border-collapse: collapse;
	      font-size:12px;
	      }
	    </style>
	    </head>
	    <body>
	    hi, all<br>
	    以下是丽人业务线上线计划，请阅：<br>
	    <table class="relative-table wrapped confluenceTable" ;">
	    <colgroup>
	    	<col style="width: 30px;"/>
	    	<col style="width: 40px;"/>
	    	<col style="width: 40px;"/>
	    	<col style="width: 40px;"/>
	        <col style="width: 300px;"/>
	        <col style="width: 60px;"/>
	        <col style="width: 150px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 30px;"/>
	    </colgroup>
	    <tbody>
	    '''
	    msg_content = table_css + str(tr[0])
	    #currentTime = '20160919'
	    currentTime = datetime.datetime.now().strftime('%Y%m%d')
	    print '今天是：' + currentTime
	    planNum = 0
	    for r in tr:
	        if r.find(text=currentTime) is not None:
	            print r
	            planNum = planNum + 1
	            msg_content = msg_content + str(r)
	    if planNum > 0:
	        msg_content = msg_content + '</tbody></table></body></html>'
	        print '共' + str(planNum) + '条上线计划'
	    else:
	        msg_content = None
	    return msg_content
	
	def sendMail(msg_content):
	    def _format_addr(s):
	        name, addr = parseaddr(s)
	        return formataddr(( \
	            Header(name, 'utf-8').encode(), \
	            addr.encode('utf-8') if isinstance(addr, unicode) else addr))
	    from_addr = 'xxxxxx@daojia.com'
	    mail_account = 'xxxxx@daojia.com'
	    mail_password = 'xxxxxxxx'
	    to_addr = ['xxxxx@daojia.com']
	    #to_addr = ['xxxxxxx@daojia.com']
	    currentDate = datetime.datetime.now().strftime('%Y年%m月%d日')
	    mail_Subject = '【Djoy】【丽人_今日上线计划】汇总' + currentDate
	
	    msg = MIMEText(msg_content, 'html', 'utf-8')
	    msg['From'] = _format_addr(from_addr)#u'自营QA上线计划 <%s>' %
	    msg['To'] = _format_addr(to_addr)
	    msg['Subject'] = _format_addr(mail_Subject)
	
	    smtp_server = 'smtp.exmail.qq.com'
	    smtp_port = 465
	    #server = smtplib.SMTP_SSL(smtp_server, smtp_port) #此方式在服务器上不支持！！！换成以下两条语句就ok
	    server = smtplib.SMTP(smtp_server)
	    server.starttls()
	    server.set_debuglevel(1)
	    server.login(mail_account, mail_password)
	    server.sendmail(from_addr, to_addr, msg.as_string())
	    server.quit()
	
	
	if __name__ == '__main__':
	    currentDate_url = datetime.datetime.now().strftime('%Y%m')
	    lrOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr' + currentDate_url
	    confluenceLoginUrl = 'http://confluence.daojia-inc.com/dologin.action'
	    #confluenceOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr201609'
	    paramLogin = {'os_username': 'xxxxxxxx', 'os_password': 'xxxxxxx'}
	
	    #登录confluence, 并获取到上线计划的html
	    pageHTML = getContent(confluenceLoginUrl, lrOnlinePlanUrl, paramLogin)
	
	    #获取表格内容并生成邮件内容
	    msg_content = None
	    try:
	        msg_content = genMSGtext(pageHTML)
	    except :
	        print '缺少上线计划表格！'
	    
	
	    #发送邮件
	    if msg_content is not None:
	        sendMail(msg_content)
	    else:
	        print '今日无上线计划，好开心~'



## 自动发送上线过程脚本 ##

代码：

	# coding=utf-8
	
	import requests, ConfigParser, os, datetime, re, smtplib
	from lxml import etree
	from bs4 import BeautifulSoup
	from email.mime.text import MIMEText
	from email.header import Header
	from email.utils import parseaddr, formataddr
	
	
	def get_conf():
	    conf_file = ConfigParser.ConfigParser()
	    conf_file.read(os.path.join(os.getcwd(), 'sendOnlinePlan.ini'))
	
	def getContent(confluenceLoginUrl, confluenceOnlinePlanUrl, paramLogin):
	    s = requests.session()
	    r_login = s.post(confluenceLoginUrl, paramLogin)
	    r_OnlinePlan = s.get(confluenceOnlinePlanUrl)
	    pageHtml = r_OnlinePlan.content
	    return pageHtml
	
	def genMSGtext(pageHtml):
	    r_OnlineProcess_soup = BeautifulSoup(pageHTML, 'lxml')
	
	    r_OnlineProcess_soup_table = r_OnlineProcess_soup.find_all('table')
	    table2_soup = BeautifulSoup(str(r_OnlineProcess_soup_table[1]), 'lxml')
	    tr = table2_soup.find_all('tr')
	    print '表格已抓到~'
	    table_css = '''
	    <html>
	    <head>
	    <style type="text/css">
	    table, td, tr
	      {
	      border:1px solid grey;
	      border-collapse: collapse;
	      font-size:12px;
	      }
	    </style>
	    </head>
	    <body>
	    hi, all<br>
	    以下是丽人业务线上线过程，请阅：<br>
	    <table class="relative-table wrapped confluenceTable" ;">
	    <colgroup>
	    	<col style="width: 30px;"/>
	    	<col style="width: 40px;"/>
	    	<col style="width: 40px;"/>
	    	<col style="width: 40px;"/>
	        <col style="width: 300px;"/>
	        <col style="width: 60px;"/>
	        <col style="width: 150px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 70px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 75px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 40px;"/>
	        <col style="width: 30px;"/>
	    </colgroup>
	    <tbody>
	    '''
	    msg_content = table_css + str(tr[0])
	    #currentTime = '20160918'
	    today = datetime.date.today()
	    oneday = datetime.timedelta(days=1)
	    lastday = today - oneday
	    currentTime = lastday.strftime('%Y%m%d')
	    print '今天是：' + currentTime
	    planNum = 0
	    for r in tr:
	        if r.find(text=currentTime) is not None:
	            print r
	            planNum = planNum + 1
	            msg_content = msg_content + str(r)
	    if planNum > 0:
	        msg_content = msg_content + '</tbody></table></body></html>'
	        print '共' + str(planNum) + '条上线过程'
	    else:
	        msg_content = None
	    return msg_content
	
	def sendMail(msg_content):
	    def _format_addr(s):
	        name, addr = parseaddr(s)
	        return formataddr(( \
	            Header(name, 'utf-8').encode(), \
	            addr.encode('utf-8') if isinstance(addr, unicode) else addr))
	    from_addr = 'xxxxxxx'
	    mail_account = 'xxxxxxx'
	    mail_password = 'xxxxxx'
	    to_addr = ['xxxxx']
	    today = datetime.date.today()
	    oneday = datetime.timedelta(days=1)
	    lastday = today - oneday
	    currentDate = lastday.strftime('%Y年%m月%d日')
	    mail_Subject = '【Djoy】【丽人_昨日上线过程】汇总' + currentDate
	
	    msg = MIMEText(msg_content, 'html', 'utf-8')
	    msg['From'] = _format_addr(from_addr)#u'自营QA上线计划 <%s>' %
	    msg['To'] = _format_addr(to_addr)
	    msg['Subject'] = _format_addr(mail_Subject)
	
	    smtp_server = 'smtp.exmail.qq.com'
	    smtp_port = 465
	    #server = smtplib.SMTP_SSL(smtp_server, smtp_port)#此种方式在服务器上不支持，换成以下两条语句即可
	    server = smtplib.SMTP(smtp_server)
	    server.starttls()
	    server.set_debuglevel(1)
	    # server.starttls()
	    server.login(mail_account, mail_password)
	    server.sendmail(from_addr, to_addr, msg.as_string())
	    server.quit()
	
	
	if __name__ == '__main__':
	    if (datetime.datetime.now().strftime('%d') != '01'):
	        currentDate_url = datetime.datetime.now().strftime('%Y%m')
	        lrOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr' + currentDate_url
	    else:
	        currentDate_url = datetime.datetime.now().strftime('%Y%m')
	        currentDate_url = int(currentDate_url) - 1
	        lrOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr' + str(currentDate_url)
	    print lrOnlinePlanUrl
	    # currentDate_url = datetime.datetime.now().strftime('%Y%m')
	    # lrOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr' + currentDate_url
	    confluenceLoginUrl = 'http://confluence.daojia-inc.com/dologin.action'
	    #lrOnlinePlanUrl = 'http://confluence.daojia-inc.com/display/proprietary/lr201609'
	    paramLogin = {'os_username': 'xxxx', 'os_password': 'xxxxxxx'}
	
	    #登录confluence, 并获取到上线计划的html
	    pageHTML = getContent(confluenceLoginUrl, lrOnlinePlanUrl, paramLogin)
	
	    #获取表格内容并生成邮件内容
	    msg_content = None
	    try:
	        msg_content = genMSGtext(pageHTML)
	    except :
	        print '缺少上线过程表格！'
	
	    #发送邮件
	    if msg_content is not None:
	        sendMail(msg_content)
	    else:
	        print '昨天无上线过程，好开心~'