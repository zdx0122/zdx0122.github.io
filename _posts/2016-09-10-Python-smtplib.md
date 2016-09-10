---
layout: post
title:  "Python使用smtplib和email发送腾讯企业邮箱邮件"
date:   2016-09-10 01
categories: Python
---




## SMTP 发送邮件 ##

http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386832745198026a685614e7462fb57dbf733cc9f3ad000

http://www.cnblogs.com/dyfblog/p/4294172.html

## Show me the code ##

	import smtplib
	from email.mime.text import MIMEText
	from email.header import Header
	from email.utils import parseaddr, formataddr
	
	# 发送邮件
	
	def _format_addr(s):
	    name, addr = parseaddr(s)
	    return formataddr(( \
	        Header(name, 'utf-8').encode(), \
	        addr.encode('utf-8') if isinstance(addr, unicode) else addr))
	
	from_addr = 'xxx@xxx.com'
	mail_account = 'xxx'
	mail_password = 'xxx'
	to_addr = ['xxx@qq.com', 'xxx@qq.com']
	mail_Subject = '速运业务线' + currentTime + '上线计划内容'
	msg_content = 'The msg Content'
	
	msg = MIMEText(msg_content, 'html', 'utf-8')
	msg['From'] = _format_addr(u'自营QA上线计划 <%s>' % from_addr)
	msg['To'] = _format_addr(to_addr)
	msg['Subject'] = _format_addr(mail_Subject)
	
	smtp_server = 'smtp.exmail.qq.com'
	smtp_port = 465
	server = smtplib.SMTP_SSL(smtp_server, smtp_port)
	server.set_debuglevel(1)
	#server.starttls()
	server.login(mail_account, mail_password)
	server.sendmail(from_addr, to_addr, msg.as_string())
	server.quit()

