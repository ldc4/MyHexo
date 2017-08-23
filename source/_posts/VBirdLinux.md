title: 鸟哥Linux私房菜简记
date: 2016-05-26 16:26:19
categories:
- Linux
tags:
- vbird
---

因为实习岗位的原因，又拿起了以前的《鸟哥的Linux私房菜基础篇(第三版)》来看，
当时看到软件磁盘阵列就没看了，现在打算接着看，先把第一遍刷完，虽然前面好多都记不起了。
然后再重点看常用的。

<!--more-->

## 第16章 例行性工作（crontab）

### at
**/etc/init.d/atd**
在我的VPS中没有发现这个

**/var/spool/at/**
使用at这个命令来生成所要运行的工作，并将这个工作以文本文件的方式写入/var/spool/at/目录内，该工作便能等待atd这个服务的取用与执行

**/etc/at.allow与/etc/at.deny**
先寻找/etc/at.allow这个文件，写在这个文件中的用户才能使用at，没有在这个文件中的用户则不能使用at
如果/etc/at.allow不存在，就寻找/etc/at.deny这个文件，若写在这个at.deny的用户则不能使用at，而没有在这个at.deny文件中的用户就可以使用at
如果两个文件都不存在，那么只有root可以使用at这个命令

常用命令：
```
at now + 5 minutes
at> ...
(ctrl+d)
```

atq/atrm 是at -l/-d的alias

当使用at时会进入一个at shell的环境来让用户执行工作命令。
建议使用绝对路径来执行命令，比较不会有问题。

另外一个alias: at -b = batch
它会在CPU工作负载小于0.8的时候，才进行你所执行的工作任务
（我在运行的时候并不能加上时间参数）

### crontab
/etc/cron.allow与/etc/cron.deny
与at相似

/var/spool/cron/
当用户使用crontab这个命令来新建工作调度之后，该项工作就会被记录到/var/spool/cron/里面去了，而且是以账号来作为判别的。
例如：/var/spool/cron/root
不要直接编辑该文件，使用**crontab -e**

**/var/log/cron**
另外，cron执行的每一项工作都会被记录到/var/log/cron这个日志文件

**/etc/crontab**
这个是系统的配置文件
里面的run-parts  /etc/cron.hourly是指的运行cron.hourly目录下的所有任务

周和日、月不可同时并存

### anacron
anacron
关机了怎么办，执行不了crontab了，这时就需要使用anacron了

当然，并不需要我们手动运行，centos开机期间运行

---

## 第17章 程序管理与SELinux初探

### 进程

在Linux系统中：触发任何一个事件，系统都会将它定义称为一个进程，并且给予这个进程一个ID，称为PID，同时依据触发这个进程的用户与相关属性关系，给予这个PID一组有效的权限设置。

系统就是通过这个PID来判断该Process是否具有权限进行工作

进程衍生出来的其他进程在一般状态下，也会沿用这个进程的相关权限。如：（在bash中执行touch）

程序（program）：通常以二进制程序放置在存储媒介中（如硬盘、光盘、软盘、磁带等），以物理文件的形式存在
进程（process）：程序被触发后，执行者的权限与属性、程序的程序代码与所需数据等都会被加载到内存中，操作系统并给予这个内存内的单元一个标识符（PID），可以说，进程就是一个正在运行中的程序

1.子进程与父进程
PPID（Parent PID）

2.过程调用的流程：fork and exec

3.系统或网络服务：常驻在内存的程序
daemon

### 工作控制（job conrol）

无法以job control的方式由tty1的环境去管理tty2的bash

限制条件：
这些工作所触发的进程必须来自于你shell的子进程（只管理自己的bash）
前台：你可以控制与执行命令的这个环境被称为前台（foreground）
后台：可以自行运行的工作，你无法使用[ctrl]+c终止它，可使用bg/fg调用该工作
后台中”执行“的进程不能等待terminal/shell的输入（input）


直接将命令丢到后台中”执行“的&

2>&1  表示错误标准输出合并在一起

将目前的工作丢到后台中”暂停“：[ctrl]-z

查看目前的后台工作状态：jobs

+表示最近被放到后台的工作号码，-表示最近第二个被放置到后台中的工作号码

将后台工作拿到前台来处理：fg

让工作在后台下的状态变成运行中：bg

管理后台当中的工作：kill
（kill后面接的数字默认是PID，如果想要管理bash的工作控制，就得加上%数字了）
-1 重新读取一次配置文件，类似与reload
-9 立刻强制删除一个工作
-15 以一个正常的程序方式终止一项工作

kill -signal %jobnumber
kill -l

脱机管理：nohup

另外还有一个工具：screen

### 进程的查看

1.仅查看自己的bash相关程序：ps -l
各参数的意义到时候查阅：P516-517
2.查看系统所有进程：ps aux
3.动态查看进程的变化：top
4.pstree找到进程间的相关性

### 进程的管理

进程之间是可以相互控制的，通过给予该线程一个信号（signal）去告知该进程你想要让它做什么！
1-SIGHUP
2-SIGINT
9-SIGKILL
15-SIGTERM
17-SIGSTOP

kill与killall

### 进程的执行优先级
nice值为-20~19
普通用户只能设置0-19，而且只能调高nice值

nice与renice：
nice -n -5 vi         (nice值为-5)
renice 10 (PID) 

nice值会在父子线程中传递

### 系统资源的查看

free:查看内存使用情况
uname:查看系统与内核相关信息
uptime:查看系统启动时间和工作负载
netstat:跟踪网络
dmesg:分析内核产生的信息
vmstat:检测系统资源变化

### 特殊文件与程序

具有SUID/SGID权限的命令执行状态
/proc/*代表的意义
查询已打开文件或已执行程序打开的文件
fuser:通过文件（目录）找出正在使用该文件的程序
lsof:列出被进程所打开的文件名
pidof:找出某个正在执行的进程的PID

### SELinux

传统的文件权限与账号关系:自主访问控制(DAC)

以策略规则制定特定程序读取特定文件:强制访问控制(MAC)

ls -Z

Identify:role:type

身份识别：root / system_u / user_u
角色：system_r / object_r
类型：文件资源是type 主体程序是domain

getenforce 显示当前的SELinux模式（enforcing、permissive、disabled）
sestatus 更详细

启动SELinux：修改/etc/selinux/config的内容，然后重启

setenforce 0/1 
0：转成permissive宽容模式
1：转成enfocing强制模式

setenforce无法在Disabled的模式下进行模式的切换

这一部分没有实践

重设SELinux安全上下文
chcon [-R] [-t type] [-u user] [-r role] 文件
chcon --reference=范例文件 文件

系统默认的目录都有特殊的SELinux安全上下文

restorecon -[Rv] 文件或目录

设置错误的type很有可能是因为该文件由其他位置复制或移动过来所导致的

错误分析方法：se troublehoot 与 audit和audit2why

SELinux的策略与规则管理：
seinfo/sesearch/getsebool/setsebool/semanage fcontext

seinfo/sesearch找不到，需要安装setools-console

---

## 第18章 认识系统服务（daemons）

### daemon与service

daemon分类：
1.stand_alone:此daemon可以自行单独启动服务
响应快
2.super daemon:一个特殊的daemon来同一管理

signal-control与interval-control

服务与端口

http 80 ssh 22 ftp 21

通常distriution都会记录每一个daemon启动后所取得进程的PID并放在/var/run这个目录下

### daemon的启动脚本与运行方式

daemon相关配置文件：
/etc/init.d/*:启动脚本放置处
/etc/sysconfig/*:各服务的初始化环境配置文件
/etc/xinetd.conf,/etc/xinetd.d/*:super daemon配置文件
/etc/*：各服务各自的配置文件
/var/lib/*：各服务产生的数据库
/var/run/*：各服务的程序的PID记录处

stand alone的/etc/init.d/* 启动
这里面的脚本会去检测环境、查找配置文件、加载distribution提供的函数功能、判断环境是否可以运行此daemon等

service crond restart == /etc/init.d/crond restart

/sbin/service

super daemon的启动：先修改/etc/xinetd.d/下面的配置文件，然后再重新启动xinetd

而xinetd的默认配置文件：xinetd.conf

一pia拉参数，要用的时候书上看P559-561

### 服务的防火墙管理

任何以xinetd管理的服务都可以通过/etc/host.allow,/etc/host.deny来设置防火墙

配置文件语法：ALL/LOCAL/UNKNOWN/KNOWN

TCP Wrappers特殊功能（spawn与twist）

### 系统开启服务

查看系统开启的服务

netstat -tulp (找出目前系统开启的网络服务)
netstat -lnp(找出所有的有监听网络的服务)
service --status-all(查看所有的服务状态)

设置开机后立即启动启动服务的方法：

开机流程：“
1.打开计算机电源，开始读取BIOS并进行主机的自我检测
2.通过BIOS取得第一个可开机设备，读取主要开机区（MBR）取得启动装载程序
3.通过启动装载程序的设置，取得kernel并加载内存且检测系统硬件
4.内核主动调用init进程
5.init进程开始执行系统初始化（/etc/rc.d/rc.sysinit）
6.依据init的设置进行daemon start（/etc/rc.d/rc[0-6].d/*）
7.加载本机设置（/etc/rc.d/rc.local）
”

chkconfig:管理系统服务默认开机启动与否
chkconfig --list
chkconfig --level [0123456] [服务名称] [on|off]

ntsysv:一个有类图形化界面管理工具，Red Hat系统特有

chkconfig --add 增加一个服务名称给chkconfig来管理，该服务名称必须在/etc/init.d/中
--del 就是删除呗

自定义服务，可以在init.d中加入脚本：
脚本中包含：
```
# chkconfig: 35 80 70
# description: 描述信息
```
chkconfig: runlevels 启动顺序 停止顺序

## 第19章 认识与分析日志文件

### 日志文件

日志文件的重要性：
解决系统方面的错误
解决网络服务的问题
过往事件记事簿


常见的日志文件名：
/var/log/cron
/var/log/dmesg
/var/log/lastlog
/var/log/maillog或/var/log/mail/*
/var/log/messages
/var/log/secure
/var/log/wtmp, /var/log/faillog
/var/log/httpd/*,/var/log/news/*,/var/log/samba/*

日志文件相关服务
syslogd：主要登录系统与网络等服务的信息
klogd：主要登录内核产生的各项信息
logrotate(日志文件轮换)：自动化处理日志文件容量与更新的问题

### rsyslogd

ps aux | grep syslog
chkconfig --list syslog
我在我的Linux上找到的是rsyslogd，而不是syslogd

syslog的配置文件:/etc/syslog.conf
规定了什么服务的什么等级信息以及需要被记录在哪里

服务名称[.=!]信息等级  信息记录的文件名或设备或主机

服务名称：syslog本身有设置一些服务（man 3 syslog）

信息等级：info - notice - warning(warn) - err(error) - crit - alert - emerg(panic)

日志文件的安全性:通过chattr +a使得只能被增加而不能被删除

当你不小心手动改过日志文件后，例如/var/log/messages，你不小心用vi打开它，离开却执行：wq的参数，
那么该文件将不会再继续进行日志操作。
要让该日志文件可以继续写入，你只需要重启syslog即可

日志文件服务器：修改syslogd的启动配置文件，通常在/etc/sysconfig内

### 日志文件的轮替（logrotate）

logrotate的配置文件：
/etc/logrotate.conf
/etc/logrotate.d/*

可调用外部命令来进行额外的命令执行，这个设置需要与sharedscripts...endscript设置起使用才行。

### 分析日志文件

CentOS默认提供的logwatch

鸟哥自己编写的日志分析脚本


## 第20章 启动流程、模块管理与Loader

### Linux的启动流程分析

1.加载BIOS的硬件信息与进行自我测试，并依据设置取得第一个可启动的设备。
2.读取并执行第一个启动设备内MBR的boot Loader（即grub,spfdisk等程序）
3.依据boot loader的设置加载Kernel，Kernel会开始检测硬件与加载驱动程序
4.在硬件驱动成功后，Kernel会主动调用init进程，而init会取得run-level信息
5.init执行/etc/rc.d/rc.sysinit文件来准备软件执行的操作环境（如网络、时区等）
6.init执行run-level的各个服务的启动（script方式）
7.init执行/etc/rc.d/rc.local文件
8.init执行终端机模拟程序mingetty来启动login进程，最后就等待用户登录


由于内核模块（/lib/modules）放置到磁盘根目录内，要记得/lib不可以与/分别放在不同的分区。

虚拟文件系统（InitialRAM Disk）一般使用的文件名为/boot/initrd.但CentOS6.5使用的/boot/initramfs
它能够通过boot loader来加载到内存中，然后这个文件会被解压缩并且在内存当中仿真成一个根目录，且此仿真在内存当中的文件系统能够提供一个可执行的程序，通过该程序来加载启动过程中所最需要的内核模块，通常这些模块就是USB，RAID，LVM，SCSI等文件系统与磁盘接口的驱动程序。

cpio -ivcdu < initrd-2.x.xx-xx.elx

如果你的Linux是安装在IDE接口的磁盘上，并且使用默认的ext2/ext3文件系统，那么不需要initrd也能够顺利进入Linux的

接下来开始执行第一个程序/sbin/init

/sbin/init最主要的功能是准备软件执行的环境，包括系统的主机名、网络设备、语系处理、文件系统格式及其他服务的启动等。
所有操作都会通过init的配置文件：/etc/inittab来规划。inittab内还有一个很重要的设置选项，默认的run level(启动执行等级)

runlevel:

0-halt
1-single user mode
2-Multi-user,without NFS
3-Full multi-user mode
4-unused
5-X11
6-reboot


/etc/inittab语法：
[设置选项]:[run level]:[init的操作行为]:[命令选项]


init系统初始化流程（/etc/rc.d/rc.sysinit）
...


启动系统服务与相关启动配置文件（/etc/rc.d/rc N & /etc/sysconfig）


每个run level要执行的脚本放置在/etc/rcN.d -> /etc/rc.d/rcN.d/中，
主要是通过/etc/rc.d/rc这个命令来处理相关任务
找到/etc/rc5.d/K??*开头的文件，并进行“/etc/rc5.d/K??* stop”操作
找到/etc/rc5.d/S??*开头的文件，并进行“/etc/rc5.d/S??* start”操作
K表示kill,S表示start，??代表的数字表示执行顺序。

[KS]??*这些全是连接文件，chkconfig就是在管理这些连接文件

S99local -> /etc/rc.d/rc.local
用户自定义开机启动程序

启动终端

设备与模块对应文件：/etc/modprobe.conf

run level的切换
init [0-6]

查看当前run level:直接用runlevel命令
N 3
左边代表前一个runlevel，右边代表目前的runlevel

### 内核与内核模块

内核模块的放置处是在/lib/modules/$(uname -r)/kernel当中
内核模块的依赖性：/lib/modules/$(uname -r)/modules.dep这个文件

depmod [-Ane]

内核模块的查看
lsmod:查看目前内核加载的所有模块
modinfo：查阅某个模块的信息
内核模块的加载与删除
modprobe：先分析依赖再加载模块
insmod:完全由用户自行加载一个完整文件名的模块，并不会主动分析模块依赖性
rmmod:删除模块
建议使用modprobe

modprobe [-lcfr] module_name
例如：
modprobe cifs
modprobe -r cifs

内核模块的额外参数设置：/etc/modprobe.conf


### Boot Loader：Grub

boot loader有2个stage(阶段)

stage1:执行boot loader主程序
stage2:主程序加载配置文件

配置文件都在/boot下
与grub有关的文件放置在/boot/grub中，最重要的就是配置文件（menu.lst）

硬盘与分区在grub中的代号：
1.硬盘代号以小括号（）括起来
2.硬盘以hd表示，后面会接一组数字
3.以“查找顺序”作为硬盘的编号，而不是依照硬盘扁平电缆的排序
4.第一个查找到的硬盘为0号，第二个为1号，以此类推
5.每块硬盘的第一个分区代号为0，依序类推


1.直接指定内核启动
root、kernel、initrd
2.利用chain loader的方式转交控制权
root、chainloader +1、makeactive
hide (hd0,4)可以对windows隐藏分区

如果有特殊需要所以要重制initrd文件的话，可以使用mkinitrd来处理

### 测试与安装Grub
1.如果是从其他boot loader转成grub时，得先使用grub-install安装grub配置文件
2.开始编辑menu.lst这个重要的配置文件
3.通过grub来将主程序安装到系统中，如MBR的(hd0)或者boot sector的(hd0,0)等

如果grub本身就发生错误，导致连grub都无法启动。我们可以是用具有grub启动的CD来启动，然后再以CD的grub的在线编辑，同样可以使用硬盘上面的内核文件来启动

在kernel参数中添加vga=844可以改分辨率，错了也无所谓，它会提示你查看，但是要注意提示的值是16进制，而你在配置文件中要写10进制。

BIOS至少可以读到大磁盘的1024柱面的数据

为菜单加密：grub-md5-crypt
将生产的MD5字符串，写入到grub.conf里面
password --md5 $***************
在要锁的菜单中加lock


### 启动过程的问题解决
1.忘记root密码
修改grub进入单用户维护模式：single
然后passwd就行

2.init配置文件错误
这样连单用户维护模式都进不去
就告诉内核不要执行init，改调用bash
init=/bin/bash(指定内核调用的第一个进程变成/bin/bash)
虽然可以利用root取得bash来工作，但此时除了根目录外，其他的目录都没有被挂载。
根目录被挂载成为只读状态。
mount -o remount,rw /
mount -a(参考/etc/fatab的内容重新挂载文件系统)

3.BIOS磁盘对应的问题
当你有2块硬盘的时候，一个装windows一个装Linux，需要解决grub对磁盘的设备代号问题
grub对磁盘的设备代号使用的是检测到的顺序，当调整了BIOS磁盘启动顺序后，menu.lst内的设备代号就可能会对应到错误的磁盘上

cat /boot/grub/device.map
(fd0)     /dev/fd0
(hd0)     /dev/hd0

如果不清楚如何处理的话，直接用grub-install --recheck /dev/hda1

因文件系统错误而无法启动fsck

利用chroot切换到另一块硬盘工作


## 第21章 系统设置工具（网络与打印机）与硬件检测

### CentOS系统设置工具：setup

输入setup命令后会有一个GUI来设置

1.用户身份验证设置
Authentication configuration

左边是用来配置账号服务器，右边是登录时需要提供的身份验证码(密码)所使用的机制
主要是修改/etc/sysconfig/authconfig

2.网络配置选项（手动设置IP与自动获取）

Name与Device名称最好相同
vim /etc/resolv.conf 可以设置DNS IP

3.防火墙配置

这个操作是新建/etc/sysconfig/iptables这个文件而已

4.系统服务的启动与否设置
ntsysv可以直接到这个界面

我的虚拟CentOS里面只有这4项

书上其他设置：
键盘形式设置
/etc/sysconfig/keyboard
系统时钟的时区设置
/etc/sysconfig/clock
/usr/share/zoneinfo
X窗口界面分辨率设置
/etc/X11/xorg.conf
/var/log/Xorg.0.log

### 利用CUPS设置Linux打印机

硬件支持度查询：
http://www.linuxfoundation.org/en/OpenPrinting

打印组件

用到的时候再看

### 硬件数据收集与驱动及lm_sensors

fdisk -l  将分区表列出来
hdparm 查看硬盘的信息与测试读写速度
dmesg 查看内核运行过程当中所显示的各项信息记录
vmstat 可分析系统（CPU/RAM/IO）目前的状态
lspci 列出整个PC系统的PCI接口设备
lspci -s 00:00.0 -vv (-s后面接的那个怪东西表示每个设备的总线、插槽与相关函数功能)
/usr/share/hwdata/pci.ids
/proc/bus/pci

lsusb 列出目前系统上面各个USB端口的状态与连接的USB设备
lsusb -t

iostat 可实时列出整个CPU与接口设备的I/O状态

usb = universal serial bus 通用串行总线

sensors-detect 生成lm_sensors文件

sensors 检测温度、电压等硬件参数

udev与HAL两者配合能够让你的设备PnP


## 第22章 软件安装：源码与Tarball

源码->编译->可执行文件

函数库

make 与 configure

所谓Tarball文件，就是将软件的所有源码文件先以tar打包，然后再以压缩技术来压缩
所以说，Tarball是一个软件包，你将其解压缩之后，可以得到：
1.源代码文件
2.检测程序文件（可能是configure或config等文件名）
3.本软件的简易说明与安装说明（INSTALL或README）

安装与升级有两种：
直接以源码通过编译来安装与升级
直接以编译好的二进制程序来安装与升级


gcc -c hello.c
gcc -O hello.c -c
gcc sin.c -lm -L/usr/lib -I/usr/include
gcc -o hello hello.c
gcc -o hello hello.c -Wall


makefile语法

目标（target）: 目标文件1 目标文件2
<tab>     命令（如 gcc -o hello hello.c）

make <target>

makefile也可以是用变量
1.变量与变量值以“=”隔开，同时两边可以具有空格
2.变量左边不可以有<tab>
3.变量与变量值在“=”两边不能具有“:”
4.习惯上变量名用大写
5.运用变量：${} 或 $()
6.在该shell的环境变量是可以被套用的
7.在命令行模式也可以定义变量

由于gcc在进行编译的行为时，会主动去读取CFLAGS这个环境变量，
所以你可以直接在shell定义出这个环境变量，也可以在makefile文件里面去定义，更可以在命令列当中定义

环境变量优先级：make命令行的>makefile文件里面的>shell原本具有的

$@：代表目前的目标（target）


源码管理软件步骤：
1../configure
2.make clean
3.make
4.make install

配置文件、函数库、可执行文件、在线帮助文档

patch更新源码
patch -pN < patch_file
这个N表示patch_file的路径拿掉几个斜线


函数库管理
静态函数库（.a）与动态函数库（.so）
ldconfig与/etc/ld.so.conf
程序动态函数库解析：ldd

检验软件的正确性：
md5sum与sha1sum

## 第23章 软件安装：RPM、SRPM与YUM功能

### 软件管理器介绍

RPM与Debian的dpkg

RPM的问题：
1.软件安装的环境必须与打包时的环境需求一致或相当
2.需要满足软件的依赖性需求
3.反安装时需要特别小心，最底层的软件不可先删除，否则可能造成整个系统的问题

SRPM = Source RPM
通常SRPM的扩展名是以***.src.rpm这种格式来命名的

软件名称-版本信息-发布版本次数-操作硬件平台（CPU型号）

RPM软件的属性依赖的解决方案：YUM在线升级


### RPM软件管理程序：rpm

/var/lib/rpm目录下的数据作用：
版本之间的比较，查询系统已经安装的软件
同时，目前的RPM也提供数字证书信息，这些数字证书也是在这个目录内记录

安装需要root身份

rpm -i xxx.rpm
rpm -ivh package_name
（package_name可以是本地文件，也可以是网络文件，可以多个同时安装）

一些强制参数：
--nodeps 不考虑依赖
--replacefiles 覆盖
--replacepkgs 覆盖
--force 强制（等于上面2个参数之和）
--test 测试安装，找出是否有属性依赖问题
--justdb
--nosignature 略过数字证书的检查
--prefix 指定新路径
--noscript 不让该软件在安装过程中自行执行某些系统命令

不推荐使用强制参数，一般用ivh就够了

RPM的升级与更新（upgrade/freshen）

-Uvh与-Fvh

RPM查询（query）

rpm -q...
rpm -qp... 
查询某个RPM文件，必须列出整个文件的完整文件名才行

RPM验证与数字证书（Verify/Signature）

rpm -V
输出代表的含义看书上P695

安装原厂发布的公钥文件：
locate GPG-KEY
rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
rpm -qa | grep pubkey
(这里看得有点懵)

卸载RPM与重建数据库（erase/rebuilddb）

rpm -e
删除的时候注意依赖关系

rpm --rebuilddb

### SRPM的使用：rpmbuild

--rebuild    编译与打包
--recompile  编译打包并安装

SRPM使用的路径：
我测试的时候是在/root/rpmbuild/目录下：
有SPECS/SOURCES/BUILD/BUILDROOT/RPMS/SRPMS

设置文件的主要内容（*.spec）
除了使用SRPM内默认的参数来进行编译之外，我们还可以修改这些参数后再重新编译。
具体文件格式看书上P701-702

rpmbuild -ba xxx.spec 编译并同时生成RPM与SRPM文件
rpmbuild -bb xxx.spec 编译成RPM文件


打包自己的软件：
把源文件打包，放到SOURCES下
然后新建*.spec的设置文件
最后rpmbuild -ba main.spec

### YUM在线升级机制

yum是通过分析RPM的标题数据后，根据各软件的相关性制作出属性依赖的解决方案，然后可以自动处理软件的依赖属性问题，以解决软件安装或删除与升级的问题。

由于distribution必须要先释出软件，然后将软件放置于yum服务器上面， 以供各户端来要求安装与升级只用。因此我们想要使用yum的功能时，必须要先找到适合的yum server才行

yum查询功能：

yum [list|info|search|provides] 参数

-y自动提供yes的响应
--installroot=/path 将该软件安装在/path中而不使用默认路径
search:搜索某个软件名称或者是描述的重要关键字
list:列出目前yum所管理的所有的软件名称与版本，类似于rpm -qa
info:同上,类似于rpm -qai
provides:从文件去搜索软件，类似与rpm -qf

yum安装/升级功能：install、update

yum删除功能：remove

一些关键字：updates/installed


yum的设置文件
/etc/yum.repos.d/CentOS-Base.repo

列出目前yum server所使用的容器有哪些：yum repolist all

yum会先下载容器的清单到本机的/var/cache/yum
yum clean all 可以清理本机上的缓存数据


yum软件组功能：
yum grouplist


凌晨三点全系统自动升级:
vim /etc/crontab
0 3 * * * root /usr/bin/yum -y update


至此，鸟哥这本书就告一段落了，接下来需要重点练习常用的

