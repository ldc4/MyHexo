---
title: React学习总结
categories:
  - React
tags:
  - base
date: 2017-03-19 19:49:06
---

背景：
之前从ng1，转战到了ng2，唯一对React的了解，就是阮神写的一篇React入门实例教程（[当时写的笔记](https://ldc4.github.io/2016/06/22/ReactStart/)）。
过完年，回到公司，就开始学习React，并参与项目。
这一个多月下来，也没抽出时间来总结，感觉忘得也差不多了，趁着还没有忘光，还是总结一下。


<!-- more -->

## 学习路径

不知道当时在哪找的这么一篇文章[React的学习路径](http://www.igeekbar.com/igeekbar/post/110.htm?utm_source=tuicool&utm_medium=referral)

大致如下：

基础
1.React
2.npm（npm还用得着学？全民都会）
3.Javascript打包工具（webpack，我都没有去官网看过，npm start | npm run build）
4.ES6（还没系统看过，凭借着以往的耳濡目染）
5.路由
6.Flux

进阶
1.内联样式
2.服务端渲染
3.Immutable.js
4.Relay,Falcor等

说实话，对于开发一个项目而言：保证可用，很多东西都可以不用了解细节，会用就行。
但是不懂原理，在排错、高性能中简直就是痛不欲生。
虽然，我也才只是达到会用，甚至谈不上熟练工。上面的道理还是懂的。

所以说，上面的学习路径仅仅是针对于真正想学好而言的。
事实上，在项目开发过程中，就必须忽略一些东西，围绕你的项目的技术栈学习。
**不谈技术栈的学习路径都是耍流氓**

目前，我们的项目前端使用的技术栈是：[React](https://facebook.github.io/react/) + [ant.design](https://ant.design/index-cn) + [dva](https://github.com/dvajs/dva)
其实，我们本来用的angular，看中蚂蚁金服的这套设计规范，其对应的angular实现在阿里内部，并没有开源出来。

## 真 · 学习路径

之前leader给的一份指引，我再结合自己情况，整理而得：

### [react官网](https://facebook.github.io/react/)

[QUICK START](https://facebook.github.io/react/docs/hello-world.html)（耗时一天）
[Tutorial](https://facebook.github.io/react/tutorial/tutorial.html)（一会儿）


### [React Router](https://reacttraining.com/react-router/)

新版本：
a.[V4.0.0](https://reacttraining.com/react-router/)
老版本：
b.[V3英文文档](https://github.com/ReactTraining/react-router/tree/v3/docs)
c.[V2.8.1英文文档](https://github.com/ReactTraining/react-router/tree/v2.8.1/docs)
辅助阅读：
d.[React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html?utm_source=tool.lu) (一个小时)
e.[React Router 中文文档](https://react-guide.github.io/react-router-cn/)

目前dva使用的是[react router 2.8.1](https://github.com/ReactTraining/react-router/tree/v2.8.1)

建议阅读顺序：d -> b/c（把基础看了即可，进阶等用到再来查阅）



### [Redux](https://github.com/reactjs/redux)

a.[Redux官方文档](http://redux.js.org/)

辅助阅读：
b.[Redux中文文档](http://cn.redux.js.org/)
c.[Flux 架构入门教程](http://www.ruanyifeng.com/blog/2016/01/flux.html)
d.[Redux 入门教程（一）：基本用法](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
e.[Redux 入门教程（二）：中间件与异步操作](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)
f.[Redux 入门教程（三）：React-Redux 的用法](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_three_react-redux.html)
（总共耗时半天）

建议阅读顺序：c -> d -> e -> f -> a


### [Redux-Saga](https://github.com/redux-saga/redux-saga)

a.[Redux-Saga官方文档](https://redux-saga.github.io/redux-saga/index.html)

辅助阅读：
b.[Redux-Saga简体中文文档](http://leonshi.com/redux-saga-in-chinese/index.html)
c.[Redux-Saga繁体中文文档](https://neighborhood999.github.io/redux-saga/)
d.[ES6 Generator](http://www.ruanyifeng.com/blog/2015/04/generator.html)

建议阅读顺序：d -> a


### [ant.design](https://ant.design/index-cn)

a.[规范，模式，组件](https://ant.design/docs/react/introduce-cn) 
b.[源码](https://github.com/ant-design/ant-design/)

可以git clone , npm install, npm start， 如果顺利跑起来，就等同本地版本的ant-design。

可以从中窥探官网实现思路。


### [dva](https://github.com/dvajs/dva)


a.[React+ant.design+dva项目实战](https://ant.design/docs/react/practical-projects-cn) (一个小时)

b.[8个概念](https://github.com/dvajs/dva/blob/master/docs/Concepts.md)

c.[12步30分钟，完成用户管理的CURD应用](https://github.com/sorrycc/blog/issues/18)

d.[知识地图](https://github.com/dvajs/dva-knowledgemap)

e.官方案例[DEMO](https://github.com/dvajs/dva/blob/master/README_zh-CN.md)，建议走读代码，理解代码组织及react + ant.design + dva 协同设计开发运行原理。

[Count](https://github.com/dvajs/dva/tree/master/examples/count)([jsfiddle](https://jsfiddle.net/puftw0ea/3/))，简单计数器

[User Dashboard](https://github.com/dvajs/dva-example-user-dashboard)，用户管理

[HackerNews](https://github.com/dvajs/dva-hackernews)([Demo](https://dvajs.github.io/dva-hackernews/))，HackerNews Clone

[antd-admin](https://github.com/zuiidea/antd-admin)([Demo](http://zuiidea.com/antd-admin/))，基于 antd 和 dva 的后台管理应用

[github-stars](https://github.com/sorrycc/github-stars)([Demo](http://sorrycc.github.io/github-stars/))，Github Star 管理应用


### 其他

[Create-React-App](https://github.com/facebookincubator/create-react-app)
[roadhog](https://github.com/sorrycc/roadhog)
[postgrest](https://github.com/begriffs/postgrest)
[PostgreSQL](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)
[fetch](https://github.com/github/fetch)
[科普文章：传统Ajax已死，Fetch永生](https://segmentfault.com/a/1190000003810652)


roadhog主要学习一下配置代理和MOCK数据
postgrest是基于PostgreSQL自动生成RESTful接口的web服务器
dva示例中就是用的fetch，貌似用ajax有点落伍了












































