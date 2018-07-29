---
layout: post
title:  "使用Chrome开发者工具调试Android端内网页(微信，QQ，UC，App内嵌页等)"
date:   2018-07-24 01
categories: 工具技巧
tags:  Chrome 工具技巧
author: i.itest.ren
---

* content
{:toc}

## 前言 ##
![](http://zdx0122.qiniudn.com/timg.jpg)

移动端页面调试一直是好多朋友头疼的问题，iOS 由于其封闭的特性和整体较高的性能，整体适配相对好做，调试也由于 safari 的打通变得很方便。

而 Android 系统，尤其是国内环境下的 Android 系统，由于版本跨度大，且各家厂商采用自研内核，使得其成为移动端页面问题出现的主要平台。

工程师们采用了各种各样的方式来对其进行调试：alert 大法，页面引入或注入VConsole/eruda开启移动端控制台，Fiddler / Charles / Whistle抓包，Weinre......，各种解决方式层出不穷，但随着国内厂商积极的更新内核和旧版本Android机型的逐渐淘汰，在当前节点下，我们只需要做一点微小的工作，就可以使用我们最熟悉的 Chrome 开发者工具对移动端页面进行调试。

这篇文章主要讲述使用 Chrome 开发者工具调试 Android 终端内的各种 webview 环境下页面的方法，主要包含以下环境：
- Chrome 等基于原生 Chromium 内核的浏览器
- 微信、QQ、QQ 浏览器等 X5 内核webviewUC 
- 浏览器等 U4 以上内核 WebviewApp 
- 内嵌 Webview
- 系统浏览器
如果你已经分别知道以上每种环境中开启 Inspect 调试的方法，这篇文章对你来说意义不大，可以选择关闭。

但如果以上有你不知道如何开启 Inspect 调试的环境甚至你不知道 Chrome 的 Inspect 调试，可以继续往下看，应该会对你之后的调试工作有所帮助，Let's do it！






## 开启 Chrome 浏览器 Inspect 调试 ##

第一次启用inspect调试如果一直loading不进去说明需要翻墙，翻过一次墙后后面就不需要了，不过有时候Chrome版本更新就需要再翻墙一次

在 Chrome 浏览器地址栏中输入 `chrome://inspect/#devices` 并回车，就可以打开 `Inspect` 调试界面，此时我们勾选 `Discover USB devices` 选项便可以看到设备列表。

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724131231.png)

然后打开手机中开发者选项并打开 USB 调试开关（具体方法自行百度，不同手机有一定区别），使用数据线将手机连接到电脑上，我们就可以在设备列表中看到自己的设备。

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724131322.png)

这时，如果你手机上安装了 Chrome 浏览器的话，随便打开一个网址（以掘金为例），设备列表中你的设备下便会出现你打开的页面。

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724131414.png)

此时我们点击 `inspect` 选项

看到这个熟悉的界面了吗？接下来你便可以和调试 PC 界面一样通过 Chrome 进行你所需要的调试，你在左侧屏幕上做的一切操作和你的手机上的操作会始终保持同步，如果你嫌左边这块多余，也可以关闭 Toggle Screencast 只保留控制台本身。

如果你之前没有使用过这种调试方式，你应该会感到相比之前的调试方式更加方便快捷问题来了，Chrome 手机浏览器在国内市场份额非常少，我们的页面主要出现的地方也是微信，QQ，UC浏览器或者 App 内嵌等等，针对这些环境，如何开启 inspect 调试？下面我们逐一讨论。

## 微信、QQ、QQ浏览器等 X5 内核 Webview ##

X5 内核环境下，我们访问 `debugx5.qq.com/`

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724131758.png)

在这个页面中我们可以对 X5 内核进行配置（如非必要不建议改动配置），我们选择信息Tab并勾选“`打开TBS内核Inspector调试功能`”和“`打开TBS内核X5jscore Inspector调试功能`”两个选项，完成后重启页面生效

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724131953.png)

然后 inspect 页面就会自动刷新列表，这时我们就能对微信及 QQ 等 X5 内核 Webview 进行调试

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724132033.png)

## UC 浏览器等 U4 以上内核 Webview ##

针对U4及以上内核，我们访问 `plus.ucweb.com/download/`，便可下载中文版及国际版 UC 浏览器的对应开发版

然后我们在 UC 开发版中打开要调试的页面，和上面一样，我们也可以在列表中看到 UC 浏览器打开的页面并对其进行调试

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724132126.png)

## App 内嵌 Webview ##

很多团队会采用 Hybrid 模式开发业务，针对 App 内嵌页面，我们需要 Android 同事协助打开 Webview 调试，具体方法为：
```java
webView.setWebContentsDebuggingEnabled(true);
```
每个 webview 组件实例需要单独设置，开启后便可进行 inspect 调试

## 系统自带浏览器 ##

针对 MIUI 自带浏览器（MIUI为例），我们需要刷入开发版系统，然后系统自带浏览器便可以通过 inspect 进行调试

![](http://zdx0122.qiniudn.com/QQ%E6%8B%BC%E9%9F%B3%E6%88%AA%E5%9B%BE20180724132346.png)

以上就是在使用 Chrome 开发者工具调试 Android 端各种环境 Web 页面的方法，希望能帮助到你.

