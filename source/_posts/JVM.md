title: 深入理解Java虚拟机，知识点梳理
date: 2016-03-19 16:44:30
categories:
- JVM
tags:
- jvm
- openJDK
- AMM
---

对于这种性质（偏理论）的书，看的时候，能看懂，就是容易忘。
不过能在脑内留有大概印象。
想吐槽的是，书上的很多实践都无法完成。

<!--more-->
## 编译OpenJDK
### Windows下
```
1.获取JDK源码
     确定版本。   OpenJDK 6是从OpenJDK 7的某个基线引出的
     下载源码。   jdk7.java.net/source.html
2.看README-builds.html
3.构建编译环境
     安装Cygwin   在编译中要使用GNU Make来执行Makefile文件
     安装C++编译器    建议使用VS 2010以上   注意：VS的Path必须在Cygwin之前
     已经编译好的JDK   Bootstrap JDK   编译OpenJDK 7的话，Bootstrap JDK必须使用JDK6u14以上
     安装ANT   1.6.5以上
4.准备依赖项
     JDK Plug     ALT_JDK_IMPORT_PATH  JDK安装目录
     FreeType     ALT_FREETYPE_LIB_PATH   bin目录      ALT_FREETYPE_HEADERS_PATH include目录
     Microsoft DirectX 9.0 SDK     ALT_DXSDK_PATH DX安装目录
     MSVCR100.DLL     ALT_MSVCRNN_DLL_PATH  文件所在目录 
5.进行编译
     。。。
```
Cygwin找不到free 和 ar

### Linux下
我用的ubuntu kylin，在terminal执行：
```
sudo apt-get install build-essential gawk m4 libasound2-dev libcups2-dev libxrender-dev xorg-dev xutils-dev x11proto-print-dev binutils libmotif3 libmotif-dev openjdk-6-jdk ant
```
配置
```
export LANG=C
export ALT_BOOTDIR=/home/ldc4/jdk1.8.0_73

export ALLOW_DOWNLOADS=true

export HOTSPOT_BUILD_JOBS=8
export ALT_PARALLEL_COMPILE_JOBS=8

export SKIP_COMPARE_IMAGES=true

export USE_PRECOMPILED_HEADER=true

export BUILD_LANGTOOLS=true
#export BUILD_JAXP=false
#export BUILD_JAXWS=false
#export BUILD_CORBA=false
export BUILD_HOTSPOT=true
export BUILD_JDK=true

export BUILD_DEPLOY=false

export BUILD_INSTALL=false

export ALT_OUTOUTDIR=/home/ldc4/workspace/openjdk/build

unset JAVA_HOME
unset CLASSPATH

make 2>&1 | tee $ALT_OUTPUTDIR/build.log
```
遇到第一个错误（最开始我是自己装的JDK 1.8，ant）:
```
BUILD FAILED
/home/ldc4/openjdk/langtools/make/build.xml:452: The following error occurred while executing this line:
/home/ldc4/openjdk/langtools/make/build.xml:795: Compile failed; see the compiler error output for details.
```
网上没找到答案 ，有人说貌似jdk7,8都是老版本的xcode 大哥 我用的Linux

尝试性的试试apt-get install openjdk-6-jdk
果然 能跑了
但是 跑了一会 出现了第二个错误：

error: cannot find symbol

依赖目测出问题了

（虽然没有编译成功，但是了解到OpenJDK的存在，加上第一大章介绍的jvm的历史，虽然映象模糊了。。。而且这段还是在网吧前台看的）

## Java内存区域与内存溢出异常

### 运行时数据区域
    
     方法区
     堆

     虚拟机栈
     本地方法栈
     程序计数器

方法区、堆是线程共享的
虚拟机栈、本地方法栈、程序计数器是线程隔离的

**程序计数器**：

     可以看做是当前线程所执行的字节码的行号指示器
     分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖它来完成

**Java虚拟机栈**：

     生命周期与线程相同
     它描述的是Java方法执行的内存模型：每个方法在执行的同时都会创建一个栈帧
          用于存储  局部变量表、操作数栈、动态链接、方法出口

     局部变量表：各种基本数据类型、对象引用、returnAddress类型（指向了一条字节码指令的地址）
     
     如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常
     如果虚拟机栈可以动态扩展，如果扩展时无法申请到足够的内存，就会抛出OutOfMemoryError异常

**本地方法栈**：

     类似于虚拟机栈，一样会抛出那两种异常
	 它们之间的区别不过是虚拟机栈为虚拟机执行Java方法（也就是字节码）服务，
	 而本地方法栈则是虚拟机用到的Native方法服务。

**Java堆**：
     
	 被所有线程共享的一块内存区域，随虚拟机启动时创建
     
     唯一目的就是用来存放对象实例

     Java堆是垃圾收集器管理的主要区域，又被称为GC堆

     Java堆可以处于物理上不连续的内存空间，只要逻辑上是连续的即可
     
     如果在堆中没有内存完成实例分配，并且堆也无法再扩展，将会抛出OutOfMemeryError异常

**方法区**：

     用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码

     当方法区无法满足内存分配需求时，将抛出OutOfMemoryError异常

     运行时常量池是方法区的一部分


直接内存（DirectMemory）并不是虚拟机运行时数据区的一部分。
NIO可以使用Native函数库直接分配对外内存，然后通过一个存储在Java堆中DirectByteBuffer对象作为这块内存的引用进行操作。
这样可以避免了在Java堆和Native堆中来回复制数据。
受到本机总内存大小以及处理器寻址空间的限制，动态扩展时可能会出现OOM

### 对象的创建
     1.当虚拟机遇到一条new指令时，首先将去检查这个指令的参数是否能在常量池中定位到一个类的符号引用
     并且检查这个符号引用代表的类是否已被加载、解析和初始化过。

     2.在类加载检查通过后，虚拟机要给对象分配内存：
     
          - 指针碰撞  Serial、PerNew等带Compact过程的收集器
          - 空闲列表  CMS这种基于Mark-Sweep算法的收集器
     
     x.分配过程中的并发问题：
          采用CAS配上失败重试
          本地线程分配缓冲（TLAB）   -只有在TLAB用完并分配新的TLAB的时候，才需要同步锁定

          虚拟机是否使用TLAB，可以通过 -XX:+/-UseTLAB参数来设定

     3.内存分配完，初始化内存空间为零值。
          若是使用TLAB，可以提前至分配TLAB时初始化
          初始化这步操作保证了对象的的实例字段在Java代码中可以不用赋初始值就直接使用

     4.对对象进行必要的设置
          对象头
     
     5.最后执行<init>方法，把对象按照程序员的意愿进行初始化

### 对象的内存布局
     HotSpot虚拟机中，对象在内存中存储的布局可以分为3块区域：
          对象头、实例数据、对齐填充
     
     对象头：
          1.存储对象自身的运行时数据
               - hashcode
               - GC分代年龄
               - 锁状态标志
               - 线程持有的锁
               - 偏向线程ID
               - 偏向时间戳
               这部分数据的长度在32位和64位虚拟机中分别为32bit和64bit,官方称为“Mark Word”
          2.类型指针

     实例数据是对象真正存储的有效信息，也是在程序代码中所定义的各种类型的字段内容

     对齐填充，HotSpot VM的自动内存管理系统要求对象起始地址必须是8字节的整数倍


### 对象的访问定位

目前主流的两种：句柄和直接指针

### 内存溢出异常 OOM 实战
分析工具： Memory Analyzer
-XX:+HeapDumpOnOutOfMemoryError
MAT 分析 Dump 出的 .hprof文件


## 垃圾收集器与内存动态分配
针对的是java堆和方法区这部分内存

（对象已死么）
引用计数算法：不能解决相互引用的问题

可达性分析算法（用来判断对象是否存活）
     - GC Roots
          - 虚拟机栈（栈帧中的本地变量表）中引用的对象
          - 方法区中类静态属性引用的对象
          - 方法区中常量引用的对象
          - 本地方法栈中JIN（即一般说的Native方法）引用的对象
     - 引用链


"引用"分为
     - 强引用（Strong Reference）
          类似于 Object obj = new Object();
     - 软引用（Soft Reference）
          描述一些还有用但非必须的对象
     - 弱引用（Weak Reference）
     - 虚引用（Phantom Reference）

在可达性分析算法中不可达的对象，会先被GC标记一次并进行一次筛选，
筛选的条件是此对象是否有必要执行finalize()方法。覆盖finalize()方法并且没有被虚拟机调用过就是”有必要执行“
如果这个对象被判定为有必要执行finalize()方法，则把它放到F-Queue的队列。
稍后GC将对F-Queue中对象进行第二次小规模的标记


回收方法区（略）

### 垃圾收集算法
1.标记 - 清除算法 Mark-Sweep

      -不足：效率、空间问题：标记清除之后会产生大量不连续的内存碎片
      -后续收集算法都是基于这种思路并对其不足进行改进的

2.复制算法

     -解决效率问题
     -将可用内存划分为大小相等的两块，每次只用其中一块。
     -代价：内存缩水一半

3.标记 - 整理算法
4.分代收集算法

     -把java堆分为新生代和老年代
     -新生代选用复制
     -老年代选用标记-清理 或者 标记-整理

### HotSpot的算法实现
1.枚举根节点

     -Stop the world
     -当执行系统停顿下来后，虚拟机应当是有办法直接得知哪些地方存放着对象引用。这就得靠OopMap数据结构
     -OopMap：在OopMap的协助下，HotSpot可以快速且准确地完成GC Roots枚举

2.安全点
3.安全区域

### 垃圾收集器
-Serial收集器
-ParNew收集器
-Parallel Scavenge收集器

-Serial Old收集器
-Parallel Old收集器
-CMS收集器

-G1收集器

### 内存分配策略
场景：Serial/Serial Old收集器下的内存分配策略

1.对象优先在Eden分配
2.大对象直接进入老年代
3.长期存活的对象将进入老年代
4.动态对象年龄判定
5.空间分配担保


## 虚拟机性能监控与故障处理工具

Sun JDK 监控和故障处理工具
jps:  jvm process status tools
jsta: jvm statistics monitoring tools
jinfo: configuration info for java
jmap: memory map for java
jhat: jvm heap dump browser
jstack: stack trace for java


HSDIS: JIT生成代码反汇编

JDK的可视化工具  JConsole  VisualVM
