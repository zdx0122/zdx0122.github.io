---
layout: post
title:  "Android性能专项-内存测试方法"
date:   2016-07-23 01
categories: Android
---




## 内存测试方法 ##

目前存在的android的内存测试方法可以分为以下几类：

- 使用Android自身提供的 `ActivityManager.MemoryInfo()` 方法获得
- 使用android提供adb指令集获取内存信息即`adb shell dumpsys meminfo | grep packagename or pid` 来获取
- 使用android 提供的`procrank`获取
- 通过ADT插件DDMS查看用内存MAT进行分析


## dumpsys之meminfo ##

dumpsys获取内存信息： 

	C:\Users\Administrator>adb shell dumpsys meminfo com.wuba.huoyun
	Applications Memory Usage (kB):
	Uptime: 33840913 Realtime: 103845085
	
	** MEMINFO in pid 14688 [com.wuba.huoyun] **
	                   Pss  Private  Private  Swapped     Heap     Heap     Heap
	                 Total    Dirty    Clean    Dirty     Size    Alloc     Free
	                ------   ------   ------   ------   ------   ------   ------
	  Native Heap        0        0        0        0    13748    11720     1903
	  Dalvik Heap    19665    19576        0     5508    34244    20380    13864
	 Dalvik Other     4178     4088        0     1180
	        Stack      360      360        0       12
	    Other dev        4        0        4        0
	     .so mmap     1484      796       24     2212
	    .apk mmap      477        0      108        0
	    .dex mmap     2095       24      988       88
	   Other mmap       31        4        0        0
	      Unknown     9966     9948        0      852
	        TOTAL    38260    34796     1124     9852    47992    32100    15767
	
	 Objects
	               Views:      313         ViewRootImpl:        3
	         AppContexts:        4           Activities:        2
	              Assets:        5        AssetManagers:        5
	       Local Binders:       17        Proxy Binders:       26
	    Death Recipients:        2
	     OpenSSL Sockets:        0
	
	 SQL
	         MEMORY_USED:      328
	  PAGECACHE_OVERFLOW:       82          MALLOC_SIZE:       62
	
	 DATABASES
	      pgsz     dbsz   Lookaside(b)          cache  Dbname
	         4       28             40        87/31/4  /data/data/com.wuba.huoyun/da
	tabases/user-db
	         4       24             80       13/37/10  /data/data/com.wuba.huoyun/da
	tabases/bugly_db
	         4       28             64        28/37/8  /data/data/com.wuba.huoyun/da
	tabases/cars-db
	
	 Asset Allocations
	    zip:/system/framework/framework-res.apk:/resources.arsc: 1096K
	    zip:/data/app/com.wuba.huoyun-1.apk:/assets/fonts/HYQiH2312F45.ttf: 1525K

### 查看单个应用程序最大内存限制的指令 ###

	C:\Users\Administrator>adb shell getprop | findstr heapgrowthlimit
	[dalvik.vm.heapgrowthlimit]: [256m]

可以看到单个应用的最大内存限制为256M, 而`meminfo`里面`dalvik heap size`的最大值如果超过了256M就可能出现`OOM`。`dalvik.vm.heapgrowthlimit`和`dalvik.vm.heapsize`都是java虚拟机的最大内存限制，应用如果不想在`dalvik heap`达到`heapgrowthlimit`限制的时候出现OOM，需要在`Manifest`中的`application`标签中声明`android：largeHeap=“true”`，声明后，如果应用的dalvik heap 达到heapsize的时候才会出现OOM！

另：设备不一样，最大内存的限制也可能不一样.

C/C++申请的内存空间在native heap中，而java申请的内存空间则在`dalvik heap`中。这是因为Android系统对dalvik的vm heapsize作了硬性限制，当java进程申请的java空间超过阈值时，就会抛出OOM异常（这个阈值可以是48M、24M、16M等，视机型而定），可以通过`adb shell getprop | grep dalvik.vm.heapgrowthlimit`查看此值。

也就是说，程序发生OMM并不表示RAM不足，而是因为程序申请的java heap对象超过了dalvik vm heapgrowthlimit。也就是说，在RAM充足的情况下，也可能发生OOM.

### 查看单个应用的内存占有量的情况 ###

查看单个应用程序最大内存限制：`adb shell getprop|grep heapgrowthlimit`

	C:\Users\Administrator>adb shell getprop| findstr heapgrowthlimit
	[dalvik.vm.heapgrowthlimit]: [256m]

应用启动后分配的初始内存： `adb shell getprop|grep dalvik.vm.heapstartsize`

	C:\Users\Administrator>adb shell getprop | findstr dalvik.vm.heapstartsize
	[dalvik.vm.heapstartsize]: [8m]

单个java虚拟机最大的内存限制：`adb shell getprop|grep dalvik.vm.heapsize`

	C:\Users\Administrator>adb shell getprop | findstr dalvik.vm.heapsize
	[dalvik.vm.heapsize]: [512m]

## 使用android 提供的procrank获取 ##

通过指令：`adb shell procrank | grep packagename`

	C:\Users\Administrator>adb shell procrank
	  PID       Vss      Rss      Pss      Uss  cmdline
	  518   632108K   55712K   19193K   15340K  system_server
	  154    74640K   23352K   16888K   13844K  /system/bin/mediaserver
	  575   586896K   50352K   15898K   12804K  com.android.systemui
	  152   552684K   56896K   15135K    7968K  zygote
	  697   595208K   46336K   10955K    7276K  com.android.launcher
	  680   584928K   41292K    8632K    6552K  com.android.phone
	  752   583660K   42112K    7926K    4372K  com.hqgm.maoyt
	  828   583712K   42160K    7904K    4356K  com.hqgm.maoyt:push
	  664   570720K   39788K    7174K    5348K  com.android.inputmethod.latin
	 1237   572636K   38128K    6684K    5112K  com.android.email
	  997   564396K   38544K    6237K    4448K  android.process.media
	  729   565556K   37484K    6013K    4384K  android.process.acore
	 1132   572128K   35632K    5071K    3460K  com.android.mms
	 1187   568936K   35124K    4793K    3336K  com.android.calendar
	 1083   562580K   35476K    4396K    2912K  com.android.providers.calendar
	 1220   564680K   35544K    4184K    2416K  com.android.deskclock
	  651   572188K   33648K    4025K    2180K  com.android.settings
	 1030   565016K   32824K    3720K    2352K  com.android.dialer
	 1271   564396K   32696K    3689K    2348K  com.android.exchange
	  973   562256K   32912K    3418K    2032K  com.android.music
	 1258   562132K   33660K    3380K    1912K  com.genymotion.superuser
	  144    32156K    3580K    3241K    3232K  /system/bin/local_opengl
	  710   561884K   31704K    2862K    1536K  com.android.printspooler
	 1337   560680K   30640K    2529K    1216K  com.android.musicfx
	 1158   560552K   31044K    2526K    1260K  com.android.onetimeinitializer
	 1173   560692K   30712K    2455K    1196K  com.android.voicedialer
	  782   560548K   30744K    2436K    1152K  com.android.smspush
	  153    15148K    3928K    1462K     860K  /system/bin/drmserver
	 1369     2640K    1768K    1444K    1436K  procrank
	  151    54800K    3084K    1352K    1024K  /system/bin/surfaceflinger
	  641     5420K    2184K    1057K     888K  /system/bin/wpa_supplicant
	   78     9492K    2068K    1021K     836K  /system/bin/genyd
	  148    11340K    1700K     849K     748K  /system/bin/netd
	  156     4716K    1436K     481K     372K  /system/bin/keystore
	  140     6236K    1280K     479K     392K  /system/bin/vold
	  498     1680K     816K     468K     452K  logcat
	  150     6392K    1076K     416K     376K  /system/bin/rild
	    1      716K     440K     325K     240K  /init
	  824     1616K     744K     317K     292K  /system/bin/dhcpcd
	   82     6820K     312K     296K     296K  /sbin/adbd
	   84     3624K     648K     283K     272K  /system/bin/genybaseband
	   81     1548K     680K     263K     116K  /system/bin/sh
	  138     1600K     280K     260K     260K  /sbin/healthd
	  491     1548K     680K     259K     112K  /system/bin/sh
	   80     4036K     584K     225K     216K  /system/bin/sdcard
	  122     2760K     212K     212K     212K  /sbin/v86d
	   65      708K     268K     182K     100K  /sbin/ueventd
	  155     1544K     556K     172K     160K  /system/bin/installd
	  149     1648K     540K     165K     152K  /system/bin/debuggerd
	  157     1196K     152K     148K     148K  /system/xbin/su
	  142     1420K     484K     129K     120K  /system/bin/vinput
	   79     1308K     432K     120K     112K  /system/bin/logwrapper
	  139     1464K     432K     120K     112K  /system/bin/servicemanager
	  143     1404K     408K     112K     104K  /system/bin/vinput_seamless
	  145     1404K     380K     111K     104K  /system/bin/local_gps
	  147     4880K     384K     109K     100K  /system/bin/local_camera
	  146     4876K     384K     109K     100K  /system/bin/local_camera
	                           ------   ------  ------
	                          194338K  135056K  TOTAL
	
	RAM: 2068996K total, 1713928K free, 12292K buffers, 178908K cached, 4172K shmem,
	 20824K slab

通过`adb shell procrank`指令可以获取VSS，RSS，USS，PSS

- VSS – Virtual Set Size 虚拟耗用内存（包含共享库占用的内存）
- RSS – Resident Set Size 实际使用物理内存（包含共享库占用的内存）
- PSS – Proportional Set Size 实际使用的物理内存（比例分配共享库占用的内存）
- USS – Unique Set Size 进程独自占用的物理内存（不包含共享库占用的内存）

一般来说内存占用大小有如下规律：VSS >= RSS >= PSS >= USS

其中USS只能通过`procrank`获取，首先网上下载`libpagemap.so`, `procmem`, `procrank`，然后push到android手机中。有的root机自带这几个文件，genymotion机器也自带，不需要额外下载。


## 通过ADT插件DDMS查看用内存MAT进行分析 ##

利用DDMS的Heap可以很方便的查看app的内存占用情况，在app运行时，打开DDMS选项，在Devices下，可以看到正在运行的App，选择要查看内存的App，点击该条目，并选择Update Heap。

在Heap中，选择Cause GC，可以查看应用的内存占用情况。


参考资料： 百度移动云测试中心公众号