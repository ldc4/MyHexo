title: Linux常用命令
date: 2016-03-08 10:29:24
categories:
- Linux
tags:
- linux
- command
---
东西久了不用就会忘记，何况时间辣么长。
写给自己备用，保持更新。

<!--more-->

## 常用命令

``` bash
ls			显示文件或目录
	-l		列出文件详细信息l（list）
	-a		列出当前目录下所有文件及目录，包括隐藏的a（all）
	
cd			切换目录

mkdir		创建目录  
	-p		若无父目录，则创建p(parent)

touch		创建空文件

cp			复制

mv			移动或重命名

rm			删除
	-r		递归删除，可删除子目录及文件
	-f		强制删除

rmdir		删除空文件夹

cat			查看文件内容

more		分页显示文本文件内容

less		分页显示文本文件内容

head		显示文件头部内容

tail		显示文件尾部内容

grep		在文本文件中查找某个字符串

find		在文件系统中搜索某个文件

wc			统计文本中行数、字数、字符数

pwd			显示当前目录
```
## 系统管理命令
``` bash
stat		显示制定文件的详细信息，比ls更详细

who			显示在线登录用户

whoami		显示当前操作用户

hostname	显示主机名

uname		显示系统信息

top			动态显示当前消耗资源最多进程信息

ps			显示瞬间进程状态

du			查看目录大小
	-h		带有单位显示目录信息

df			查看磁盘大小
	-h		带有单位显示磁盘信息

ifconfig	查看网络配置

ping		测试网络连通

netstat		显示网络状态信息

clear		清屏

alias		别名，解除使用unaliax

kill		杀死进程

```
## 打包压缩相关命令
``` bash
gzip

bzip2

tar			打包压缩
	-c		创建新的归档文件
	-x		从归档文件中释放文件
	-t		列出归档文件的内容
	-z		具有gzip的属性
	-j		具有bzip2的属性
	-v		显示压缩或解压过程v(view)
	-f		使用档名

```
## 关机/重启机器
``` bash
shutdown
	-r		关机重启
	-h		关机不重启
	now		立刻关机
	
halt		关机

reboot		重启
```
## Linux管道
``` bash
使用“|”
将一个命令的标准输出作为另一个命令的标准输入。
例：cat 123.txt | grep haha
```
## Linux软件包管理
``` bash
rpm 			安装.rpm
	-i			在系统中安装软件i(install)
	-v			详细安装过程
	-h			用#(hash)符显示安装过程
	-e			在系统中卸载软件
	-U			在系统中升级软件

//yum基于RPM包管理工具
安装
yum -y install package-name
升级
yum update package-name
卸载
yum remove package-name
列出已安装RPM包
yum list
列出系统中可升级的所有软件
yum check-update

dpkg			安装.deb
	-i			在系统中安装/升级软件
	-r			在系统中卸载软件，不删除配置文件
	-P			在系统中卸载软件以及删除配置文件

//APT的全称为AdvancedPackaging Tools，与 YUM对应。
更新源索引
apt-get update
安装
apt-get install package-name
下载指定源文件
apt-get source package-name
升级所有软件
apt-get upgrade
卸载
apt-get remove [-purge] package-name

//Alien工具可以将RPM软件包和DEB软件包相互转换
alien -d package.rpm
dpkg -i package.deb

alien -r package.deb
rpm -ivh package.rpm

```
## 权限管理
``` bash
更改文件的用户及用户组
chown [-R] owner[:group] {File|Directory}

三种基本权限
R	读		数值表示为4
W	写		数值表示为2
X	可执行		数值表示为1

-rw-rw-r--一共十个字符，分成四段。权限用数值表示为664
第1个字符“-”表示普通文件；这个位置还可能会出现“l”链接；“d”表示目录
第2、3、4个字符“rw-”表示当前所属用户的权限。   
第5、6、7个字符“rw-”表示当前所属组的权限。     
第8、9、10个字符“r--”表示其他用户权限。

更改权限
chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名 

例：有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765
sudo chmod u+x g+w o+r  filename
上面的例子可以用数值表示
sudo chmod 765 filename
```