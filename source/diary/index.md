title: 日常随记
date: 2016-02-23 12:02:43
---
# 日常随记
## 2016年9月5日 - 2016年9月25日

交接davionli的自助项目

着手进行页面优化、完成新需求、修复bug

其间，还大致搞清楚了，habov2的业务接入全流程

---
## 2016年8月29日 - 2016年9月4日
接入即通视频云player sdk-统计事件

接入即通视频云player sdk-结束推流

HaboV2组件化

给即通视频云下的每个业务添加“总数”指标

HaboV2若干BUG修复

学习《编程的智慧》

---
## 2016年8月22日 - 2016年8月28日
接入即通视频云rtmp sdk-统计事件

接入即通视频云rtmp sdk-结束推流

接入即通视频云rtmp sdk-开始推流

接入即通视频云upload-统计事件

接入即通视频云upload-开始推流

解决HaboV2初始菜单定位

即通视频云分享按钮替换

学习《我们为什么需要组件化开发？》
学习《AngularJS中的Provider们：Service和Factory等的区别》

---
## 2016年8月15日 - 2016年8月21日
HaboV2 即通视频云添加天粒度

配置HaboV2 即通视频云接口

HaboV2时间范围选择器BUG修复

自助详细信息页面

---
## 2016年8月8日 - 2016年8月14日
HaboV2 QQ空间部分添加天粒度

深入理解js原型和闭包

简单了解叮咚使用和habo数据接入过程

---
## 2016年8月1日 - 2016年8月7日
学习《AngularJS学习总结详细教程》

完整分析angularjs1.5-components-showcase项目

HaboV2 H5返回码完善

API展示页面重构

habo日粒度数据接入学习

HaboV2分享功能第一版

---
## 2016年7月25日 - 2016年7月31日
本周任务：
1.Mini项目
2.HaboAPI展示页面重构和完善
3.HaboV2接入H5返回码业务

下周计划：
1.HaboV2的H5返回码部分上线
2.全面分析angular-components-showcase项目，并结合HaboV2思考如何重构
3.深入学习angularjs

总结：
Mini项目感触很深，做一件事情，先要评估自己的能力，到底有没有办法胜任这一职责，其次再是去努力把这件事做好。
也许是因为想挑战一下，然而失败了。好在后面和组员协商好，及时变更职责。
另外一个是对于API展示页面的感触，老是过多的把时间停留在不是很重要的细节上。

周常：（回顾这一周）
周一：
1.API展示页面流程理清楚了，和上周想的不一样。于是，按照Inspinia-Seed版本流程重新实现了一遍API展示页面。剩下一些样式问题，待解决。
2.晚上mini项目：找到qiniu上传base64图片解决方案
周二：
1.API展示页面的一些细节处理
2.学习如何上传到SVN上，以及预发布测试。
3.小组例会：哈勃大数据处理平台（用户自助接入）
周三：
1.Mnet账号申请，申请中
2.API展示页面发布达到svn上遇到一些问题：上线后找不到资源文件等
3.chimmy讲解HaboV2如何完成接入业务
4.晚上mini项目：使用apiDoc生成API文档，完成一部分注释
周四：
1.API页面完善
2.接到HaboV2新业务接入需求：H5返回码部分
3.根据第一版需求完成页面的曲线图部分，发现问题：域名太多，导致浏览器下拉框卡死。
4.晚上mini项目：apiDoc注释完成，架构设计文档完成
周五：
1.完成页面剩下的多维分析部分，找到多维分析指标按钮不默认选中的原因：uib-btn-radio里面的值需要单引号括起来
2.理了一遍控制器的逻辑，解决newRowData.forEach is not a function错误。
3.下午Mini项目：Mini项目答辩，获得铜奖
4.完成HaboV2的H5返回码业务接入，本地环境测试通过
周六：
1.Mnet账号申请完成
2.折腾hexo博客，由于hexo-qiniu-sync插件遇上了公司的网络环境原因，导致博客不能用。
3.查看一些angular博客
周日：
1.和mini项目小组的同学聚会，玩狼人杀。
2.分析了一会angular-components-showcase

---
## 2016年7月18日 - 2016年7月24日
本周任务：
1.Mini项目的开展
2.HaboAPI展示页面制作
3.分析angular-components-showcase项目

下周计划：
1.每天晚上继续Mini项目相关工作
2.从下周一，熟悉哈勃V2 WEB实现及业务接入方法，按哈勃V2新架构设计思路，重构哈勃V2实现。

总结：
Mini项目过程中遇到很多问题，包括自身职责定位不明确，前期技术选型把握力度有误。

周常：(回顾这一周)
周一：
1.angular conceptural overview章节补完
2.简单浏览angular guide directives
3.自助配置原型设计稿讨论会议
4.部分学习angular guide components
周二：
1.sng培训，mini项目组队
2.sng开发流程培训，小组讨论，确定产品。
3.接到API展示页面需求
4.看了第一遍angular guide components，理解不够深入
周三：
1.SNG培训：腾讯云、测试流程、编码规范
2.小组讨论，需求确定，初步分工，我负责后台开发，开发语言为GO
周四：
1.HaboAPI展示页面大体完成，还有部分细节问题。
2.chimmy给我和davionli讲哈勃V2未来规划，我接到任务：从下周一，熟悉哈勃V2 WEB实现及业务接入方法，按哈勃V2新架构设计思路，重构哈勃V2实现。
3.学习go环境搭建和go基础一部分
周五:
1.分析了angular-components-showcase项目，衍生分析showcase的Feature组件，发现问题，待跟进。
2.go语言学习进度没有预期的那么快，于是和另一个后台同学商量，转向运维，继续学习go语言，负责腾讯云，后期辅助后台同学开发
3.腾讯云SVN服务器搭建
4.晚上小组内开会，不在状态，浪费了时间。
周六：
1.学习go流程，函数，struct，面向对象，interface
2.学习一整天go，才刚把go基础看完，后面还有web部分，框架部分，发现时间不允许，再次和后台同学商量，后台部分交给他，我负责运维方面。
3.第一次使用腾讯云弹性公网IP对无公网IP的云主机进行网络调整
4.腾讯云主机上安装mongodb，配置负载均衡
周日：
1.浏览qiniu文档，解决qiniu文件上传问题。
2.给前端同学一个接口文档

---
## 2016年7月11日 - 2016年7月17日
本周任务：
1.AngularJS快速入门学习
2.熟悉Angular应用开发以及habo系统使用的INSOINIA模板。

下周计划：
1.参与项目开发，负责前台页面开发
2.mini项目准备

总结：
在学习过程中，遇到了很多坑，幸运的是目前可见的问题都解决了。但因此耗费了不少不必要的时间，原因多种多样，较多是因为自身的原因。
1.自身马虎没有找到文档的位置，就以为没有文档。
2.发现在写分享文档和总结的时候，动则2个小时以上，效率低，质量差，正在尝试调整。

日常：（回顾这一周）
周一：
1.简单了解了大数据的一些概念
2.参加小组头脑风暴会议
3.vis.js文档的dataset部分
4.接到chimmy给的《angularjs1.5web应用开发快速入门文档》
5.node.js、bower环境安装
6.查看数加和御膳房使用手册
周二：
1.遇到问题：tnpm找不到工作目录下的package.json
2.bower代理问题
3.参看《AngularJS v1.5 简明教程中文版》部分，遇到很多坑（原因在于翻译版本与git clone的代码不匹配），放弃继续阅读
周三：
1.了解到AngularJS是一个MVW框架
2.参看《angular tutorial官方文档》 0-3节
3.分析idataJS组件
4.参加哈勃自助平台产品功能流程设计，及第一个版本（为期一个月）版本目标讨论会议
5.尝试搭建INSPINIA模板种子项目，遇到环境问题，未解决
6.参看《angular tutorial官方文档》 4-6节
周四：
1.继续尝试解决NSPINIA模板种子项目环境问题，在一组同学张晋蔚的协助下，找到了问题所在并解决。
2.参看《angular tutorial官方文档》剩余部分
周五：
1.写了初学angular总结并分享
2.学习到：<meta http-equiv="X-UA-Compatible" content="IE=edge">的作用
3.制作INSPINIA模板种子项目绿色压缩包，并附上使用文档
4.尝试分析模板加载流程，以及FULL版本向SEED版本dashboard_1.html页面移植，出现问题，未解决
周六：
1.继续尝试解决移植页面问题，花费大量时间自行分析inspinia加载过程，并思考问题出在哪。通过自身的努力找出了问题在于ocLazyLoad
2.查看angular guide《introduction》 和 《conceptual overview》部分，巩固angluar的基本概念
3.之后，在INSPINIA模板文档中找到了angularjs版本的文档链接，学习并记录
4.dashboard页面部分移植，遗留问题：页面右下角的small chat显示有点不正常
周日：
1.解决掉small chat的显示问题，问题出在 slim-scroll JQuery插件，并产生了一个想法：
bower上找不到第三方插件,gulp构建的时候就不会自动添加`<script>`。而我解决问题是手动添加，不知道有没有更加优雅的办法
2.基于gulp serve命令执行时，控制台老是出现controller-as-vm的一个提示，要求把this改成vm，在网上找到了JohnPapa的Angularjs编程风格，先mark，之后再参看
3.基于我的学习经历，向chimmy的文档中添加一个注意事项
4.gulp的学习使用

每周一悟：用时间堆出来的印象才是最深刻的
（虽然在解决问题中浪费的时间较多，但是每一步都是自己独立思考分析，可能偶尔方向不太对，你的所作所为，在大脑中潜意识的累积加深印象，正如我在写这份周报的时候，印象最深刻的就是花时间最久的）

---
## 2016年7月8日 - 2016年7月10日
由于第一次出远门，周3考完试，周4就飞到深圳，周5就报道，熟悉这边的环境，花了不少时间。

日常：
周五：报道，认识导师和leader，配置电脑环境，了解部门业务，熟悉工作环境，成立5人小组，分析产品模型
周六：激活token，初识KM，通过《初窥SNG运维体系》了解SNG整体的运维体系架构
周日：参看部分vis.js文档，部分时间花在购买日常生活用品

感知：
1.学校的环境和公司的环境很不一样，公司有很多好资源，外网是看不到的。
2.同期实习生，都很厉害，要向他们学习。
3.这两天学习效率很低，对于一个新的事物，往往不能最快找到最合适的方式去迎合。需要反思反思。也希望各位前辈、同学，不吝赐教。
4.各种突发事件导致Todo-list没有落实，说明自己的时间安排能力，自身能力估量不到位。需要反思反思。

吐槽：
腾大食堂比万利达食堂便宜好吃

---
## 2016年7月12日
入门angular.js

遇到的各种坑

---
## 2016年7月11日
vis.js

阿里御膳房使用手册

头脑风暴会议

AngularJS入门

---
## 2016年7月10日
vis.js

大沙河公园宜家家居

周报

---
## 2016年7月9日
SNG运维体系结构

---
## 2016年7月8日
性能测试应该怎么做？
http://coolshell.cn/articles/17381.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io

java后端工程师技能树

实习报道

装电脑、配环境、熟悉业务

成立5人小组

web2.0、阿里御膳房、vij.js

---
## 2016年7月7日
与苹苹和汤圆一起到江北机场

厦门航空 到深圳

政哥儿和悠悠

宝安区 中煦·香槟山

豪方花园

莫泰酒店

---
## 2016年7月6日
架构师成长之路
http://mp.weixin.qq.com/s?__biz=MjM5NzAwNDI4Mg==&mid=2652190561&idx=1&sn=afdee086a6a7cf212f8f1ece01fdae79&scene=4#wechat_redirect

搬寝室。

交电费。

逃课群。

数学建模考试。

明天，准备走了~ fight

---
## 2016年7月5日
洗衣服，洗机械键盘

python HTMLParser

python urllib

写给自己，梳理一下我现在对前端知识结构的理解
https://segmentfault.com/a/1190000005875954?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly

---
## 2016年7月4日
寝室篇之遥控板

python collections

python base64

python hashlib

python itertools

python XML

---
## 2016年7月3日
起得最晚的一次，睡得最舒服的一次

看着拆热水器

python datetime

翻了翻数模，不太想看。。。

github乱逛了一圈

---
## 2016年7月2日
EditThisCookie

python正则表达式

gundum简史

看了一下近两年的新番列表

---
## 2016年7月1日
toutiao:码农与英语

python多进程

python进程与线程

找到一起下山的车友

---
## 2016年6月30日
MVC-MVP-MVVM...

信息对抗考试资料准备

信息安全考试

电话套餐咨询，无果

学习了微信公众投票原理

---
## 2016年6月29日
文档测试收尾

我以前学过的资源回顾

每天还是得逛逛头条、CSDN、开源中国、伯乐在线、Github才行啊

python IO编程

---
## 2016年6月28日
出发东西清单、观影录

银行账单

信息安全工程大作业告一段落

MVC一些新的认识

Python异常、调试、测试

---
## 2016年6月27日
魔鬼训练营前4章已看完

写渗透测试报告，发现偏题了。

写了一半。

---
## 2016年6月26日
openvas postgresql

metasploit魔鬼训练营 第四章一半

---
## 2016年6月25日
渗透测试魔鬼训练营 第三章

---
## 2016年6月24日
第一次睡到快10点

看了一会Python。

惊天魔盗团2 
Now u see me 2

python面向对象高级编程

---
## 2016年6月23日
啊哈哈哈哈。信息对抗也开卷

Kali装了3次才装好

魔鬼训练营第二章 P56

游了个泳

python 面向对象高级编程 看了一点

---
## 2016年6月22日
React博文

漏洞披露方式与漏洞信息库

Metasploit的历史与发展

临独沉疯

Metasploit下载安装

Kali安装了一半

---
## 2016年6月21日
阮一峰的react入门实例

重拾web安全233

浏览了一下知道创宇技能表
http://blog.knownsec.com/Knownsec_RD_Checklist/index.html

Metasploit渗透测试魔鬼训练营 P8

---
## 2016年6月20日
上课，逃，回顾这个月。

三国杀了一会。

python面向对象编程

李鑫想吃火锅，4人行吃了最后一次的火锅

特别崇拜这个人
http://www.bilibili.com/video/av4941338/

为什么要自己写框架
http://developer.51cto.com/art/201606/512536.htm

学习编程的加速度
http://mp.weixin.qq.com/s?__biz=MzAxOTc0NzExNg==&mid=2665513163&idx=1&sn=d2389e1b1f311cdfda125a3ae32829c2#rd

---
## 2016年6月19日
python高阶函数 
map/reduce filter sorted

python返回函数、匿名函数、装饰器、偏函数

python模块

---
## 2016年6月18日
python 汉诺塔

翻了翻以前看的动漫清单

下山拿了箱子，背包，茶叶，鞋子上来

喜满堂

游泳，吃饭，睡了一会

python 高级特性 over

---
## 2016年6月17日
python list

tuple、条件、循环、dict、set
python基础over

回顾一下之前看的python,并写个博文

python 调用函数

游了个泳

python 定义函数、函数参数

---
## 2016年6月16日
早上8点后：
去张羿家，吃饭，三国杀，狼人

认识几个新朋友

回校，上课，逃课，奶茶，回寝

晚上情绪开始有点波动了。
:晚上9点前

吃着西瓜听着oped看着代码

python list
---
## 2016年6月15日
选课

食品安全心得体会

余罪。。。

hello python

食品安全考试

python基础：
数据类型和变量
字符串和编码

---
## 2016年6月14日
信息安全课大作业布置

海贼已最新

理了发

与阿宅的美好回忆^v^

---
## 2016年6月13日
第4个月最后一周

shell基础语法看完

解决hexo的一个小问题

shell中各种括号的使用

食品安全考试的资料准备

火影已最新=-=

简要搜索了一下python资源

---
## 2016年6月12日
分答与值乎3.0

hello,shell!

shell变量、字符串、数组、注释

shell传递参数

付费社交、逻辑思维

排序动态过程
http://jsdo.it/norahiko/oxIy/fullscreen

shell基本运算符

shell echo printf test

shell 流程控制，看到一半

---
## 2016年6月11日
鸟哥差不多就看到这里了，还剩3章无关紧要。
到时候再看，接下来...容我想想

环顾了一下我的磁盘

吉他堂之从零开始
http://www.jitatang.com/tag/ling

蓝山工作室聚餐

---
## 2016年6月10日
鸟哥第22章完

海贼王最新

RPM

---
## 2016年6月9日
上午边睡边看，好久没有这样的情况了。
果然红牛的副作用体现了

中午回去补了近3个小时的觉

鸟哥Linux第21章

海贼王

鸟哥Linux第22章快看完了

---
## 2016年6月8日
三点就起床排队

看了几集火影

然后考科二，80分过了

中午回来，傻凯买票买错时间了

魔兽

0401205班聚

---
## 2016年6月7日
谁的青春不迷茫

科目二合场

---
## 2016年6月6日
惊天魔盗团在课上看了。。

学车归来：已经炉火纯青，获得场地老司机称号

今天没怎么看书，和阿翟聊天呢

---
## 2016年6月5日
剩下那点Grub看完了

看了一会HeadFirst

学车：快要成为老司机了

迭代器模式与组合模式

---
## 2016年6月4日
Linux启动流程分析

学车：1/6的时间利用率

内核与内核模块

Grub还有一点没看完

---
## 2016年6月3日
看了一会鸟哥

然后学车

下山看电影：X战警

带某凯吃了豆花

回来写点心得就到这个点了

算得上放了一天假=-=

---
## 2016年6月2日
Linux更新时间:ntpdate

鸟哥第19章看完

模板方法模式

Linux启动流程

---
## 2016年6月1日
daemon: stand_alone 与 super daemon

vim行号，复制多行

linux网络配置

学车：正倒

第18章 认识系统服务看完

---
## 2016年5月31日
5月底

上课归来，有一次作业没交，5分一次。。。

SELinux看完了

VMware网络连接

装一晚上的CentOS

---
## 2016年5月30日
这是第14周，第4个月过去1/3了

上了一节课

上午SELinux开了个头

下午学车，倒车入库一部分

晚上上了一节课，看了会HF一点点，效率有点低

对象适配器与外观模式

---
## 2016年5月29日
凌晨+尘埃（wee hours + dust）= weedust

进程的管理另一半

学车：侧方位练习

适配器模式开了个头

和TEG的大师兄聊了聊

---
## 2016年5月28日
Google site:v2ex.com/t 运维

学车

工作控制

进程管理一半

---
## 2016年5月27日
命令模式看一半

古香缘火锅（曦、凯、弈*2、翔、峰*2、茂）

下午在场地开了10来圈车，曲线和直角差不多会了。

命令模式看完。

简书、轻单

预约8号科目二

运维运营运维运营

---
## 2016年5月26日
at

Mark任宇昕

crontab

anacron

设计模式并没有看成

帮建兵解决了一下java/mysql环境问题

---
## 2016年5月25日
fdisk -l

硬盘的概念图

直接把虚拟机玩死了

LVM

脚本语言比较

---
## 2016年5月24日
腾讯offer签字确认回复

OCView使用到的技术

想个英文名真难：rixzhao，暂定它吧

RAID5，mdadm

LVM理论

---
## 2016年5月23日
技术运营offer拿到

了解技术运营，发现还有很多分类

江边烧烤

---
## 2016年5月22日
把githug整理成博文，其实也就复制粘贴

东搞搞西逛逛，下午2点了。

单例模式

智慧树食品安全食品

java并发编程实战开始看了

---
## 2016年5月21日
菜鸟网络

蛤蛤，excited...

抽象工厂

想把md文件放在gh上，想了一下还是算了

想整理一下导航分类，但是无从下手

githug小游戏完结，突然觉得git好那个啥

三个月过去

---
## 2016年5月20日
githug 21-30关，git感觉好难用啊。。。

HeadFirst工厂方法

蹭火锅吃233

---
## 2016年5月19日
发现一只我邮的野生大神
http://www.magicalwolf.com/

githug 14-20关：git mv、git log、git tag/git push --tags、git commit --amend --date

杂七杂八的浏览些关于技术运营的东西。

OCView维护

装饰者模式

---
## 2016年5月18日
安装ruby,使用gem，安装githug

githug第1-13关

腾讯深圳总部HR给我打电话了...

HeadFirst观察者模式

spring aop aop aop...

---
## 2016年5月17日
spring概述、spring bean、bean 作用域、bean 生命周期、bean的继承

BeanPostProcessor后置处理器

依赖注入的两种方式

注入集合

自动装配

基于注解的配置

java监听器原理

---
## 2016年5月16日
HeadFirst第一章，讲的策略模式

JUnit初窥

---
## 2016年5月15日
不断学习

java日志系统初窥

HeadFirst看了几页，有种说不出的感觉

---
## 2016年5月14日
清醒清醒，开始学习啦

晚期（运行期）优化
深入JVM第一遍过完了，实践感觉很少，发现根本就干不了个什么。

信息隐藏作业

HeadFirst设计模式开头

---
## 2016年5月13日
JVM 早期（编译器）优化

整个人的状态感觉怪怪的

---
## 2016年5月12日
默默的滚回重邮了

一下午也不知道干了个啥就过去了

写了下计划

JVM剩下的两章，开了个头。javac源码

---
## 2016年5月11日
虽然没有懵逼了，但是结果却很不理想

这是我人生的第二个低谷，愿我不甘现状，好好调整状态，越战越勇。

---
## 2016年5月10日
HTTP

再到成都

心情突然变好了，看开了。

找到一个面经刷了刷。

---
## 2016年5月9日
JMM与线程看完

线程安全与锁优化看完

OCView添加首页链接

---
## 2016年5月8日
字节码实战

JMM一部分

---
## 2016年5月7日
QQ空间Live预约，QQ空间也要开始做直播了么？

/etc/sudoers

调度

Linux进程调度

同步异步阻塞非阻塞

---
## 2016年5月6日
可靠传输的工作原理

TCP报文段的首部格式

TCP可靠传输的实现

中午睡了2个小时，做了一个胆战心惊的梦

TCP流量控制

一致性哈希

第四章网站性能优化看完

---
## 2016年5月5日
刷面经：

类加载过程

HashMap

进程间通信

JVM运行时内存分区

收集器如何GC

平衡树是二叉排序树的一种特例

---
## 2016年5月4日
MyISAM和InnoDB的区别和选择

Google hosts

MySQL索引原理

求重复字符的最大次数

Web前端性能优化

刷面试题

---
## 2016年5月3日
镜架坏了，换镜架

大型网站技术架构 网站性能测试

红黑树java实现

---
## 2016年5月2日
花了点时间浏览了一下上周的日记

XMind

java的多态

红黑树的概念

---
## 2016年5月1日
剑指offer 第4章 23,24,25,26,27,28

---
## 2016年4月30日
调优案例分析看完了

看了会吉他入门视频

剑指offer 面试题19,20,21,22

---
## 2016年4月29日
synchronized原理，总结，并写篇博文

什么是全文检索

react nativ

Leetcode 44 未解决

JVM 监控工具

jvm 高性能硬件上的程序部署策略

---
## 2016年4月28日
把昨天线下看的内容在笔记和代码上补上
剑指offer和大型网站技术架构

Leetcode 133

阿里巴巴测评

大型网站5大核心架构要素

synchronized与lock的区别

---
## 2016年4月27日
剑指offer 面试题13

大型网站技术架构 第一节

剑指offer 面试题14

大型网站技术架构 第二节

---
## 2016年4月26日
奇舞周刊
http://old.75team.com/weekly/

第8章 虚拟机字节码执行引擎

微信红包算法

---
## 2016年4月25日
重庆腾讯一面

懵逼的一天

比萨

java后端书架作者博客
http://calvin1978.blogcn.com/

回复上面作者的人的博客的书单
http://www.bysocket.com/?page_id=712

微信红包随机金额算法

---
## 2016年4月24日
Java编程思想 NIO部分

突然收到重庆腾讯面试，状态变成初始了。

打印简历，明天继续奋战

LeetCode 187

---
## 2016年4月23日
元数据：关于数据的数据

jvm 加载 连接（验证，准备，解析）

JVM类加载机制看完

瞎逛逛，获知LOL LPL总决赛 SNG对战EDG

阿里笔试题的思考。

淘生词http://readdoc.net，样式有点糟糕，想法不错

---
## 2016年4月22日
食品安全与日常饮食第7章

阿里笔试第一题，一下午就写了个将文件分块。。

Java编程思想I/O前小半部分

JVM第7章类加载机制开始看

---
## 2016年4月21日
早起，坐高铁回来了。
话说，高铁和动车有什么区别？

写HR面试经历

QCON 多盟

HDFS

---
## 2016年4月20日
HR面试

下午准备回去的，结果没有带够现金，而且半小时前就能不能买高铁票了，10分钟前就不能买人工票了，虽然支付宝说向银行卡转账需要2小时，结果几分钟就到了。

阿里笔试.炸，每每笔试都炸了。

---
## 2016年4月19日
技术运营一面

技术运营二面

Uniqlo

鲜芋仙

HR面试准备

---
## 2016年4月18日
懵逼的一天

改简历

---
## 2016年4月17日
早上早起，去了人和大厦，发现地点是错的。
然后到了，华西坝。学弟学妹的旅馆。

leetcode 54 re

中午睡了2个小时。。。

AJAX,好久没写过了。

下午没看个什么，就这样过来了。

晚上吃了川西坝子火锅。

Uber和滴滴，作为新用户，滴滴第一印象好些。

原谅我这几天水得很，因为在外面的效率真的是太低太低了。

---
## 2016年4月16日
早上来看了会操作系统的知识。

下午准备去成都

已到成都，寄宿在奎哥儿这里

---
## 2016年4月15日
nowcoder看了一会面经。

你真的会二分查找吗？
http://blog.csdn.net/int64ago/article/details/7425727/

查找算法

TCP三次握手四次挥手

Java线程

Mysql数据库引擎Innodb和MyISAM

TopK算法

---
## 2016年4月14日
AngularJS、MVVM杂七杂八的

项目介绍

排序算法手写

基数排序

TCP握手与挥手，挥手有点懵

---
## 2016年4月13日
spring ioc aop原理

买了16号到成都的动车票，17号霸面

http://blog.csdn.net/kingofworld/article/details/17718587
jvm内存模型与垃圾回收算法

面试整理之必须熟练的知识

classweb和ldc4_web项目介绍

---
## 2016年4月12日
修改了下简历，改了一下阿里的在线简历。

jvm类文件结构看完了。

实习僧，投投投

Leetcode

剑指offer面试题11，面试题12

---
## 2016年4月11日
起得晚，东西放在图书馆就去上课了，上课在看衣服。

jvm字节码指令

剑指offer第二章看完

---
## 2016年4月10日
手机没有电，起得晚。

C++的析构函数

剑指offer面试需要的基础知识（数据结构）完成

学车，离合器。。。

coding!

---
## 2016年4月9日
京东笔试第一题，京东笔试感觉就是考逻辑，靠流程。

浏览器同源策略

突然发现生活中好多东西要去学习，比如我想弹吉他，衣服搭配，还有学车

剑指Offer中，今天效率不高。因为，我又把皇室战争下下来了，护眼。

嘿咻嘿咻，那妹纸不见了TT

---
## 2016年4月8日
有点迷

No189 Rotate Array 2种解法

看了会开发者头条

由于剑指offer起包名引发了一系列事情

京东笔试

---
## 2016年4月7日
似乎想明白了一些事情，但是又不知道自己想明白了什么。

继续昨天那题

找到SOF问题所在

知道问题所在，但是还是解决不了。。算了，先别想这个了。

JNI javah demo 过程

收到JD笔试通知了

http://acm.acmcoder.com/listChineseproblem.php

TreeMap API

---
## 2016年4月6日
时间过得好快。。。

翻了翻剑指offer，刷了刷nowcoder.

OCView修改后台图片错误的资源路径

一个微软笔试题，迷宫问题，一直SOF。

---
## 2016年4月5日
sql基本用法

知道了CASS是什么东西，以及过了一遍基本用法

sql其他用法

腾讯笔试题最后一道sql题，其实挺简单的。

把字节码文件的常量池翻译了一遍。

字节码的文件结构。只能说，好复杂！

投了eleme

---
## 2016年4月4日
今天起得挺早的

笔试题整理，还剩腾讯的笔试。

整理完毕，剩一个sql

发现首页的一个bug，遂修改之

---
## 2016年4月3日
外面的小笼包好贵啊，好吧，我怂了。

因为手机坏了，看了一下iphone se。

面试经历之javase部分，看完了，并挨个学习了。NIO与JNI不懂，一个字，学。

腾讯笔试炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸炸

---
## 2016年4月2日
Memcached安装

socket/socketserver

JVM垃圾回收算法，内存模型

蘑菇街2面面经

js完全不会了啊~

手机坏了，手机坏了，手机坏了。。。

面试问题j2se部分

---
## 2016年4月1日
阶段性跳跃，fighting

LeetCode 65 Rotate List

大数据与云计算，Memcache与Redis 简单介绍

TopK算法

Navication页面，当沉入到一个东西当中，时间过得真的很快

B3log maven install

编程之美，写得好简陋啊。脑袋不够大

---
## 2016年3月31日
3月底了。

科目一 91分

Sao Utils整理快捷键

真的摸

JDBC事务、并发控制

网易笔试目测走远

---
## 2016年3月30日
http://www.race604.com/java-double-checked-singleton/
java单例真的写对了么？

http://blog.csdn.net/shuangde800/article/details/7341289
哈弗曼树

与翔哥聊了聊

CGLIB - code generation library

深入理解Hibernate持久化对象

---
## 2016年3月29日
数据结构中的查找算法

LeetCode 35 

http://www.ibm.com/developerworks/cn/java/j-lo-chinesecoding/
深入分析 Java 中的中文编码问题

360笔试题

肖申克的救赎

---
## 2016年3月28日
编程之美：50%cpu、滚动匹配字符串、手机9宫格按键：手机号码映射字典

Spring MVC 运行原理

XML解析方式与区别

http://88250.b3log.org/why-latke-exists
为什么又要造一个叫 Latke 的轮子

初识Maven核心概念jikexueyuan

---
## 2016年3月27日
自我介绍

java学习路线

http://elf8848.iteye.com/blog/875830
Spring MVC 教程,快速入门,深入分析

http://blog.csdn.net/lufeng20/article/details/7598801
SpringMVC 基于注解的Controller @RequestMapping @RequestParam..

360能力测评

重装CENTOS，OCVIEW和Home搭好了

---
## 2016年3月26日
centos下的mysql还是没有解决。。。先放一放吧，实习重要

GrayCode题在nowcoder做了

http://www.nowcoder.com/discuss/278
java的web开发常用框架

大端小端

数据类型占内存大小

http://developer.51cto.com/art/200908/141825.htm
什么是REST？以及RESTful的实现

---
## 2016年3月25日
FileZillaClient

上传OCview到CentOS7.1上

设计模式简单过了一遍

编程能力、架构能力、工程能力

目标全栈

鹅厂wo谈会

腾讯笔试模拟考。

---
## 2016年3月24日
在信息隐藏技术课上看了一下编程之美的序言。。

腾讯云买了个Linux主机

http://blog.csdn.net/yasi_xi/article/details/8373057
yum找不到包

在CentOS下装上了java mysql tomcat
竟然这么吃力，看来学习能力太弱了

---
## 2016年3月23日
8点正，眼睛有点涩涩的

工厂方法模式、抽象工厂模式、单例模式

http://www.cnblogs.com/aigongsi/archive/2012/04/01/2429166.html
java中volatile关键字的含义

http://www.xker.com/page/e2011/0422/101056.html
双重检查锁定

http://blog.csdn.net/findsafety/article/details/17993093
Java线程同步：synchronized锁住的是代码还是对象

http://www.blogjava.net/kenzhh/archive/2013/03/15/357824.html
Java：单例模式的七种写法

github的fork功能

fork了人生中的第一个项目233

http://www.open-open.com/lib/view/open1456194995870.html
Maven简单入门，尝试过程中出了个问题，应该是依赖问题，发现Maven并不好学啊

---
## 2016年3月22日
贪心

提莫蘑菇之扫描透镜

http://blog.jobbole.com/83944/
分治

网易练习题第二套

写了点人生！

fg与bg命令

网易正式笔试，做得很糟糕！

---
## 2016年3月21日
http://www.cnblogs.com/sdjl/articles/1274312.html
通过金矿模型介绍动态规划

数组分割

网易去年笔试的最后三题：dp、归并、文件和多线程

http://bsurl.cn/ngXnDz6mq3ajj
网易在线笔试调试

Nowcoder网易练习题

---
## 2016年3月20日
第三周最后一天。不知不觉一个月了。一年并没有想象中的那么长

如何计算算法复杂度

前缀、中缀、后缀表达式

Java IO最详解

http://blog.csdn.net/kavensu/article/details/8058155
网上随便找的笔试题： 2013网易技术类笔试题（java开发方向+移动平台开发）

去年网易笔试题1-16，还剩三道题

---
## 2016年3月19日
http://www.runoob.com/regexp/regexp-syntax.html
正则表达式

FreeMarker快速入门

收到网易笔试通知了（下周二）

Velocity快速入门

java虚拟机：自动内存管理机制(实战没看)

明天记得接着看Tcp/ip

---
## 2016年3月18日
top添加退出，不采用下拉菜单

前台退出功能

看电影去，免费的。2333

回来了~  11.00 - 6.00

解决js添加标签的样式问题\n

http://toutiao.com/i6263033729199899137/
深入理解Java之泛型

sb凯的小知识，猛然发现mysql，数据库这块忘完了

---
## 2016年3月17日
信息隐藏技术课，听不懂，就只记得什么细胞XXX，最后得到灰度图像。

Leetcode 101

Sturts2配置文件

次奥，我的简历上面有2个错误。。

在JD上填了简历

http://blog.csdn.net/elimago/article/details/3601579
struts2.i18n.encoding的较量

添加退出功能，修改“退出”的样式中

---
## 2016年3月16日
http://www.gbtags.com/gb/tutorials/220/1133.htm
AJAX

http://www.cnblogs.com/jsnewland/p/4724516.html
为什么要使用Freemarker

http://blog.csdn.net/sinat_26812289/article/details/50898693
java面试心得

又把排序算法看了一遍，既然要忘，我特码就多看几遍

发现一个野生的NexT.  实力high一下

虚拟机性能监控与故障处理工具
---
## 2016年3月15日
leetcode 278

给主页添加随机选择格言

http://www.cnblogs.com/mengdd/p/3447464.html
在GitHub上管理项目

为OCView重新安装一个新版本6.3->6.7

解决media页面index问题

突然发现我要把我干过的事情写三遍。。 印象笔记，日常随机，Hexo博客。是不是有点太过了。

https://www.zybuluo.com/mdeditor
在线Markdown编辑器，内置一些进阶markdown

git与github

---
## 2016年3月14日
<meta http-equiv="X-UA-Compatible" content="IE=edge">

“copy”了一个个人主页上去，低调有内涵。 23333

类的反射

选完课

0441303

回顾了一下JAVA 反射   泛型/枚举/注解

---
## 2016年3月13日
抱歉起来晚了

垃圾收集器与内存分配策略

想换个耳机的，然后买个小米手环，想想还是算了

把昨天用NexT主题遇到的问题记录了一下

看了一会Markdown进阶，但是Hexo貌似不支持

http://rapheal.iteye.com/blog/1526861
BFS
LeetCode 127解决，最后改得变成和别人一样了。。

---
## 2016年3月12日
内存溢出异常实战

java基础面试题，好多都是一看就知道，让我单独说的话说不出来。。

ModelDriven

Next主题

http://fontawesome.dashgame.com/

https://swiftype.com/

---
## 2016年3月11日
http://blog.csdn.net/niushuai666/article/details/7275406
vim简明教程

中午不太舒服，睡了一觉

到二教5楼

git push -u 的意义所在

http://velocity.oreilly.com.cn/2010/index.php?func=session&name=%E9%9D%99%E6%80%81%E7%BD%91%E9%A1%B5%E8%B5%84%E6%BA%90%E7%9A%84%E7%AE%A1%E7%90%86%E5%92%8C%E4%BC%98%E5%8C%96
静态网页资源的管理和优化
随便点着看了一下

---
## 2016年3月10日
日常打卡 7.30-10.40.。。

归并排序

把Hexo博客背景图片放到七牛

打卡终于结束，再也不用坐4小时车了

Kloudsec 免费提供https的CDN商

快速排序

---
## 2016年3月9日
安装hexo-qiniu-sync

三种简单排序算法：冒泡，简单选择，直接插入

给某凯做了一个动图

希尔排序、堆排序

---
## 2016年3月8日
http://www.cnblogs.com/laov/p/3541414.html
【Linux】Linux中常用操作命令

http://blog.csdn.net/ljq1203/article/details/7401616
软件包管理 rpm yum apt-get dpkg

http://www.cnblogs.com/li-hao/archive/2011/10/03/2198480.html
linux下使用tar命令

CNAME 玩脱了  我删了 为什么还是要跳转！！

chrome浏览器记住重定向了

http://www.importnew.com/10761.html
Java中如何克隆集合——ArrayList和HashSet深拷贝

但是Set<String>呢。。正在想。。

Leetcode 127 未果，我也就只能刷刷水题

对象的创建

---
## 2016年3月7日
leetcode NO7 Reverse Integer

jsp...

java运行时数据区域

弄了一晚上编译JDK

---
## 2016年3月6日
最后一天打卡

Servlet中持久化状态

下山堵车没赶上，白打了

编译Windows版的OpenJDK

卡在Cygwin了，一是有2个工具没找到，二是网速不行。

---
## 2016年3月5日
早上起来晚了，下山打卡人真多。
因为要领那个什么急救单子，就没有回学校。

在网咖看了一会书

Java和JVM的历史

Java技术的未来展望：模块化，混合语言，更多语法特性，64位

Servlet资源访问方式

http://www.cnblogs.com/leslies2/archive/2011/05/20/2051844.html
JAVA RMI远程方法调用简单实例

http://haolloyin.blog.51cto.com/1177454/332426
Java RMI 框架（远程方法调用）

---
## 2016年3月4日
日常下山打卡 还有3天

Servlet介绍

Servlet请求响应过程

下午打卡，然后和某凯看了场前天订的电影 叶问3

Servlet生命周期

阿里内推简历提交

---
## 2016年3月3日
下山打卡

leetcode 328

http://www.importnew.com/7010.html
HashMap和Hashtable的区别

HashMap的实现原理。。简单扫了一眼

花了一个半小时，又改了一下简历

---
## 2016年3月2日
驾校打卡 待了一天

网易内推、阿里内推、百度内推、腾讯内推。。

幕布寄到，寄快递

http://www.lintcode.com/
lintcode的邮箱发不过来
默默的上leetcode

简历上语言需要精炼简洁一下，时间顺序不要颠倒，自己掌握的技能最好单独列出来，这样一目了然，另外有想去的部门吗？

你学习一门技术的最佳时机是三年前，其次是现在。

http://www.w2bc.com/Article/84529
Java Web架构知识整理——记一次阿里面试经历

某凯拷了一些他以前面试时整理的东西

还是先看书吧。  第一二天就这么过去了。 博客还是没有动过，因为没有能写的。

---
## 2016年3月2日
JavaEE应用

报驾校

https://segmentfault.com/q/1010000000757213
linux中间有的命令是一个横杠，有的命令是两个横杠。请问这有什么区别吗？

http://blog.csdn.net/sunweizhong1024/article/details/8055400
GIT 的使用方法详解

http://blog.csdn.net/liuqiaoyu080512/article/details/8665394
commit 和 push 的临界点

---
## 2016年2月29日

手机已坏 刷机

如何写项目经验

---
## 2016年2月27日

腾讯云服务器 右边管理那里有个安全组 设置开放端口

Windows Server 2012 上安装wamp

http://download.csdn.net/detail/qq_31764923/9303545#comment
微软常用运行库合集64位_2015.11.zip

打扫一下午加一晚上清洁

---
## 2016年2月26日

MyEclipse里面设置多个Tomcat
http://bendan123812.iteye.com/blog/1716789

重新弄了个简历

腾讯云 云主机

119.29.33.176

折腾了一晚上也没解决，外网访问腾讯云的CVM

---
## 2016年2月25日

注定难熬的一年

---
## 2016年2月24日

poweroff..

goagent gogotester

devenv.exe

Cannot find one or more components.Please reinstall the application.

重新弄个VS-2013...

MinGW = Minimalist GNU for Windows

瞄了一眼以前写的C语言...

System.lineSeparator();报错，改成：
System.getProperty("line.separator")

多说.css 基本设置里面可以自定义

---
## 2016年2月23日

看了一哈 luuman 的github资源库。。

hexo new page ""

markdownpad 2

http://www.jianshu.com/p/9e5cd946696d
markdownpad 2.5注册码

---
## 2016年2月22日

多说

hexo本地  cannot get
-未果

gitcafe  coding

基于J2EE的网络问卷调查系统的设计与实现


hexo本地  cannot get
hexo本地  cannot get
hexo本地  cannot get！

---
## 2016年2月21日

http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门

http://www.jianshu.com/p/465830080ea9
HEXO+Github,搭建属于自己的博客

spfk主题

用Hexo写文章

Markdown

http://wowubuntu.com/markdown/#list
Markdown 语法说明 (简体中文版)

---
## 2016年2月20日

基于J2EE的网络调查问卷的设计与实现

Android 是么

迷茫

通过简历开始入手

乔布简历

www.king-liu.net

百度云

阿里云

腾讯云

凌渡辰风！！！

判断一件事情值不值得去做有一个方法：在一张白纸的左边写不值得做的原因，
然后在右边写值得做的原因，写完一比较，一权衡，自然能够得出结果。

GITHUB PAGES

---
## 起点。                         -ldc4,凌渡辰风,青松