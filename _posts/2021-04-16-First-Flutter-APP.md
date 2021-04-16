---
layout: post
title:  "First Flutter APP"
date:   2021-04-16 
categories: Flutter
tags:  Flutter Dart 
author: i.itest.ren
---

* content
{:toc}

Flutter is Google’s UI toolkit for building beautiful, natively compiled applications for mobile, web, and desktop from a single codebase.





## 移动开发技术介绍

通过移动开发技术介绍，有助于理解Flutter诞生的背景和必要性

https://book.flutterchina.club/chapter1/mobile_development_intro.html

## Flutter介绍

Flutter是一个跨平台UI开发的框架，可通过一个代码库为Android、IOS、web(h5)和桌面应用等打造精美和快速的用户体验

- 支持毫秒级的热重载
- 富有表现力和灵活的UI
- 达到原生应用一样的性能

类似的框架：
- React Native: React Native将React的声明性UI框架引入iOS和Android。
- weex: Weex是一个可以使用现代化的 Web 技术开发高性能原生应用的框架。

区别：

WeeX 和 React Native是基于JavaScript语言开发，相对来说学习成本更低。但JavaScript在移动端的性能表现比较一般，flutter采用Dart语言，有接近原生（Native）的性能表现。

### 为什么要学习Flutter？

内因：
公司今年移动端大方在向flutter技术转型，不少业务采用flutter实现，或采用flutter重构。那么测试也要跟上做一些测试相关技术的预研。
知己知彼，当然要上手撸点Flutter代码。

外因：编写一遍代码能生成安卓/ios/web/桌面程序等多个平台的app，值得去花费精力去学习和尝试。

大胆的想象下，以后招聘的APP工程师、前端工程师可能要必备Flutter技能。

需要说明的是Flutter的Issue还有超过5K+，问题有很多，但参与开源的人更多（star超10W+），相信通过开源的力量能让这个框架更加稳定和完美。

## 安装

官方教程中文版：https://flutter.cn/docs/get-started/install/windows

### 安装Flutter SDK

下载最新版的Flutter，最新版是2.0+，自2.0大版本后支持生成web程序。
官方提醒注意：请勿将 Flutter 安装在需要高权限的文件夹内，例如 C:\Program Files\

下载链接：https://storage.flutter-io.cn/flutter_infra/releases/stable/windows/flutter_windows_2.0.4-stable.zip


```
设置环境变量：
FLUTTER_HOME：D:\TestWork\WorkSpace\flutter_windows_2.0.4-stable\flutter
Path增加：%FLUTTER_HOME%\bin
```

打开CMD命令行工具，正常输出以下内容即是SDK安装成功：

```
C:\Users\daojia>flutter --version
Flutter 2.0.4 • channel stable • https://github.com/flutter/flutter.git
Framework • revision b1395592de (2 weeks ago) • 2021-04-01 14:25:01 -0700
Engine • revision 2dce47073a
Tools • Dart 2.12.2
```

输入flutter doctor命令，可查看开发环境安装情况

### 安装Android SDK 或者IOS SDK

#### Android SDK

Google官方未单独提供Android SDK的下载地址，需要下载Android Studio，利用其下载SDK tools等。

强烈建议使用Android Studio下载，这里踩了大坑，从网络上单独下载的SDK tools会在编译运行Flutter程序时提示未同意licences，原因是网上提供的SDK是Google长时间未更新的版本。

安装后，设置SDK环境变量，如下：

```
新增ANDROID_HOME：D:\TestWork\WorkSpace\sdk
Path增加：
%ANDROID_HOME%\tools
%ANDROID_HOME%\platform-tools
```

对应安卓系统版本的build-tools可酌情安装，在第一次编译程序时，也会自动下载对应安卓系统版本的build-tools。

有安卓真机作为开发调试机器时，不需要下载模拟器模块（安装包过大）。

#### IOS SDK

本文使用Windows系统（不支持IOS），暂不安装iOS SDK和打包ios应用，使用mac同学自行安装，略过不表。

### 安装IDE

官方推荐的三款IDE，本文采用VS Code。

- Android Studio and IntelliJ
- Visual Studio Code
- Emacs

安装和设置方法如：https://flutter.cn/docs/get-started/editor?tab=vscode

安装VS Code：https://code.visualstudio.com/

安装 Flutter 和 Dart 插件

1. 打开 VS Code。
1. 打开 View > Command Palette….
1. 输入 “install”，然后选择 Extensions: Install Extensions
1. 在扩展搜索输入框中输入 “flutter”，然后在列表中选择 Flutter 并单击 安装。此过程中会自动安装必需的 Dart 插件。


### 检查开发环境

cmd命令行中，运行flutter doctor未报错即是开发环境搭建完毕


```
C:\Users\daojia>flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 2.0.4, on Microsoft Windows [Version 10.0.18363.1500], locale zh-CN)
[√] Android toolchain - develop for Android devices (Android SDK version 30.0.3)
[√] Chrome - develop for the web
[√] Android Studio (version 4.1.0)
[√] IntelliJ IDEA Community Edition (version 2018.1)
[√] VS Code (version 1.55.2)
[√] Connected device (3 available)

• No issues found!
```

## Dart语言简介

https://book.flutterchina.club/chapter1/dart.html

https://dart.dev/overview

和Java、JavaScript类似，很快能上手

## 第一个Flutter APP

### 创建应用

以VS Code为例，创建应用：

1. 打开 View > Command Palette。
1. 输入 “flutter”，选择 Flutter: New Application Project。
1. 输入项目名称，比如 myapp，然后点 Enter
1. 创建或者选择新项目的父文件夹。
1. 稍等一下项目创建成功，目录中就会生成 main.dart 文件。

生成后的代码目录如图：
![image](https://tva2.sinaimg.cn/large/6b6f6a6fly1gpls29oldaj20sg0k8n0a.jpg)

### 运行应用：

1. 找到 VS Code 的状态栏(窗口底部蓝色的条)
1. 从 Device Selector 里选择一个设备。
1. 选择 Run > 开始 Debugging 或者按F5
1. 当应用启动以后— 处理进度会出现在 Debug Console 页面中。
1. 当应用编译完成后，就可以在设备上运行这个起步应用了。



Android:

![image](https://tvax3.sinaimg.cn/large/6b6f6a6fly1gplsfdx4haj208w0injs9.jpg)

Chrome web:

![image](https://tvax3.sinaimg.cn/large/6b6f6a6fly1gplsjkwolxj20t60jomyh.jpg)

### 代码分析

参考：https://book.flutterchina.club/chapter2/first_flutter_app.html

在这个计数器示例中，主要Dart代码是在 lib/main.dart 文件中，下面是它的源码：

```
import 'package:flutter/material.dart';//导入包

//应用入口main()方法，可以看出dart语言和Java编码类似
void main() {
  runApp(MyApp());
}
//所有的页面都是由Widget组成的
//StatelessWidget代表的是无状态的小部件，相对应有状态的是StatefulWidget
class MyApp extends StatelessWidget {
  // 这个小部件是你应用的结构
  
  @override// 继承StatelessWidget后，需要重写build()，来描述如何构建UI界面
  Widget build(BuildContext context) {
    return MaterialApp(//MaterialApp 是Material库中提供的Flutter APP框架,MaterialApp也是一个widget
      title: 'Flutter Demo',//标题
      theme: ThemeData(
        // 应用的主题
        // 主题的颜色是蓝色
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),//首页，也是一个Widget
    );
  }
}
// 首页代码，它属于有状态的小部件，个人理解有状态的意思是小部件内存在交互反馈
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);

  final String title;//首页的标题

  @override//重写createState()
  _MyHomePageState createState() => _MyHomePageState();
}

//State类
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;//用于记录按钮点击的总次数

  // 设置状态的自增函数
  void _incrementCounter() {
    setState(() {// setState方法的作用是通知Flutter框架，有状态发生了改变，Flutter框架收到通知后，会执行build方法来根据新的状态重新构建界面

      _counter++;//点击一次，次数自增一次
    });
  }

  @override// widget和state类都需要重写build方法用来构建页面
  Widget build(BuildContext context) {
    // 构建UI界面的逻辑在build方法中，当MyHomePage第一次创建时，_MyHomePageState类会被创建，当初始化完成后，Flutter框架会调用Widget的build方法来构建widget树，最终将widget树渲染到设备屏幕上。
    return Scaffold(//Scaffold 是 Material 库中提供的页面脚手架,提供了导航栏、body等
      appBar: AppBar(// 导航栏
        title: Text(widget.title),
      ),
      body: Center(//居中属性的小部件

        child: Column(// 包含两个Text文本小部件

          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',//固定写死的文案
            ),
            Text(
              '$_counter',//动态取记录的按钮的总次数
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(//右下角的按钮
        onPressed: _incrementCounter,//点击后调用上面的自增的方法
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```

### 热重载

保持IDE在debug运行状态，修改一处文案，保存代码，代码变动会迅速体现在APP上。

### 使用三方依赖

pubspec.yaml是管理依赖包的文件，类似于pom.xml

```
dependencies:
    flutter:
      sdk: flutter
    cupertino_icons: ^1.0.2
    english_words: ^4.0.0-0
```

保存会自动拉取三方依赖包；右键文件点击“Get Packages”按钮也可以执行拉取动作。


## Flutter 基础知识

中心思想：一切都是Widget(小部件/组件)



### Widget简介

https://book.flutterchina.club/chapter3/flutter_widget_intro.html

Widget是Flutter构建UI的关键

在Flutter中，Widget的功能是“描述一个UI元素的配置数据”，它就是说，Widget其实并不是表示最终绘制在设备屏幕上的显示元素，而它只是描述显示元素的一个配置数据。

Widget实际上就是Element的配置数据，Widget树实际上是一个配置树，而真正的UI渲染树是由Element构成；不过，由于Element是通过Widget生成的，所以它们之间有对应关系，在大多数场景，我们可以宽泛地认为Widget树就是指UI控件树或UI渲染树。

一个Widget对象可以对应多个Element对象。这很好理解，根据同一份配置（Widget），可以创建多个实例（Element）。

Widget不仅可以表示UI元素，也可以表示一些功能性的组件

StatelessWidget和StatefulWidget都是直接继承自Widget类，这两个类也正是Flutter中非常重要的两个抽象类，它们引入了两种Widget模型。

StatelessWidget用于不需要维护状态的场景，它通常在build方法中通过嵌套其它Widget来构建UI，在构建过程中会递归的构建其嵌套的Widget。

和StatelessWidget一样，StatefulWidget也是继承自Widget类，并重写了createElement()方法，不同的是返回的Element 对象并不相同；另外StatefulWidget类中添加了一个新的接口createState()。

### 基础组件

- Text：创建一段有样式的文本
- Row，Colmun：这两个控件可以让你用web Flex布局模型来构建UI
- Stack: 可以让控件脱离上下文，相对于屏幕上下左右，等同于web里的绝对定位模型
- Container：创建一个矩形元素，等同于web里的div，他有外边距内边距。


### pubspec.yaml

- 包管理，下载三方依赖包
- 资源管理，操作assets

### 文件操作与网络请求

https://book.flutterchina.club/chapter11/

### 路由管理

https://book.flutterchina.club/chapter2/flutter_router.html

### 调试Flutter应用

https://book.flutterchina.club/chapter2/flutter_app_debug.html

#### 静态代码检查工具

在运行应用程序前，运行flutter analyze测试你的代码

#### 查看日志

Dart print()功能将输出到系统控制台，您可以使用flutter logs来查看它（基本上是一个包装adb logcat）。

#### Dart Observatory (语句级的单步调试和分析器)

如果我们使用flutter run启动应用程序，那么当它运行时，我们可以打开Observatory工具的Web页面，例如Observatory默认监听http://127.0.0.1:8100/，可以在浏览器中直接打开该链接。直接使用语句级单步调试器连接到您的应用程序。

#### 统计应用启动时间

要收集有关Flutter应用程序启动所需时间的详细信息，可以在运行flutter run时使用trace-startup和profile选项。


```
$ flutter run --trace-startup --profile
```

跟踪输出保存为start_up_info.json，在Flutter工程目录在build目录下。输出列出了从应用程序启动到这些跟踪事件（以微秒捕获）所用的时间：

- 进入Flutter引擎时.
- 展示应用第一帧时.
- 初始化Flutter框架时.
- 完成Flutter框架初始化时.

### Flutter框架结构

Flutter框架本身有着良好的分层设计，框架结构如图作为了解：

![image](https://tvax3.sinaimg.cn/large/6b6f6a6fly1gplqq3lkpgj20mn0ch0tc.jpg)

#### Flutter Framework

这是一个纯 Dart实现的 SDK，它实现了一套基础库，自底向上，我们来简单介绍一下：

- 底下两层（Foundation和Animation、Painting、Gestures）在Google的一些视频中被合并为一个dart UI层，对应的是Flutter中的dart:ui包，它是Flutter引擎暴露的底层UI库，提供动画、手势及绘制能力。
- Rendering层，这一层是一个抽象的布局层，它依赖于dart UI层，Rendering层会构建一个UI树，当UI树有变化时，会计算出有变化的部分，然后更新UI树，最终将UI树绘制到屏幕上，这个过程类似于React中的虚拟DOM。Rendering层可以说是Flutter UI框架最核心的部分，它除了确定每个UI元素的位置、大小之外还要进行坐标变换、绘制(调用底层dart:ui)。
- Widgets层是Flutter提供的的一套基础组件库，在基础组件库之上，Flutter还提供了 Material 和Cupertino两种视觉风格的组件库。而我们Flutter开发的大多数场景，只是和这两层打交道。

#### Flutter Engine

这是一个纯 C++实现的 SDK，其中包括了 Skia引擎、Dart运行时、文字排版引擎等。在代码调用 dart:ui库时，调用最终会走到Engine层，然后实现真正的绘制逻辑。




## 参考资料
- 开源地址：https://github.com/flutter/flutter
- 官网：https://flutter.dev/
- 官网中文版：https://flutter.cn
- Flutter中文网：https://flutterchina.club/
- 《Flutter实战》电子书：https://book.flutterchina.club/