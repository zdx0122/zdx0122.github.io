---
layout: post
title:  "Chrome DevTools --Network"
date:   2018-06-08 01
categories: 前端
tags:  前端
author: Dex
---

* content
{:toc}

Chrome 开发者工具是一套内置在Google Chrome中Web开发和调试工具。使用开发者工具来重演，调试和剖析您的网站。





本篇仅是记录Chrome开发者工具中的Network部分，内容从同事笔记转载~

### 0x01 停止记录网络请求 ###

- 点击`Stop recording network log`红色图标，当它变为灰色时，表示`DevTools`不在记录请求
- 在`Network`面板下快捷键为：`Command+E(Mac)`或者`Ctrl+E(Windows,Linux)`

![](http://zdx0122.qiniudn.com/image2018-5-27%2016_45_34.png)


### 0x02 清楚网络请求 ###

![](http://zdx0122.qiniudn.com/image2018-5-27%2016_47_40.png)

### 0x03 跨页面加载时，保留网络请求记录 ###

- 当页面重载或者页面跳转时，默认情况下，`Network`面板下的网络请求记录表也是刷新的。如果想保留之前页面的网络请求数据，可以勾选`Preserve log`。 
-  常用的一个应用场景：登录/注册时会调用登录/注册API，开发者想查看这个接口返回的情况，但是登录/注册成功后一般会跳转到新的页面，导致了`Network`面板的请求记录被刷新从而看不到登录/注册接口返回的情况。此时勾选上`Preserve log`，无论跳转到那个页面，都能在`Network`网络请求记录表中查看到之前接口返回的情况。

![](http://zdx0122.qiniudn.com/image2018-5-27%2016_59_56.png)

### 0x04 页面加载时捕获屏幕截图 ###

- 捕获屏幕截图可以分析在页面加载的过程中，用户在不同的时间段内看到的网页是什么样子的。
- 点击`Capture screenshots`图标开启捕获功能，当图标变为蓝色表示已开启，重载页面即可看到不同时间的屏幕截图。

![](http://zdx0122.qiniudn.com/image2018-5-27%2017_1_18.png)

- 鼠标悬浮在一张图片上时，该图片四周会出现一个黄色的边框，同时，在`Overview`和`Waterfall`窗口里面也分别有一条黄色的竖线，这条黄色的竖线表示这张图片的捕获时间
- 点击某一张图片，可以过滤掉在这张图片捕获之后发生的所有请求
- 双击图片，可以放大该图片

![](http://zdx0122.qiniudn.com/image2018-5-27%2017_6_7.png)

### 0x05 禁用浏览器缓存 ###

在http请求的过程中，有些资源在页面初次加载之后会被缓存到浏览器中，也就是那些状态码为`304`的资源。为了尽可能准确地模拟用户第一次加载我们网页时的情景，需要禁用浏览器缓存，这样，每一个请求都是从服务端传送过来的，较为准确地反应出网页初次加载的实际情况。

![](http://zdx0122.qiniudn.com/image2018-5-27%2017_16_15.png)

### 0x06 模拟网速条件 ###

在`Network Throttling`下拉框中可以选择不同的网络条件进行模拟，如2G、3G、4G、WiFi等。

![](http://zdx0122.qiniudn.com/image2018-5-27%2017_19_42.png)

除了选中已有的网络选项，也可以自定义网速相关条件：打开`Network Throttling`菜单，选择`Custom > Add`。

另一种模拟情况较为特殊，就是无网络。利用`service workers`，`PWA(Progressive Web Apps)`在无网络的情况下依然可以使用。模拟这种无网络环境，直接勾选`Offline`即可。

![](http://zdx0122.qiniudn.com/image2018-5-27%2018_11_58.png)

提示：有时候开发者会看到`Network`左侧有个警告图标，这个图标就是提示开发者当前处于模拟网络条件下。

![](http://zdx0122.qiniudn.com/image2018-5-27%2018_12_33.png)

### 0x07 手动清除浏览器缓存、cookies ###

在网络请求记录表里面右键，选择`Clear Browser Cache`或`Clear Browser Cookies`。 

![](http://zdx0122.qiniudn.com/image2018-5-27%2019_36_40.png)

### 0x08 过滤请求 ###

#### 根据类型过滤 #####

![](http://zdx0122.qiniudn.com/image2018-5-27%2022_8_47.png)

这里是可以多选的：按住`Command(Mac)`键或`Ctrl(Windows,Linux)`键，然后单击不同的类型，如点击`JS`和`Img`，则过滤出js文件和图片。显然，`All`不与其他类型共存，选择`All`的时候不能再选某一个具体类型。

#### 根据属性过滤 ####

在输入框中可以输入一些字符串、域、大小、状态码、媒体类型等等，如果业务比较简单，可能输入一些字符串就搜索到自己想要的结果了。

![](http://zdx0122.qiniudn.com/image2018-5-27%2022_0_21.png)

- `domain`：筛选出指定域名的请求，不仅支持自动补全，还支持*匹配。
- `has-response-header`：筛选出包含指定响应头的请求。
- `is`：通过is:running找出WebSocket请求。
- `larger-than`：筛选出请求大于指定字节大小的请求，其中1000表示1k。
- `method`：筛选出指定HTTP方法的请求，比如GET请求、POST请求等。
- `mime-type`：筛选出指定文件类型的请求。
- `mixed-content`：筛选出混合内容的请求（不懂啥意思）。
- `scheme`：筛选出指定协议的请求，比如scheme:http、scheme:https。
- `set-cookie-domain`：筛选出指定cookie域名属性的包含Set-Cookie的请求。
- `set-cookie-name`：筛选出指定cookie名称属性的包含Set-Cookie的请求。
- `set-cookie-value`：筛选出指定cookie值属性的包含Set-Cookie的请求。
- `status-code`：筛选出指定HTTP状态码的请求

可输入的详细类型可参考[官方文档](https://developers.google.com/web/tools/chrome-devtools/network-performance/reference#filter-by-property)。

![](http://zdx0122.qiniudn.com/image2018-5-27%2022_6_35.png)

#### 隐藏data URLs ####

`data URLs`指一些嵌入到文档中的小型文件，在请求表里面以`data:`开头的文件就是，如较为常见的svg文件。勾选`Hide data URLs`复选框即可隐藏此类文件。

![](http://zdx0122.qiniudn.com/image2018-5-27%2022_11_42.png)

#### 根据时间过滤 ####

点击下图中绿色方框的图标，显示/隐藏`Overview`窗口。在`Overview`窗口分别拖动左边或右边黄色圆圈中的滑动条，就可过滤出位于两个滑动条之间这段时间发出的请求，不是在这段时间发出的请求就被隐藏掉了。

![](http://zdx0122.qiniudn.com/image2018-5-27%2022_15_59.png)

### 0x09 按照类型排序 ###

击请求表每一列的列头，即可按照相应的类型进行排序，如，点击`Size`，则可按照资源从小到大或者从大到小（点击`Size`自动切换）进行排序。

![](http://zdx0122.qiniudn.com/image2018-6-3%2011_9_42.png)

### 0x10 按照请求的不同阶段排序 ###

在请求表的列头右键，然后鼠标移动到`Waterfall`，然后选择以下选项，默认按照对应时间从短到长的顺序排列：

- `Start Time`：请求开始的时间（默认）
- `Response Time`：资源开始下载的时间
- `End Time`：请求结束的时间
- `Total Duration`：请求的整个持续时间（发起至下载结束）
- `Latency` 请求等待响应的时间

![](http://zdx0122.qiniudn.com/image2018-6-3%2011_15_50.png)

比如，选择了`Total Duration`，`Waterfall`如下图所示：

![](http://zdx0122.qiniudn.com/image2018-6-3%2011_25_6.png)

注：上图中的不同颜色代表不同的文件类型，如js、img、css等。每个请求的瀑布流图像都分为浅色部分和深色部分，

浅色部分表示等待时间，深色部分表示下载时间，如上图中66ms是等待时间，58ms是下载资源所用的时间。

### 0x11 查看请求在各个阶段对应的时间 ###

![](http://zdx0122.qiniudn.com/1810613275-58aa493eb89c3_articlex.jpg)

- `Queueing`：浏览器会在以下情况对请求进行排队：
	- 有更高优先级的请求
	- 在这个域下，已经有6个TCP连接了，达到Chrome最大限制数量。此条规则仅适用在HTTP/1.0和HTTP/1.1
- `Stalled`：Queueing中的任何一个因素发生都会导致该请求被拖延
- `Proxy negotiation`：浏览器与代理服务器协商消耗的时间
- `DNS Lookup`：浏览器对请求的IP地址进行DNS查找所消耗的时间
- `Initial conncection`：发起连接所消耗的时间
- `Request sent`：请求发送消耗的时间
- `Waiting (TTFB)`：浏览器等待响应的时间，TTFB表示 Time To First Byte
- `Content Download`：资源下载所消耗的时间

### 0x12 请求表有简版和详细版两种不同的显示，默认是简版 ###

![](http://zdx0122.qiniudn.com/image2018-6-3%2021_21_23.png)

![](http://zdx0122.qiniudn.com/image2018-6-3%2021_33_15.png)

其实详细版就是多提供了一部分信息：

- `Name`列多了一行灰色的文字，表示该资源的路径
- `Status`列多了一行灰色的文字，表示HTTP状态码对应的文本
- `Initiator`列多了一行灰色的文字，表示发起对象类型
- `Size`列多了一行灰色的文字，表示该资源的实际大小 
- `Size`列的第一行数据表示请求头和请求体的大小之和，由于HTTP请求的多样，会导致第一行数据的大小和第二行数据大小的不同，有可能第一行的数据比第二行的数据大，也可能第一行的数据比第二行的数据小，一般有以下几种原因：
	- 有响应头，甚至包含cookie（第一行 > 第二行）
	- 请求被缓存了（一般情况下，第一行 < 第二行）
	- 服务端gizp压缩（一般情况下，第一行 < 第二行）
- `Time`列多了一行灰色的文字，表示请求等待响应的时间

### 0x13 XHR 重放 ###

在 ajax 请求上右键，选择 `Replay XHR`，可以将重新再发一遍，这在前端调试接口时比较好用。

![](http://zdx0122.qiniudn.com/image2018-6-6%2022_35_38.png)

