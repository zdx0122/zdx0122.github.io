---
layout: post
title:  "HttpClient教程"
date:   2018-04-19 01
categories: Java
tags:  Java
author: i.itest.ren
---

* content
{:toc}

大多数的HTTP接口，都是用HttpClient来调用发起请求的。





## HttpClient是用来干什么的？ ##

HttpClient是一个客户端的HTTP通信实现库。HttpClient的目标是发送和接收HTTP报文。HttpClient不是一个浏览器。

在测试工作中，使用Java编写接口测试接口框架时，会用到 HttpClient，类似与Python接口测试框架中的requests.

官网下载地址：http://hc.apache.org/downloads.cgi

官网文档：http://hc.apache.org/httpcomponents-client-4.5.x/index.html

官网教程：http://hc.apache.org/httpcomponents-client-4.5.x/tutorial/html/index.html

`pom.xml`依赖：

        <dependency>
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.5</version>
        </dependency>

## 功能特色 ##

写几个用的多的特色~

- 基于标准的纯java，HTTP版本1和1.1的实现
- 支持所有的HTTP方法(GET, POST, PUT, DELETE, HEAD, OPTION, and TRACE)
- 支持HTTPS加密协议
- 通过HTTP代理透明连接
- 用于多线程应用程序中的连接管理支持。支持设置最大的总连接以及主机的最大连接数。检测和关闭陈旧的连接。
- 自动处理Set-Cookie中的Cookie
- 在http1.0和http1.1中利用KeepAlive保持持久连接
- 直接获取服务器发送的response code和 headers
- 设置连接超时的能力
- 源代码基于Apache License 可免费获取


## HTTP请求和响应 ##

HttpClient的最本质的功能是执行HTTP方法。

每个HTTP方法都有一个相对应的类：HttpGet,HttpHead,HttpPost,HttpPut,HttpDelete, HttpOptions, HttpTrace.

## 开始编码请求 ##

### HttpGet ###

首先对www.baidu.com发起GET请求：

        CloseableHttpClient httpClient = HttpClients.createDefault();
        HttpGet httpget = new HttpGet("http://www.baidu.com/");

        CloseableHttpResponse response;

        {
            try {
                response = httpClient.execute(httpget);
                System.out.println(response);

                System.out.println(EntityUtils.toString(response.getEntity()));
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

注意代码中的response，直接输出response是不会有详细的html信息的，必须用HttpClient中的EntityUtils工具类转为String才行。

运行，输出如下：

	HttpResponseProxy{HTTP/1.1 200 OK [Server: bfe/1.0.8.18, Date: Thu, 19 Apr 2018 07:45:43 GMT, Content-Type: text/html, Last-Modified: Mon, 23 Jan 2017 13:27:36 GMT, Transfer-Encoding: chunked, Connection: Keep-Alive, Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform, Pragma: no-cache, Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/] org.apache.http.client.entity.DecompressingEntity@6d00a15d}
	<!DOCTYPE html>
	<!--STATUS OK--><html> <head><meta http-equiv=content-type content=text/html;charset=utf-8><meta http-equiv=X-UA-Compatible content=IE=Edge><meta content=always name=referrer><link rel=stylesheet type=text/css href=http://s1.bdstatic.com/r/www/cache/bdorz/baidu.min.css><title>ç¾åº¦ä¸ä¸ï¼ä½ å°±ç¥é</title></head> <body link=#0000cc> <div id=wrapper> <div id=head> <div class=head_wrapper> <div class=s_form> <div class=s_form_wrapper> <div id=lg> <img hidefocus=true src=//www.baidu.com/img/bd_logo1.png width=270 height=129> </div> <form id=form name=f action=//www.baidu.com/s class=fm> <input type=hidden name=bdorz_come value=1> <input type=hidden name=ie value=utf-8> <input type=hidden name=f value=8> <input type=hidden name=rsv_bp value=1> <input type=hidden name=rsv_idx value=1> <input type=hidden name=tn value=baidu><span class="bg s_ipt_wr"><input id=kw name=wd class=s_ipt value maxlength=255 autocomplete=off autofocus></span><span class="bg s_btn_wr"><input type=submit id=su value=ç¾åº¦ä¸ä¸ class="bg s_btn"></span> </form> </div> </div> <div id=u1> <a href=http://news.baidu.com name=tj_trnews class=mnav>æ°é»</a> <a href=http://www.hao123.com name=tj_trhao123 class=mnav>hao123</a> <a href=http://map.baidu.com name=tj_trmap class=mnav>å°å¾</a> <a href=http://v.baidu.com name=tj_trvideo class=mnav>è§é¢</a> <a href=http://tieba.baidu.com name=tj_trtieba class=mnav>è´´å§</a> <noscript> <a href=http://www.baidu.com/bdorz/login.gif?login&amp;tpl=mn&amp;u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 name=tj_login class=lb>ç»å½</a> </noscript> <script>document.write('<a href="http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u='+ encodeURIComponent(window.location.href+ (window.location.search === "" ? "?" : "&")+ "bdorz_come=1")+ '" name="tj_login" class="lb">ç»å½</a>');</script> <a href=//www.baidu.com/more/ name=tj_briicon class=bri style="display: block;">æ´å¤äº§å</a> </div> </div> </div> <div id=ftCon> <div id=ftConw> <p id=lh> <a href=http://home.baidu.com>å³äºç¾åº¦</a> <a href=http://ir.baidu.com>About Baidu</a> </p> <p id=cp>&copy;2017&nbsp;Baidu&nbsp;<a href=http://www.baidu.com/duty/>ä½¿ç¨ç¾åº¦åå¿è¯»</a>&nbsp; <a href=http://jianyi.baidu.com/ class=cp-feedback>æè§åé¦</a>&nbsp;äº¬ICPè¯030173å·&nbsp; <img src=//www.baidu.com/img/gs.gif> </p> </div> </div> </div> </body> </html>

### HttpPost ###

除了GET，用到最多的就是POST了吧，来发起一波Passport登录~

        CloseableHttpClient httpclient = HttpClients.createDefault();
        HttpPost httppost = new HttpPost("http://passportwebtest.xxxxxx.cn/mobile/login");
        List<NameValuePair> formparams = new ArrayList<NameValuePair>();
        formparams.add(new BasicNameValuePair("mobile", "185xxxxxxxx"));
        formparams.add(new BasicNameValuePair("code","8888"));
        formparams.add(new BasicNameValuePair("cityid","1"));
        UrlEncodedFormEntity uefEntity;

        try {
            uefEntity = new UrlEncodedFormEntity(formparams, "UTF-8");
            httppost.setEntity(uefEntity);
            System.out.println("executing request " + httppost.getURI());
            CloseableHttpResponse response = httpclient.execute(httppost);
            try {
                HttpEntity entity = response.getEntity();
                if (entity != null) {
                    System.out.println("--------------------------------------");
                    System.out.println("Response content: " + EntityUtils.toString(entity, "UTF-8"));
                    System.out.println("--------------------------------------");
                }
            } finally {
                response.close();
            }
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (UnsupportedEncodingException e1) {
            e1.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();

        }
    }
    
代码返回如下：

	executing request http://passportwebtest.xxxxxx.cn/mobile/login
	--------------------------------------
	Response content: {"code":0,"isNewUser":0,"desc":"登录成功"}
	--------------------------------------

未完待补充。