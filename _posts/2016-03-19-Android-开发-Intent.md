---
layout: post
title:  "Android开发入门-使用Intent实现页面跳转"
date:   2016-03-19 
categories: Android
---

## Android开发入门-使用Intent实现页面跳转 ##
### 什么是Intent ###
Intent可以理解为信使（意图）

由Intent来协助完成Android各个组件之间的通讯

### 两种实现方式 ###
1. startActivity(intent)
2. startActivityForResult(intent, requestCode);

接收回传数据：

onActivityResult(int requestCode, int resultCode, Intent data)

setResult(resultCode, data)

 



