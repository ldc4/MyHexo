title: Java日志系统初窥
date: 2016-05-15 20:08:23
categories:
- JAVA
tags:
- logging
---

我以前写java程序从不打印日志，全是syso。
正好在开发者头条上看到github引用最多的开源类库是slf4j，就想着学习学习java的日志系统。
看了一点点后，感觉贵圈真乱。

<!--more-->

http://my.oschina.net/xianggao/blog/515381

总结：

在看完第一个系列介绍后，觉得Java日志系统好复杂。

不能忘的东西有如下几点：
1.常见的实现日志和日志门面
jdk-logging(jul)、log4j、log4j2、logback
jcl(apache commons logging)、slf4j(simple logging facade for java)

2.日志级别和日志对象的创建

jul:FINEST - FINE - INFO - WARNING - SEVERE
logx:trace - debug - info - warn - error

jcl: Log logger = LogFactory.getLog(~)
slf4j: Logger logger = LoggerFactory.getLogger(~)

3.常见的日志系统集成方案

![](http://ldc4.qiniudn.com/images/JavaLoggingSystem/JavaLoggingSystem-1.png)

至于日志之间的转换，一张图就解决了：

![](http://ldc4.qiniudn.com/images/JavaLoggingSystem/JavaLoggingSystem-2.png)

原理什么的,了解就行。