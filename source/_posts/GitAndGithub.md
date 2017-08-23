title: Git与Github常用命令
date: 2016-03-15 20:52:10
categories:
- Git
tags:
- git
- github
---
git渐渐得心应手了，把这几天的积累小结一下。
现在只能写写水文;)
<!--more-->
## 在github上新建一个资源库

在本地新建一个文件夹
```
#mkdir dirname
```
初始化
```
#git init
```
编辑文件后，添加
```
#git add .
```
提交
```
#git commit -m "init"
```
设置远程端路径
```
#git remote add origin https://github.com/ldc4/xxx.git
```
想github服务端提交
```
#git push -u origin master
```
**这里需要注意，因为默认git init的是master分支，所有可以直接push到github上去，即使github上没有master分支**
**同理，如果是你在分支test下push到github写的不是test的话，是提交不上去的。github上没有分支的话，想要提交，本地分支名必须和提交（远端）分支名一样**

另外一点，参数-u：
用了参数-u之后，以后就可以直接用不带参数的git pull从之前push到的分支来pull。

>此时如果origin的master分支上有一些本地没有的提交,push会失败.  
>所以解决的办法是, 首先设定本地master的上游分支:  
>git branch --set-upstream-to=origin/master  
>然后pull:  
>git pull --rebase  
>最后再push:  
>git push  

## 在github上克隆一个资源库

切换到要克隆到的文件夹
```
#cd /home/ldc4/mygitdir
```
直接克隆,可以指定目录，也可以不指定目录
```
#git clone https://github.com/ldc4/xxx.git  yyy/xxx
```

## 在本地创建/删除一个分支

创建一个分支[基于master]
```
#git branch branch1 [master]
```
创建一个分支并切换到该分支下
```
#git checkout -b branch2
```
删除分支
```
git brach -d branch2
```

## 在本地切换/合并分支

切换到另一个分支
```
#git checkout branch1
```
在切换的过程中，文件夹中的内容会变成切换到的分支所维护的内容
git真是太人性化了。

合并分支
```
#git checkout master
先转换到主分支（合并到的分支）
#git merge --no-ff branch1
```
参数意义：
不用参数的默认情况下，是执行快进式合并。
使用参数--no-ff，会执行正常合并，在master分支上生成一个新节点。
merge的时候如果遇到冲突，就手动解决，然后重新add，commit即可。


## 查看各种状态信息

```
#git status //查看git状态

#git checkout //查看当前分支与远端的哪个分支连着的

#git branch //查看所有本地分支

#git branch -a //查看所有本地与远程的分支

#git remote //查看定义了哪些远端地址
```


大致常用的就这么多，但是git远不止这些，checkout系列就够得看，等到时候要用到的时候再现学。