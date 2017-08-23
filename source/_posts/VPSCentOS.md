title: 腾讯云VPS-CentOS
date: 2016-03-25 10:21:57
categories:
- VPS
tags:
- vps
- centos
- qcloud
---

之前网易笔试完了，看书也看不进去。就去把腾讯云的代金券拿来买了个VPS（CentOS 7.1）

<!--more-->

## 修改root密码
passwd 修改当前用户密码

## 创建一个用户
ldc4
useradd ldc4
不小心创建了一个ldc
useradd [-r] ldc
          |-连同用户的主文件夹也一起删除
发现空密码登录不进去
登录root去修改ldc4密码
#passwd ldc4

OK.
ssh 119.29.171.68 输入账号密码 登录成功

##注意事项：
sudo问题：用root在/etc/sudoers中添加 ldc4  ALL=(ALL) ALL

## 开始搭建环境

Centos 用yum来安装软件
sudo yum update 更新yum
yum list installed 查看已经安装的

### Java环境（JDK）
```
ping www.baidu.com 查看网络环境是否是好的。这里因为是腾讯云买的VPS，外网肯定是没问题的

yum list java*  查看有哪些java版本
 -y, --assumeyes
              Assume yes; assume that the answer to any question which would be asked is yes.
sudo yum -y install java-1.8.0-openjdk*
```
### MySql

找不到mysql-server!

##MariaDB mysql的一个开源分支

看官网文档！！

1.Adding the MySQL Yum Repository
```
下载rpm
shell> wget  http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm

shell> sudo rpm -Uvh mysql57-community-release-el7-7.noarch.rpm
```
2.Selecting a Release Series
```
shell> yum repolist enabled | grep mysql

去查看配置： /etc/yum.repos.d/mysql-community.repo

You should only enable subrepository for one release series at any time. When subrepositories for more than one release series are enabled, the latest series will be used by Yum.

修改enabled键值

```
3.Installing MySQL
```
shell> sudo yum install mysql-community-server
```
4.Starting the MySQL Server
```
shell> sudo service mysqld start

shell> sudo service mysqld status

//自动生成的密码
shell> sudo grep 'temporary password' /var/log/mysqld.log

shell> mysql -uroot -p

//修改mysql密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
mysql> mysqladmin -u root -p password 123456
```
2个都行，就是密码太简单会被嫌弃。
另外：5.7才是自动生成密码，5.6root密码为空
### Tomcat

1.安装
```
cd /usr/local

wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-9/v9.0.0.M4/bin/apache-tomcat-9.0.0.M4.tar.gz

sudo tar -zxf apache-tomcat-9.0.0.M4.tar.gz

mv apache-tomcat-9.0.0.M4 tomcat9
```
2.修改配置
```
cd tomcat9/conf

vim server.xml   ->改成80端口
```
3.启动Tomcat
```
./bin/startup.sh
```

[参考内容](http://blog.csdn.net/renfufei/article/details/9733367)


(重复造轮子，造啊造)