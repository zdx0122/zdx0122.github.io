---
layout: post
title:  "【转】Android Monkey 命令行可用的全部选项"
date:   2016-03-08 
categories: Android
---

原文参见：http://www.douban.com/note/257030384/

## 常规 ##

--help

列出简单的用法。

-v

命令行的每一个 -v 将增加反馈信息的级别。

- Level 0(缺省值)除启动提示、测试完成和最终结果之外，提供较少信息。
- Level 1提供较为详细的测试信息，如逐个发送到Activity的事件。
- Level 2提供更加详细的设置信息，如测试中被选中的或未被选中的Activity。


日志级别 Level 0

示例 `adb shell monkey -p com.htc.Weather –v 100`

说明缺省值，仅提供启动提示、测试完成和最终结果等少量信息

日志级别 Level 1

示例 `adb shell monkey -p com.htc.Weather –v -v 100`

说明 提供较为详细的日志，包括每个发送到Activity的事件信息

日志级别 Level 2

示例 `adb shell monkey -p com.htc.Weather –v -v –v 100`

说明 最详细的日志，包括了测试中选中/未选中的Activity信息

## 事件 ##

	-s <seed>

用于指定伪随机数生成器的seed值，如果seed相同，则两次Monkey测试所产生的事件序列也相同的。

示例：

Monkey 测试1：`adb shell monkey -p com.htc.Weather –s 10 100`

Monkey 测试2：`adb shell monkey -p com.htc.Weather –s 10 100`

两次测试的效果是相同的，因为模拟的用户操作序列（每次操作按照一定的先后顺序所组成的一系列操作，即一个序列）是一样的。操作序列虽 然是随机生成的，但是只要我们指定了相同的 Seed 值，就可以保证两次测试产生的随机操作序列是完全相同的，所以这个操作序列伪随机的；


    --throttle<milliseconds>

在事件之间插入固定延迟。通过这个选项可以减缓 Monkey 的执行速度。如果不指定该选项，Monkey 将不会被延迟，事件将尽可能快地被产成。

示例：`adb shell monkey -p com.htc.Weather –throttle 3000 100`


    --pct-touch<percent>

调整触摸事件的百分比(触摸事件是一个 down-up 事件，它发生在屏幕上的某单一位置)。

示例：`adb shell monkey -p com.htc.Weather --pct-touch 10 1000`


    --pct-motion<percent>

调整动作事件的百分比(动作事件由屏幕上某处的一个 down 事件、一系列的伪随机事件和一个 up 事件组成)。

示例：`adb shell monkey -p com.htc.Weather --pct-motion 20 1000`


    --pct-trackball<percent>

调整轨迹事件的百分比(轨迹事件由一个或几个随机的移动组成，有时还伴随有点击)。

示例：`adb shell monkey -p com.htc.Weather --pct-trackball 30 1000`


    --pct-nav<percent>

调整“基本”导航事件的百分比(导航事件由来自方向输入设备的 up/down/left/right 组成)。

示例：`adb shell monkey -p com.htc.Weather --pct-nav 40 1000`


    --pct-majornav<percent>

调整“主要”导航事件的百分比(这些导航事件通常引发图形界面中的动作，如：5-way键盘的中间按键、回退按键、菜单按键)

示例：`adb shell monkey -p com.htc.Weather --pct-majornav 50 1000`


    --pct-syskeys<percent>

调整“系统”按键事件的百分比(这些按键通常被保留，由系统使用，如 Home、Back、Start Call、End Call 及音量控制键)。

示例：`adb shell monkey -p com.htc.Weather --pct-syskeys 60 1000`


    --pct-appswitch<percent>

调整启动 Activity 的百分比。在随机间隔里，Monkey 将执行一个 startActivity() 调用，作为最大程度覆盖包中全部Activity 的一种方法。

示例：`adb shell monkey -p com.htc.Weather --pct-appswitch 70 1000`


    --pct-anyevent<percent>

调整其它类型事件的百分比。它包罗了所有其它类型的事件，如：按键、其它不常用的设备按钮、等等。

示例：`adb shell monkey -p com.htc.Weather --pct -anyevent 100 1000`

指定多个类型事件的百分比：

    adb shell monkey -pcom.htc.Weather --pct-anyevent 50 --pct-appswitch 50 1000

注意：各事件类型的百分比总数不能超过100%；

## 约束限制 ##

-p<allowed-package-name>

如果用此参数指定了一个或几个包，Monkey 将只允许系统启动这些包里的 Activity。如果你的应用程序还需要访问其它包里的 Activity (如选择取一个联系人)，那些包也需要在此同时指定。如果不指定任何包，Monkey 将允许系统启动全部包里的 Activity。要指定多个包，需要使用多个 -p 选项，每个 -p 选项只能用于一个包。

指定一个包： `adb shell monkey -p com.htc.Weather 100`

说明：com.htc.Weather为包名，100是事件计数（即让Monkey程序模拟100次随机用户事件）。

指定多个包：`adb shell monkey -p com.htc.Weather –p com.htc.pdfreader -p com.htc.photo.widgets 100`

不指定包：`adb shell monkey 100`

说明：Monkey随机启动APP并发送100个随机事件。

要查看设备中所有的包，在CMD窗口中执行以下命令：

    >adb shell
    #cd data/data
    #ls

-c<main-category>

如果用此参数指定了一个或几个类别，Monkey 将只允许系统启动被这些类别中的某个类别列出的 Activity。如果不指定任何类别，Monkey 将选 择下列类别中列出的 Activity： Intent.CATEGORY_LAUNCHER 或Intent.CATEGORY_MONKEY。要指定多个类别，需要使用多个 -c 选项，每个 -c 选 项只能用于一个类别。

## 调试 ##

--dbg-no-events

设置此选项，Monkey将执行初始启动，进入到一个测试Activity，然后不会再进一步生成事件。为了得到最佳结果，把它与-v、一个或几个包约 束、以及一个保持Monkey运行30秒或更长时间的非零值联合起来，从而提供一个环境，可以监视应用程序所调用的包之间的转换。

--hprof

设置此选项，将在Monkey事件序列之前和之后立即生成profiling报告。这将会在data/misc中生成大文件(~5Mb)，所以要小心使用它。

--ignore-crashes

通常，当应用程序崩溃或发生任何失控异常时，Monkey将停止运行。如果设置此选项，Monkey将继续向系统发送事件，直到计数完成。

示例1：`adb shellmonkey -p com.htc.Weather --ignore-crashes 1000`

测试过程中即使Weather程序崩溃，Monkey依然会继续发送事件直到事件数目达到1000为止；

示例2：`adb shell monkey -p com.htc.Weather 1000`

测试过程中，如果Weather程序崩溃，Monkey将会停止运行。

--ignore-timeouts

通常，当应用程序发生任何超时错误(如“Application Not Responding”对话框)时，Monkey 将停止运行。如果设置此选项，Monkey 将继续向系统发送事件，直到计数完成。

--ignore-security-exceptions

通常，当应用程序发生许可错误(如启动一个需要某些许可的 Activity )时，Monkey 将停止运行。如果设置了此选项，Monkey 将继续向系统发送事件，直到计数完成。

--kill-process-after-error

通常，当 Monkey 由于一个错误而停止时，出错的应用程序将继续处于运行状态。当设置了此选项时，将会通知系统停止发生错误的进程。注意，正常的(成功的)结束，并没有停止启动的进程，设备只是在结束事件之后，简单地保持在最后的状态。

--monitor-native-crashes

监视并报告 Android 系统中本地代码的崩溃事件。如果设置了 --kill-process-after-error，系统将停止运行。

--wait-dbg

停止执行中的 Monkey，直到有调试器和它相连接。