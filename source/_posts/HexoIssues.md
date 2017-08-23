categories:
  - Hexo
tags:
  - hexo
  - spfk
  - Next
  - issues
date: 2016-06-13 15:04:16
title: Hexo主题功能改动以及使用遇到的问题
---

想起什么就改改，遇到什么问题就记录记录，持续更新。

<!--more-->

## 2016年6月13日15:02:34
今天写了一个博客，然后hexo g的时候，莫名其妙的报错。

大哥，我什么都没干啊。

起初以为是qiniu的问题，还去官网逛了逛

于是我把这篇博文md移开，发现没问题。

那问题就还是在博文里面了。

先看yaml的tags 和 categories,我还以shell是关键字之类的
然后改了一下标题，也没问题啊。

然后我就把文章从最后开始，一段一段的删，看看问题出在哪。
最后显示的是：
```
`length=${#array_name[@]}`
```
带不带反引号都不行，必须得用三个反引号。

### update
```
`style={{opacity: this.state.opacity}}`
```
这个也不行，两个大括号

---
## 2016年4月23日23:48:37
每次Hexo部署到github的时候，都有下面这个提示：
warning: LF will be replaced by CRLF in tags/transaction/index.html.
The file will have its original line endings in your working directory.

虽然不痛不痒，但是还是想解决掉。

网上说是git的原因： git config core.autocrlf false

然后运行git config --global core.autocrlf false解决了，
去看看有没有什么后遗症

并没有，算是解决。

有的东西不知道怎么下手，想明白问题，往往就一两步操作的事情。

---
## 2016年3月16日10:42:05
给文章添加阅读数
之前spfk主题似乎是用的多说的统计，由于配置隐藏细节了，我猜的。

NexT则是使用的LeanCloud，配置还算蛮简单的。

1. 你需要到[LeanCloud](https://leancloud.cn)注册一个账号
2. 然后创建一个应用
3. 然后在应用中添加Class：Counter
4. 找到应用的app_id，app_key，配置到NexT的配置文件中就行了

---
## 2016年3月13日17:09:39
昨天，看到一篇博文，该博文是用的Hexo博客，觉得它用的主题NexT很不错
就去看了一下，发现主题的使用文档很详细，虽然在我安装过程中遇到很多问题。。。
不过github上有issues搜搜查查也基本解决了。但是我觉得还是记录一下我之前耗费时间太多的配置主题NexT所遇到的一些问题。

**配置菜单Menu的图标以及标题**

menu对应于hexo new page "menu";
categories以及tags不能访问问题，使用文档中有，略过。
直接说重点：
### 菜单Menu的图标
Menu的图标使用的是FontAwesome，虽然文档中有提及但是没有给出Mapping在哪查询，对于像我这种小白就只能乱搜一通，然后尝试。

- [http://fontawesome.dashgame.com/](http://fontawesome.dashgame.com/)
- [http://fontawesome.io/icons/](http://fontawesome.io/icons/)

前一个是中文网，极风游科技翻译的吧。
后一个是官网。

### Menu的标题
直接配置的话，出来的字是 Menu.xxx.
这就需要你到languages文件夹下修改你选择相应的翻译文件，
如我选的简体中文zh-Hans.yml，
去里面配置一下就好了

	menu:
	  home: 首页
	  archives: 归档
	  categories: 分类
	  tags: 标签
	  about: 关于
	  search: 搜索
	  commonweal: 公益404
	  diary: 日记
	  sign: 签到

---
## 2016年3月10日13:41:48
因为装了hexo-qiniu-sync插件，我把主题的left-col的背景图片转移到了/static目录
这样可以利用七牛cdn来加快网页加载时间
翻看网页样式，发现是直接写在标签style里面的，那就是用JS修改的，去找JS文件，由于对spfk主题目录结构不太熟，找了好一会。
反正东找找西找找，js目录里面没看到，就想起是不是通过Hexo来生成的JS，没学过nodejs，最后在layout找到了themes\spfk\layout\_partial\backgound.ejs
原：
``` js
var backgroundimg = "url(backgroud/bg-x.jpg)".replace(/x/gi, Math.ceil(Math.random() * backgroundnum));
```
改：
``` js
var backgroundimg = "url(http://7naraf.com1.z0.glb.clouddn.com/images/backgroud/bg-x.jpg)".replace(/x/gi, Math.ceil(Math.random() * backgroundnum));
```
---

Start datetime : 2016-03-10 13:39:16