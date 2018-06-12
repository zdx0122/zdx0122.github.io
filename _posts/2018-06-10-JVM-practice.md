---
layout: post
title:  "JVM运维实践"
date:   2018-06-10 01
categories: Java
tags:  Java
author: Dex
---

* content
{:toc}

JVM运维实践





内容从公司[技术学院](https://tech.daojia.com/)分享转载~

## JVM简介 ##

	JVM —— Java Virtual Machine
	JRE —— Java Runtime Environment
	JDK —— Java Development Kit 

### JVM体系 ###

根据 `JVM` 规范，`JVM` 内存共分为：

- 方法区
- 堆
- 本地方法栈
- 虚拟机栈
- 程序计数器

![](http://zdx0122.qiniudn.com/JVM%E4%BD%93%E7%B3%BB.png)

![](http://zdx0122.qiniudn.com/Hotspot%E5%A0%86.png)

![](http://zdx0122.qiniudn.com/Hotspot%E5%A0%862.png)

GC过程：

![](http://zdx0122.qiniudn.com/GC%E8%BF%87%E7%A8%8B.png)



## JDK命令行工具 ##

- `jps`：`JVM Process Status Tool`，显示指定系统内所有`HotSpot`虚拟机进程
- `jinfo`：`Configuration Info for Java`，显示虚拟机配置信息
- `jstat`：`JVM Statistics Minitoring Tool`，用于收集`HotSpot`虚拟机各方面的运行数据
- `jmap`：`Memory Map for Java`，生成虚拟机的内存转储快照（`heapdump`）文件
- `jhat`：`JVM Heap Dump Browser`，用于分析`heapdump`文件，它会建立一个`HTTP/HTML`服务器，让用户可以在浏览器上查看分析结果
- `jstack`：`Stack Trace for Java`，显示虚拟机的线程快照

### jps ###

`jps`（JVM Process Status Tool），功能和`ps`类似：可以列出正在运行的虚拟机进程，并显示虚拟机执行主类（`Main Class`，main()函数所在的类）的名称，以及这些进程的本地虚拟机的唯一`ID`

### jinfo ###

`jinfo`（Configuration Info for Java）的作用是实时地查看和调整虚拟机的各项参数

- `-flag <name> pid`：打印指定JVM的参数值
- `-flag pid`：打印所有非默认JVM的参数值  
- `-flag [+|-]<name> pid`：设置指定JVM参数的布尔值
- `-flag <name>=<value> pid`：设置指定JVM参数的值
- `-sysprops pid`:打印系统变量

### jstat ###

`jstat`：JVM Statistics Minitoring Tool，用于收集HotSpot虚拟机各方面的运行数据

`jstat`命令格式：`jstat [option vmid [interval[s|ms] [count]] ]`

--参数`interval`和`count`代表查询间隔和次数，如果省略这两个参数，说明只查询一次。假设需要每225毫秒查询一次进程12448垃圾收集状况，一共查询5次，那命令行为：`jstat -gc 12448 225 5`

每1s查询一次12448的垃圾回收情况，各空间以百分比的形式输出，命令为： `jstat -gcutil 12448 1s`

`jstat`的选项`option`代表这用户希望查询的虚拟机信息，主要分为3类：类装载、垃圾收集和运行期编译状况，具体选项及使用参见下表

- `-class`	监视类装载、卸载数量、总空间及类装载所耗费的时间
- `-gc`	监视Java堆状况，包括Eden区、2个Survivor区、老年代、永久代等的容量
- `-gccapacity`	监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大和最小空间
- `-gcutil`	监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比
- `-gccause`	与-gcutil功能一样，但是会额外输出导致上一次GC产生的原因
- `-gcnew`	监视新生代GC的状况
- `-gcnewcapacity`	监视内容与-gcnew基本相同，输出主要关注使用到的最大和最小空间
- `-gcold`	监视老年代GC的状况
- `-gcoldcapacity`	监视内容与——gcold基本相同，输出主要关注使用到的最大和最小空间
- `-gcmetacapacity`	输出元空间使用到的最大和最小空间
- `-compiler`	输出JIT编译器编译过的方法、耗时等信息
- `-printcompilation`	输出已经被JIT编译的方法

### jmap ###

`jmap`（Memory Map for Java）命令用于生产堆转储快照（一般称为`heapdump`或`dump`文件）。

如果不使用jmap命令，要想获取Java堆转储快照怎么办？

- `-XX:+HeapDumpOnOutOfMemoryError`参数，可以让虚拟机在OOM异常出现之后自动生生成dump文件
- 通过`-XX:+HeapDumpOnCtrlBreak`参数则可以使用[Ctrl]+[Break]键让虚拟机生成dump文件
- 又或者在Linux系统下通过`Kill -3`命令发送进程退出信号”恐吓“一下虚拟机，也能拿到dump文件。

`jmap`的作用并不仅仅是为了获取dump文件，它还可以查询finalize执行队列，Java堆和永久代的详细信息，如空间使用率、当前用的是那种收集器等。

`jmap`命令格式： `jmap [option] vmid`

`option`选项合法值与具体含义：

- `-dump`	生成Java堆转储快照。格式为：`-dump:[live,]format=b,file=<filename>`，其中live子参数说明是否只dump出存活的对象
- `-finalizerinfo`	显示在F-Queue中等待Finalizer线程执行finalize()方法的对象。只在Linux/Solaris平台下有效
- `-heap`	显示Java堆详细信息，如使用哪种回收器、参数配置、分代状况等。只在Linux/Solaris平台下有效
- `-histo`	显示堆中对象统计信息，包括类、实例数量和合计容量
- `-permstat`	以ClassLoader为统计口径显示永久代内存状态。只在Linux/Solaris平台下有效
- `-F`	当虚拟机进程对-dump选项没有响应时，可使用这个选项强制生成dump快照。只在Linux/Solaris平台下有效

`jmap -dump:file=/temp/dump.bin 18474`  生成内存快照到`dump.bin`文件

`jhat /tmp/dump.bin` 将`dump.bin`的内容以web形式暴露，默认`7000`端口

### jstack ###

`jstack`（Stack Trace for Java）命令用于生成虚拟机当前时刻的线程快照（一般称为`threaddump`或`javacore`文件）。

线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的常见原因。

jstack命令格式：  `jstack [option] vmid`

`option`选项的合法值与具体意义如下：

- `-F`	当正常输出的请求不被响应时，强制输出线程堆栈
- `-l`	除堆栈外，显示关于锁的附加信息
- `-m`	如果调用到本地方法的话，可以显示C/C++的堆栈

### Java Monitor ###

![](http://zdx0122.qiniudn.com/java-monitor.png)

![](http://zdx0122.qiniudn.com/java-monitor2.png)

`jstack` 线程里，值得关注的线程状态有：

- 死锁，`Deadlock`（重点关注） 
- 执行中，`Runnable`   
- 等待资源，`Waiting on condition`（重点关注） 
- 等待获取监视器，`Waiting on monitor entry`（重点关注）
- 暂停，`Suspended`
- 对象等待中，`Object.wait()` 或 `TIMED_WAITING`
- 阻塞，`Blocked`（重点关注）  
- 停止，`Parked`


## JVM问题排查实践 ##


常见的一些疑难杂症? 

- OutOfMemoryError
- 内存泄露
- 死锁
- 死循环(类死循环)
- 锁争用(Lock Contention)
- CPU过高、load过高 
- GC不稳定
- Full GC频繁 

### 实例 1 ###

- 突然有一天我们发现整个服务器响应变慢了,看日志也没 发现什么明显异常。 
- 通过`top`命令查看概况

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%871.png)

- 通过`top –H p pid `查看线程状况

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%872.png)

找到线程号,转化为16进制数字 

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%873.png)

	jstack 25638 > jstack.log
	less jstack.log

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%874.png)

### 实例 2 ###

- 突然有一天发现监控发现JVM整体响应越来越慢,发 现load、CPU、内存等压力都不小 
- 通过`jmap –heap pid`发现堆空间中`perm区`很大 
- 发现监控中`JVM`的线程数有明显的上升 
- 打印出`jstack`文件后,通过linux命令找到那种状态线程较多 
- `grep java.lang.Thread.State jstack.log | sort | uniq -c`

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%875.png)

- 打印出这类线程的栈信息定位,然后具体分析
- `grep -A5 'java.lang.Thread.State: WAITING (parking)'  jstack.log `

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%876.png)

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%877.png)

发现大量线程处于等待状态`WAITING` (`parking`) ,根据栈信 息发现资源等待原因为SynchronousQueue的put操作,从而定位了问题 


### 实例 3 ###

- 突然有一天服务直接宕机了... ...宕机了... ... 于是半夜爬起来上线看日志,发现是发生了OOM,这时候 
- 重启服务还是会有发生OOM的风险 

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%878.png)

由于我们设置了JVM参数(`-XX:+HeapDumpOnOutOfMemoryError`),去找犯罪现场留下的证据 

- 通过HeapAnalyzer进行分析,有内存泄漏风险的是`hashmap`

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%879.png)

通过引用信息可以得知具体类信息,然后定位问题

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%8710.png)

- 内存泄漏还有一种表象,服务响应变慢,观察`gc`信息发现发生`fullgc`但是老年代回收效果很差

![](http://zdx0122.qiniudn.com/%E5%9B%BE%E7%89%8711.png)

