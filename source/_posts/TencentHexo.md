---
title: 腾讯环境下的Hexo移植
date: 2016-08-13 18:56:02
categories:
- Tencent
tags:
- hexo
- tencent
---

时隔一个多月，终于再次把Hexo博客搭好了。
本来很简单的一个事情，被hexo-qiniu-sync搞疯了。
浪费了不少时间。

这里记录一下遇到的问题。

<!-- more -->

## 吐槽
关于hexo-qiniu-sync插件，太让我失望了。
简直让我对这类自定义语法的插件失去信心。

当我从我的笔记本移植到我司的主机上，出现了如下错误：
![](http://ldc4.qiniudn.com/images/TencentHexo/TencentHexo-1.png)

ERROR,get file stat err？WTF？？？？

然后，我立即卸载这个插件。发现，我的文章里面的图片引用全是
```
{% qnimg xxxx %}
```

于是，只有费心费力的将所有页面手动的一个一个的替换掉。

还好我之前文件保存规范，不然就不是一下午的事情了。


## 移植过程遇到的问题

由于腾讯的网络环境问题，需要配置代理，而hexo本身没有代理。

解决方案：
1.hexo init的时候，当到达安装npm依赖的步骤，就可以ctrl-c了。
2.然后就可以使用npm加上代理参数或者tnpm来安装上hexo需要的依赖


![](http://ldc4.qiniudn.com/images/TencentHexo/TencentHexo-2.png)


另外，还有一个问题：在部署到github上时，出现如下错误

![](http://ldc4.qiniudn.com/images/TencentHexo/TencentHexo-3.png)

解决方案：
https://github.com/hexojs/hexo/issues/1495

终端出了些问题，选第二个选项，重装了一次git就好了。
