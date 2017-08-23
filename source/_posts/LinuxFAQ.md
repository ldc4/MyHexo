title: 学习Linux常见的简单问题
date: 2016-06-01 22:49:03
categories:
- Linux
tags:
- faq
---

入门掉过的坑，插个标志！
1.如何在VMware中像装真机一样装系统
2.磁盘分区原理以及开机流程（多重引导）
2.VMware网络原理
3.CentOS虚拟机NAT方式无法上网解决方法

<!--more-->

## 如何在VMware中像装真机一样装系统

当我新建一个虚拟机的时候，载入ISO文件，它就会自动检测然后帮我简化安装了。
可我不想要界面。

我用的是VMware 12 pro

稍后安装操作系统

**弄出来一个空的虚拟机配置后，在”虚拟机->设置“里面，点开CD/DVD设备，选择”使用ISO映像文件“。就行了**

## 磁盘分区原理以及开机流程（多重引导）

磁盘的第一个扇区：**MBR**(master boot record)主引导分区  和  **分区表**

分区表 64bytes
磁盘默认的分区表仅能写入四组分区信息
称为主（Primary）或扩展（Extended）分区
分区的最小单位为柱面

由扩展分区继续切出来的分区，被称为逻辑分区

扩展分区最多只能有**一个**（操作系统的限制）

**BIOS->MBR(安装有Boot Loader)->加载内核文件或者转到另一个Boot Loader（安装在启动扇区）**

## VMware网络原理

**bridged** - **NAT** - **host-only**

vmnet0 - 虚拟交换机

在桥接模式下，可以手工配置它的TCP/IP配置信息（IP、子网掩码等，而且还要和宿主机器处于同一网段）， 以实现通过局域网的网关或路由器访问互联网；还可以将IP地址和DNS设置成“自动获取”。

使用NAT模式，就是让虚拟机借助NAT（网络地址转换）功能，通过宿主机器所在的网络来访问公网。
NAT模式下的虚拟机的TCP/IP配置信息是由VMnet8虚拟网络的DHCP服务器提供的

在host-only模式中，虚拟机只能与虚拟机、主机互访，但虚拟机和外部的网络是被隔离开的，也就是不能上Internet。在host-only模式下，虚拟系统的TCP/IP配置信息（如IP地址、网关地址、DNS服务器等），都是由VMnet1虚拟网络的DHCP服务器来动态分配的。

图很容易理解

参考资料：
http://www.cnblogs.com/rainman/archive/2013/05/06/3063925.html

## CentOS虚拟机NAT方式无法上网解决方法
```
vim /etc/sysconfig/network-scripts/ifcfg-eth0

ONBOOT=yes
```
改完后重启一下网络服务
`service network restart`

我傻傻的去重启系统了。。

重启后能ping了！ifconfig也能看到eth0了

参考资料：
http://blog.sina.com.cn/s/blog_55b497690101fgxi.html
