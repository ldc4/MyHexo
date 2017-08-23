---
title: AngularJS2起步
date: 2016-10-02 10:56:10
categories:
- AngularJS2
tags:
- ng2
- angular
---

国庆七天乐的第一天：
捣腾了一下ng2的工具链以及看了快速起步。
有中文文档就是好啊(泪流满面)。
这里，回顾一下昨天看的东西。

<!-- more -->

首先，接触到了新事物
{
	1.npm ls --global --depth 0
	2.powershell、node-gyp、plunker、polyfill/shim、
}

---

## 配置

快速起步
https://angular.cn/docs/ts/latest/quickstart.html

typescript配置
https://angular.cn/docs/ts/latest/guide/typescript-configuration.html

`tsconfig.json`（typescript编译器配置）
定义了typescript编译器如何从项目源码生成javascript代码
{
	noImplicitAny  设置是否隐式转换成any类型
	现在我还没有去看typescript基础语法，估计any就是弱类型var的意思。
	就算设置为true，也可以使用"suppressImplicitAnyIndexErrors":true 而不报错
}

`typings.json`（typescript类型声明配置）
为typescript编译器无法识别的库提供了额外的定义文件
{
	很多 JavaScript 库，比如 jQuery 、 Jasmine 测试库和 Angular ，会通过新的特性和语法来扩展 JavaScript 环境。 而 TypeScript 编译器并不能原生的识别它们。 当编译器不能识别时，它就会抛出一个错误。
	d.ts文件
	typings工具
	npm run typings install
	DefinitelyTyped仓库
	npm run typings -- install dt~jasmine --save --global
	这个--选项会告诉npm要把--右侧的所有参数都传给typings命令。
}

`systemjs.config.js`
为模块加载器提供了该到哪里查找应用模块的信息，并注册了所有必备的依赖包。 它还包括文档中后面的例子需要用到的包。
{
	类似于gulpfile.js，貌似这个是webpack的配置文件
	https://github.com/systemjs/systemjs/blob/master/docs/config-api.md
	这并不是webpack，而是另外一个轮子
}

总结：就4个配置文件而言的话，package.json就不用多说，剩下2个与Typescript有关的配置文件，还有一个与模块加载器有关的配置文件。
估计js就可以免去ts的两个配置文件了。

接下来就是 `npm install`

**在安装期间可能出现红色的错误信息，你还会看到 npm WARN 信息。不过不用担心，只要末尾处没有 npm ERR! 信息就算成功了。**

这个句话很关键啊。以前我都不知道什么才叫安装成功！

然后就直接踩坑了：timeout连接超时，typings install不行
然后km上一搜，还真有人踩坑：
http://www.baidu.com/group/20251/articles/show/255887?kmref=search&from_page=1&no=1&is_from_iso=1
https://github.com/typings/typings
代理换成dev oa那个

## 模块

每个Angular应用都至少有一个模块：根模块 AppModule

app.module.ts
```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
  imports:      [ BrowserModule ]
})
export class AppModule { }
```

`import { XxModule } from '@tencent/yyy';`

由于 QuickStart 是运行在浏览器中的 Web 应用，所以根模块需要从 `@angular/platform-browser` 中导入 `BrowserModule` 并添加到 `imports` 数组中。

## 组件

组件是 Angular 应用的基本构造块。每个组件都会通过与它相关的模板来控制屏幕上的一小块（视图）。

```
import { Component } from '@angular/core';
@Component({
    selector: 'my-app',
    template: '<h1>My First Angular App</h1>'
})
export class AppComponent { }
```

import 导入后，可以用@来使用
@xxx(里面是一个对象)

@Component 装饰器 ，它会把一份 元数据 关联到 AppComponent 组件类上：

selector 为用来代表该组件的 HTML 元素指定简单的 CSS 选择器。 即可以用<my-app></my-app>来引用组件

template 用来告诉 Angular 如何渲染该组件的视图。

 导出 这个 AppComponent 类，以便让AppModule导入它。

```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent }   from './app.component';
@NgModule({
  imports:      [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

## 启动器

```
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app.module';
const platform = platformBrowserDynamic();
platform.bootstrapModule(AppModule);
```

**为什么要分别创建 main.ts 、应用模块 和应用组件的文件呢？**
应用的引导过程与 创建模块或者 展现视图是相互独立的关注点。如果该组件不会试图运行整个应用，那么测试它就会更容易。

可以通过使用不同的引导器 (bootstraper) 来在不同的环境中启动 AppComponent

## 首页
```
<html>
<head>
    <title>Angular QuickStart</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="styles.css">
    <!-- 1. Load libraries -->
    <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
        System.import('app').catch(function(err){ console.error(err); });
    </script>
</head>
<!-- 3. Display the application -->
<body>
<my-app>Loading...</my-app>
</body>
</html>
```
初识了polyfill与shim

`npm start`，包含了`tsc && concurrently "npm run tsc:w" "npm run lite"`

tsc没猜错的话就是 typescript compiler

对应于package.json中的
```
"devDependencies": {
	"concurrently": "^2.2.0",
	"lite-server": "^2.2.2",
	"typescript": "^2.0.2",
	"typings":"^1.3.2"
}
```
TypeScript 编译器运行在监听模式。

一个名叫 lite-server 的静态文件服务器，它把 index.html 加载到浏览器中，并且在该应用中的文件发生变化时刷新浏览器。

---

<未完待续>




