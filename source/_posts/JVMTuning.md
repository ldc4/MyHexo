title: JVM之调优案例分析与实战
date: 2016-04-30 16:46:55
categories:
- JVM
tags:
- jvm
- tuning
---

这里是一个小白花了一下午的时间来玩JVM调优，第一次玩，很多地方弄不太懂。
昨天晚上到今天下午，正好一天左右的时间。
简要的笔记，用于回顾，学习内容全在《深入理解Java虚拟机》

<!--more-->

## 案例分析

### 高性能硬件上的程序部署策略

目前主要有2种方式：
1.通过64位JDK来使用大内存
2.使用若干个32位虚拟机建立逻辑集群来利用资源

对于第一种：
对于用户交互性强、对停顿时间敏感的系统，
可以给Java虚拟机分配超大堆的前提是有把握把应用程序的Full GC频率控制得足够低。

除了Full GC停顿长还可能遇到其他问题

对于第二种：
具体做法是在一台物理机上启动多个引用服务器进程，每个服务器进程分配不同的端口，然后在前端搭建一个负载均衡器，以反向代理的方式来分配访问请求。

无Session复制的亲和式集群。仅仅需要保障集群具备亲和性，也就是均衡器按一定的规则算法（一般根据SessionID分配）将一个固定的用户请求永远分配到固定的一个集群节点进行处理即可。

第二种方法也有其他的缺点。


### 集群间同步导致的内存溢出（JBossCache）
不具有参考价值。。

### 堆外内存导致的溢出错误

DirectMemory
-XX:MaxDirectMemorySzie
线程堆栈
-Xss
Socket缓存区：Receive和Send两个缓存区
JNI代码
虚拟机和GC也要消耗一定内存


### 外部命令导致系统缓慢

Runtime.getRuntime().exec()

### 服务器JVM进程崩溃（堆积的Socket连接过多）
不具有参考价值。。


### 不恰当的数据结构导致内存占用过大

HashMap<Long,Long> 空间利用率太低。

### 由Windows虚拟内存导致的长时间停顿（GUI程序最小化）
恢复页面文件的操作而导致不正常的GC停顿

-Dsun.awt.keepWorkingSetOnMinimize=true
不太具有参考价值。。



## MyEclipse10不能被VisualVM识别（未果）

jps查看不到MyEclipse运行的程序
找到MyEclipse.ini配置修改-vm
```
#utf8 (do not remove)
#utf8 (do not remove)
-startup
../Common/plugins/org.eclipse.equinox.launcher_1.2.0.v20110502.jar
--launcher.library
../Common/plugins/org.eclipse.equinox.launcher.i18n.win32.win32.x86_64_3.2.0.v201103301700
-install
E:/IDE/MyEclipse/MyEclipse 10
-vm
E:/Java/JDK7u21/bin/javaw.exe
-vmargs
-Xmx512m
-XX:MaxPermSize=256m
-XX:ReservedCodeCacheSize=64m
-Dosgi.nls.warnings=ignore
```

启动时出错。google一下，说是Win8兼容问题。

恩，果然！

jps还是监测不到，多写了个jre
E:/Java/JDK7u21/jre/bin/javaw.exe
↓
E:/Java/JDK7u21/bin/javaw.exe

依然监测不到，意识到这个javaw只是给MyEclipse用的。

运行程序是
<terminated> MyEclipseTest[Java Application] E:\Java\JRE\bin\javaw.exe (...)

试试用JRE\bin文件夹下的jps

jre只是运行环境，并没有jps

用任务管理器查看进程，发现VisualVM是E:/Java/JDK7u21/jre/bin/java.exe

VisualVM是用java.exe运行的，MyEclipse是用javaw.exe运行的。

然后又回到原点了。 先整理整理思绪

使用MyEclipse2016就能检查到，先放一放这个问题。

很有可能是64位 和 32位的关系。（猜想）

## MyEclipse2016 C2 调优参数

![](http://ldc4.qiniudn.com/images/JVMTuning/JVMTuning-1.png)

-Xverify:none
![](http://ldc4.qiniudn.com/images/JVMTuning/JVMTuning-2.png)

-Xms1024m
-Xmn500m
-XX:MetaspaceSize=128M
-XX:+DisableExplicitGC

-XX:+UseParNewGC
-XX:+UseConcMarkSweepGC

-XX:+PrintGCTimeStamps
-XX:+PrintGCDetails
-verbose:gc
-Xloggc:gc.log
![](http://ldc4.qiniudn.com/images/JVMTuning/JVMTuning-3.png)


关于java8的metaspace的问题：
[Java 8: 从永久代（PermGen）到元空间（Metaspace）](http://blog.csdn.net/zhyhang/article/details/17246223)

这里有另一个人的调优过程：
http://m.oschina.net/blog/305209

