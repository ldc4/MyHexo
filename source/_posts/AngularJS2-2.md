---
title: AngularJS2辅助应用
date: 2016-10-04 10:03:49
categories:
- AngularJS2
tags:
- ng2
- angular
---

本以为可以一天结束战斗，结果硬生生的看了两天。
感觉有些概念还是没太搞懂。

<!-- more -->

**根据官网的辅助应用，把主题hero改成gumdam...**
**根据官网的辅助应用，把主题hero改成gumdam...**
**根据官网的辅助应用，把主题hero改成gumdam...**
重要事说三遍

## 第一步

### 定义一个实体

```
export class Gumdam {
    id: number; // 物理编号
    name: string;   // 名称
    description: string;    // 描述
}
```

### 在组件类中初始化

```
export class AppComponent {
    title = '我就是Gumdam';
    gumdam: Gumdam = {
        id: 1,
        name: 'ZGMF-X42S Destiny',
        description: '命运敢达'
    };
}
```

### 在模板中使用

```
template: '<h1>{{title}}</h1><h2>{{gumdam.name}} details!</h2>'
```

> 这里需要注意的就是，ts由于有数据类型/类的概念了。类型是以在变量后声明用冒号隔开。  
> 其他的没什么太大的变化，但是这里使用的话，是单向绑定（插值表达式）了。  
> 使用反引号可以输入多行字符串。

### 表单双向绑定

```
import { FormsModule } from '@angular/forms';
```

> 和angularjs1每太多区别，要导入FormsModule。  
> 双向绑定还是使用ngModule，只是写法有点诡异。。。

```
<input [(ngModel)]="gumdam.name" placeholder="name">
```

官方总结：
我们的《英雄指南》使用双大括号 ( 插值表达式——单向数据绑定的一种形式 ) 来显示应用的标题和 Hero 对象的属性。
我们使用 ES2015 的模板字符串写了一个多行模板，来让我们的模板更有可读性。
为了同时显示和修改英雄的名字，我们还使用了内置的 ngModel 指令，往 `<input>` 元素上添加了双向数据绑定。
通过 ngModel 指令，这些修改还影响到了每一个对 hero.name 的其它绑定。

## 第二步

### 列表数据

```
const GUMDAMS: Gumdam[] = [
    { id: 1, name: 'ZGMF-X42S Destiny', description: '命运高达' },
    { id: 2, name: 'ZGMF-X19A Infinite Justice', description: '无限正义高达' },
    { id: 3, name: 'ZGMF-X20A Strike Freedom', description: '强袭自由高达' },
    { id: 4, name: 'ZGMF-X10A Freedom', description: '自由高达' },
    { id: 5, name: 'GN-0000/7SG 00 Gundam Seven Sword/G', description: '七剑G型00高达' },
    { id: 6, name: 'GNT-0000 Qan[T] 00 Quanta Gundam', description: '量子型00高达' },
    { id: 7, name: 'GN-002 Gundam Dynames', description: '力天使高达' },
    { id: 8, name: 'GN-003 Gundam Kyrios', description: '主天使高达' },
    { id: 9, name: 'GN-005 Gundam Virtue', description: '德天使高达' },
    { id: 10, name: 'GN-001 Gundam Exia', description: '能天使高达' }
];
```

> 用const定义为常量，这类数据一般从接口中取。  

用官方的话说：先用一组 Mock( 模拟 ) 出来的数据

### 在组件类中创建一个属性公开出来

```
gumdams = GUMDAMS;
```

我们并不需要明确定义 gumdams 属性的数据类型， TypeScript 能从 GUMDAMS 数组中推断出来。

### 在模板中使用

```
<li *ngFor="let gumdam of gumdams">{{gumdam.name}}</li>
```

注意`let ... of ...`用法

官方说明：
ngFor 的 * 前缀表示 <li> 及其子元素组成了一个主控模板。
ngFor 指令在 AppComponent.heroes 属性返回的 heroes 数组上迭代，并输出此模板的实例。
引号中赋值给 ngFor 的那段文本表示“ 从 heroes 数组中取出每个英雄，存入一个局部的 hero 变量，并让它在相应的模板实例中可用 ”。
hero 前的 let 关键字表示 hero 是一个模板输入变量。 在模板中，我们可以引用这个变量来访问一位英雄的属性。

### 样式

```
styles: []
```

当我们把样式赋予一个组件时，它们的作用域将仅限于该组件。 
即：此样式只会作用于这个 AppComponent 组件，而不会“泄露”到外部 HTML 中。

### 选择列表项

绑定click事件

```
onSelected(gumdam: Gumdam):void{
    this.selectedGumdam = gumdam;
}
```

与

```
<li *ngFor="let gumdam of gumdams" (click)="onSelected(gumdam)">
```

> typescript中方法的定义返回值相当于属性的类型，也是通过冒号隔开  
> 这里可以看到click外面加了小括号。至此已经出现了3个特殊符号了，*/[]/()

另外值得一提的是：

```
onSelected2 = function( gumdam ){
    this.selectedGumdam = gumdam;
};
```
这样写也是能行的。。

详情模板:

```
<h2>{{selectedGumdam.name}}の机型说明!</h2>
<div><label>编号: </label>{{selectedGumdam.id}}</div>
<div>
    <label>机型: </label>
    <input [(ngModel)]="selectedGumdam.name" placeholder="name"/>
</div>
<div>
    <label>中文名: </label>
    <input value="{{selectedGumdam.description}}" placeholder="description">
</div>
```

直接运行会报错，因为selectedGumdam没有初始化。
这里官方给出一个官方的解决方案：
要处理这个问题，我们可以先让详情不要出现在 DOM 中
我们把模板中的“英雄详情”内容区用 div 包裹起来。然后往上添加一个 ngIf 内置指令，然后把 ngIf 的值设置为本组件的 selectedHero 属性。
ngIf 和 ngFor 被称为“结构型指令”，因为它们可以修改 DOM 的部分结构。 换句话说，它们让 Angular 在 DOM 中显示内容的方式结构化了。
要了解更多 ngIf ， ngFor 和其它结构型指令，请参阅： 结构型指令 和 模板语法 章节。

给选中的列表项添加样式：

```
<li *ngFor="let gumdam of gumdams" (click)="onSelected(gumdam)" [class.selected]="gumdam === selectedGumdam">
```

官方给出的解释是：
注意，模板中的 class.selected 是括在一对方括号中的。 这就是“属性绑定”的语法，一种从数据源 ( 即 hero === selectedHero 表达式 ) 到 class 属性的单向数据流。

> 那我是否能理解为[]是从数据源到（视图）属性，而()是从DOM事件到数据源。那么[(ngModel)]做为双向绑定就理解得通了。

官方总结：
我们的《英雄指南》现在显示一个可选英雄的列表
我们添加了选择英雄的能力，并且会显示这个英雄的详情
我们学会了如何在组件模板中使用内置的 ngIf 和 ngFor 指令

## 第三步

### 将详情页面组件化，单独分离出来

命名规范：
文件名用中划线隔开
每个文件能见名知意
类名需要驼峰加后缀
选择器用中划线隔开

由于详情页面也要用到Gumdam类，于是把Gumdam也单独提出来。

于是变成：
```
import { Component, Input } from '@angular/core';
import { Gumdam } from './gumdam';

@Component({
    selector: 'my-gumdam-detail',
    template:`
                <div *ngIf="gumdam">
                    <h2>{{gumdam.name}}の机型说明!</h2>
                    <div><label>编号: </label>{{gumdam.id}}</div>
                    <div>
                        <label>机型: </label>
                        <input [(ngModel)]="gumdam.name" placeholder="name"/>
                    </div>
                    <div>
                        <label>中文名: </label>
                        <input value="{{gumdam.description}}" placeholder="description">
                    </div>
                </div>
              `
})
export class GumdamDetailComponent {
    gumdam: Gumdam;
}
```
但是，Input还没用到，应该和输入有关。

终于看到有意思的一点了：
https://angular.cn/docs/ts/latest/guide/attribute-directives.html#!#why-input

这里突然来个Input属性，搞得太突兀了。
官方讲了这么多，也就重复提到一点就是：不能让组件使用者任意绑定其他的属性，从（源）到（目标)的绑定目标必须声明Input属性。
这里可能会理解得有点片面，但是突然3个名词就飚来：来源属性、目标属性、输入属性。但，应该八九不离十了。

### 在app.module中添加声明

```
import { GumdamDetailComponent } from './gumdam-detail.component';

@NgModule({
    imports:      [ BrowserModule, FormsModule ],
    declarations: [ AppComponent, GumdamDetailComponent],
    bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

### 在app.component模板中添加标签

```
<my-gumdam-detail [gumdam]="selectedGumdam"></my-gumdam-detail>
```

官方总结：
我们创建了一个可复用组件
我们学会了如何让一个组件接收输入
我们学会了在 Angular 模块中声明该应用所需的指令——只要把这些指令列在 NgModule 装饰器的 declarations 数组中就可以了。
我们学会了把父组件绑定到子组件。

## 第四步

### 创建提供数据的服务

新建一个文件gumdam.service.ts，注意命名规范。

```
import { Injectable } from '@angular/core';

@Injectable()
export class GumdamService {
    getGumdams():void {}
}
```

使用@injectable装饰器修饰，记得有个小括号。

创建一个空方法。取数据的方式有很多种。

### 创建Mock数据文件

新建一个文件mock-gumdam.ts，里面放测试数据

将app.components的数据移过去。

并在常量前面加上export

### 实现服务中的空方法

```
@Injectable()
export class GumdamService {
    getGumdams():Gumdam[] {
        return GUMDAMS;
    }
}
```

> 这里有个ws的技巧：不用去导入，直接在方法中使用，然后你选择它提供的下拉选项，就会自动导入。（alt+enter）

### 使用服务

先导入服务

```
import { GumdamService } from "./gumdam.service";
```

再使用服务

GumdamService是一个类，那就要new个实例出来？

```
gumdams = new GumdamService().getGumdams();
```

虽然这样是可以的。但是官方不推荐这样用，理由有如下：

我们的组件将不得不弄清楚该如何创建 HeroService 。 如果有一天我们修改了 HeroService 的构造函数，我们不得不找出创建过此服务的每一处代码，并修改它。 而给代码打补丁的行为容易导致错误，并增加了测试的负担。

我们每次使用 new 都会创建一个新的服务实例。 如果这个服务需要缓存英雄列表，并把这个缓存共享给别人呢？怎么办？ 没办法，做不到。

我们把 AppComponent 锁死在 HeroService 的一个特定实现中。 我们很难在别的场景中把它换成别的实现。 比如，能离线操作吗？能在测试时使用不同的模拟版本吗？这可不容易。

注入服务：
用这两行代码代替用 new 时的一行：
我们添加了一个构造函数，同时还定义了一个私有属性。
我们添加了组件的 providers 元数据

```
constructor(private gumdamService: GumdamService) { }

providers: [GumdamService]
```

刚一开始很不理解的是为什么，constructor中都有GumdamService类型了，你还不知道注入的是什么类型？
非得搞个提供者？

根据上面官方的理由，
搞出个“提供者”的目的，我猜是：为了达到解耦，而更高层次的抽象，就从一行代码冗余到了2行代码。
作何理解？providers填GumdamService就等同于直接new的操作，而你要花式实例GumdamService，你就自己去实现一个提供者，返回你花式实例。然后导入到要用服务的那个文件，然后填写在providers中，这样就达到了解耦。只需要修改提供者的那个问题。
另外，构造函数的作用，就是告诉在实例本组件的时候注入一个名为gumdamService的GumdamService类型的一个实例。
所以，根据有限的情报，我的理解是providers的GumdamService和constructor的GumdamService不是一个东西！
待看到后面的文档再回来佐证我初学时的理解。

### 生命周期 ngOnInit

```
export class AppComponent implements OnInit{
```

实现OnInit类的ngOnInit方法

### 异步服务与Promise

Promise，它就是一个承诺——在有了结果时，它承诺会回调我们。

```
getGumdams():Promise<Gumdam[]> {
    return Promise.resolve(GUMDAMS);
}
```

```
// 定义变量
gumdams: Gumdam[];
// 获取数据
getGumdams():void{
    this.gumdamService.getGumdams().then(gumdams => this.gumdams = gumdams);
}
```

回调中所用的 ES2015 箭头函数 能比等价的函数表达式更加快速、优雅的处理 this 指针。

官方总结：
我们创建了一个能被多个组件共享的服务类。
我们使用 ngOnInit 生命周期钩子，以便在 AppComponent 激活时获取英雄数据。
我们把 HeroService 定义为 AppComponent 的一个提供商。
我们创建了一个模拟的英雄数据，并把它导入我们的服务中。
我们把服务改造为返回承诺的，并让组件从承诺获取数据。

### 附件：慢一点

```
getGumdamsSlowly(): Promise<Gumdam[]> {
    return new Promise<Gumdam[]>(resolve =>
        setTimeout(resolve, 2000)) // delay 2 seconds
        .then(() => this.getGumdams());
}
```

Promise算是ES6的基础吧，为了赶进度，这一块先放一放。
留个疑问：如何还原同步等待2秒的场景

## 第五步

### 将列表页面从AppComponent中抽离出去

balabala..一大堆

单独把Gumdams的东西剥离出去。

把providers放到app.module下。gumdams.component.ts就不需要写providers了。

### 添加导航，设置路由

设置base标签
```
<base href="/">
```
路由配置
```
const appRoutes: Routes = [
    {
        path: 'heroes',
        component: GumdamsComponent
    }
];
```
appRoutes是常量名，Routes是Route[]类型。这个常量就是路由配置

光声明一个变量没有用，还得导出

```
export const routing: ModuleWithProviders = RouterModule.forRoot(appRoutes);
```

虽然这些都合情合理，但是又多出来了很多变量要记。这里一下就蹦出三个：Routes，RouterModule，ModuleWithProviders

而且这个ModuleWithProviders命名很匪夷所思。

路由插座（Outlet）

把 `<router-outlet>` 标签添加到模板的底部。 RouterOutlet 是 RouterModule 提供的 指令之一 。

```
<a routerLink="/gumdams">Gumdams</a>
<router-outlet></router-outlet>
```

于是router-outlet ， routerLink，各种新词汇

### 创建dashboard

新建dashboard.component.ts `->` app.module.ts添加声明 `->` 添加路由

### 重定向根路由

```
{
    path: '',
    redirectTo: '/dashboard',
    pathMatch: 'full'
},
```

### 填充dashboard内容

balabala,重复之前的一些步骤,无非就是准备数据，准备视图，配置路由。

```
@Component({
    selector: 'my-dashboard',
    templateUrl: 'app/dashboard.component.html'
})
export class DashboardComponent implements OnInit{
```

template `->` templateUrl

### 路由到详情页面

参数化路由
```
{
    path: 'detail/:id',
    component: GumdamDetailComponent
}
```
通过传递的id来获取详情信息

重构详情页面
```
constructor(
    private gumdamService: GumdamService,
    private route: ActivatedRoute,
    private location: Location
) {}
```
route : ActivatedRoute 当前活动状态的url

重构后的页面：
```
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params }   from '@angular/router';
import { Location }                 from '@angular/common';

import { GumdamService } from './gumdam.service';
import { Gumdam } from "./gumdam";

@Component({
    selector: 'my-gumdam-detail',
    template:`
                <div *ngIf="gumdam">
                    <h2>{{gumdam.name}}の机型说明!</h2>
                    <div><label>编号: </label>{{gumdam.id}}</div>
                    <div>
                        <label>机型: </label>
                        <input [(ngModel)]="gumdam.name" placeholder="name"/>
                    </div>
                    <div>
                        <label>中文名: </label>
                        <input value="{{gumdam.description}}" placeholder="description">
                    </div>
                </div>
                <button (click)="goBack()">Back</button>
              `
})
export class GumdamDetailComponent implements OnInit{

    ngOnInit(): void {
        this.route.params.forEach((params: Params) => {
            let id = +params['id'];
            this.gumdamService.getGumdam(id)
                .then(gumdam => this.gumdam = gumdam);
        });
    }

    constructor(
        private gumdamService: GumdamService,
        private route: ActivatedRoute,
        private location: Location
    ) {}

    gumdam: Gumdam;

    // 后退
    goBack():void {
        this.location.back();
    }
}
```

添加获取详情数据的方法

```
// 根据id得到gumdam
getGumdam(id: number): Promise<Gumdam> {
    return this.getGumdams().then(gumdams => gumdams.find(gumdam => gumdam.id === id));
}
```
{
遇到一个奇怪的错误：
`zone.js:1274 GET http://localhost:3000/traceur 404 (Not Found)`
http://stackoverflow.com/questions/37022526/angular-2-404-traceur-not-found

原因是自己作死改名字，导致粘贴过去，服务有错误导致的。
然而不是这个原因

真正的原因是：注释引发的血案
http://stackoverflow.com/questions/37179236/angular2-error-at-startup-of-the-app-http-localhost3000-traceur-404-not-fo
}

### 给dashboard添加点击事件

```
<div *ngFor="let gumdam of gumdams" (click)="gotoDetail(gumdam)" class="col-1-4">
```

```
// 跳转到详情页面
gotoDetail(gumdam: Gumdam): void {
    let link = ['/detail', gumdam.id];
    this.router.navigate(link);
}
```

### 在列表页面添加mini版详细页面（简单的信息和到详细页面的按钮）

```
<div *ngIf="selectedGumdam">
    <h2>
        {{selectedGumdam.name | uppercase}}
    </h2>
    <button (click)="gotoDetail()">View Details</button>
</div>
```

```
// 跳转到详情页面
gotoDetail(): void {
    let link = ['/detail', this.selectedGumdam.id];
    this.router.navigate(link);
}
```

### ts中的html,css提出去

```
@Component({
    moduleId: module.id,
    selector: 'my-gumdams',
    templateUrl: 'gumdams.component.html',
    styleUrls: [ 'gumdams.component.css' ]
})
```
注意templateUrl是单数，而styleUrls是复数
很好理解，一个组件一个页面，一个页面可以有多个样式文件。

### 优化页面样式

一些css文件，包括一些简单的媒体查询语句来实现响应式设计。

`routerLinkActive`指令

官方总结：
我们添加了 Angular 路由器 在各个不同组件之间导航。
我们学会了如何创建路由链接来表示导航栏的菜单项。
我们使用路由链接参数来导航到用户所选的英雄详情。
我们在多个组件之间共享了 HeroService 服务。
我们把 HTML 和 CSS 从组件中移出来，放到了它们自己的文件中。
我们添加了一个 uppercase 管道，来格式化数据。

## 第六步

### 添加http服务模块
```
import { HttpModule }       from '@angular/http';

@NgModule({
    imports: [ BrowserModule, FormsModule, HttpModule, routing ],
    declarations: [ AppComponent, GumdamDetailComponent, GumdamsComponent, DashboardComponent ],
    providers: [ GumdamService ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```
我们建议在根模块 AppModule 的 providers 数组中注册全应用级的服务。

### 伪造http请求服务器

让 http 客户端从一个 Mock 服务（ 内存 (in-memory)Web API ）中获取和保存数据。

```
// Imports for loading & configuring the in-memory web api
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';

@NgModule({
    imports: [ BrowserModule, FormsModule, HttpModule, InMemoryWebApiModule.forRoot(InMemoryDataService), routing ],
    declarations: [ AppComponent, GumdamDetailComponent, GumdamsComponent, DashboardComponent ],
    providers: [ GumdamService, InMemoryDataService ],
    bootstrap: [ AppComponent ]
})
```

```
import {Injectable} from "@angular/core";
import {InMemoryDbService} from "angular-in-memory-web-api/index";

@Injectable()
export class InMemoryDataService implements InMemoryDbService{
    createDb():{} {
        let gumdams = [
            { id: 11, name: 'ZGMF-X42S Destiny', description: '命运高达' },
            { id: 12, name: 'ZGMF-X19A Infinite Justice', description: '无限正义高达' },
            { id: 13, name: 'ZGMF-X20A Strike Freedom', description: '强袭自由高达' },
            { id: 14, name: 'ZGMF-X10A Freedom', description: '自由高达' },
            { id: 15, name: 'GN-0000/7SG 00 Gundam Seven Sword/G', description: '七剑G型00高达' },
            { id: 16, name: 'GNT-0000 Qan[T] 00 Quanta Gundam', description: '量子型00高达' },
            { id: 17, name: 'GN-002 Gundam Dynames', description: '力天使高达' },
            { id: 18, name: 'GN-003 Gundam Kyrios', description: '主天使高达' },
            { id: 19, name: 'GN-005 Gundam Virtue', description: '德天使高达' },
            { id: 20, name: 'GN-001 Gundam Exia', description: '能天使高达' }
        ];
        return gumdams;
    }
}
```

### 通过http取得数据

在gumdam.service中：
```
import { Headers, Http } from "@angular/http";
import 'rxjs/add/operator/toPromise';

private heroesUrl = 'app/gumdams';  // URL to web api

constructor(private http: Http) { }

getGumdams(): Promise<Gumdam[]> {
    return this.http.get(this.heroesUrl)
        .toPromise()
        .then(response => response.json().data as Gumdam[])
        .catch(this.handleError);
}
```

处理handlerError在后面

官方解释：
Angular 的 http.get 返回一个 RxJS 的 Observable 对象。 Observable( 可观察对象 ) 是一个管理异步数据流的强力方式。 后面我们还会进一步学习 可观察对象 。

现在 ，我们先利用 toPromise 操作符把 Observable 直接转换成 Promise 对象，回到已经熟悉的地盘。

不幸的是， Angular 的 Observable 并没有一个 toPromise 操作符 ... 没有打包在一起发布。 Angular 的 Observable 只是一个骨架实现。

有一大票像 toPromise 这样的操作符，会扩展 Observable ，为其添加有用的能力。 如果我们希望得到那些能力，就得自己添加那些操作符。 那很容易，只要从 RxJS 库中导入它们就可以了，就像这样：

```
import 'rxjs/add/operator/toPromise';
```

```
private handleError(error: any): Promise<any> {
    console.error('An error occurred', error); // for demo purposes only
    return Promise.reject(error.message || error);
}
```

官方说能够跑起来，我却卡在：
core.umd.js:3462 EXCEPTION: Uncaught (in promise): Error: unable to parse url 'gumdams'; original error: Cannot read property 'split' of undefined

这个错误的原因是需要两级域名 app/gumdams 就不会报这个错误。

可能是因为之前我输错url了，也会报错。

### 通过http更新数据

```
// 更新Gumdam
private headers = new Headers({'Content-Type': 'application/json'});

update(gumdam: Gumdam): Promise<Gumdam> {
    const url = `${this.gumdamsUrl}/${gumdam.id}`; // url 例如：app/gumdams/11
    return this.http
        .put(url, JSON.stringify(gumdam), {headers: this.headers})
        .toPromise()
        .then(() => gumdam)
        .catch(this.handleError);
}
```

好神奇！！

### 通过http添加数据

```
<div>
    <label>Gumdam name:</label> <input #name />
    <label>Gumdam description:</label> <input #description />
    <button (click)="add(name.value, description.value); name.value='';description.value=''">
        Add
    </button>
</div>
```

`<input #name />` 这种用法有点黑科技的感觉

```
// 添加
add(name: string, description: string): void {
    name = name.trim();
    description = description.trim();
    if (!name || !description) { return; }
    this.gumdamService.create(name, description)
        .then(gumdam => {
            this.gumdams.push(gumdam);
            this.selectedGumdam = null;
        });
}
```

```
// 创建Gumdam
create(name:string, description:string): Promise<Gumdam> {
    return this.http
        .post(this.gumdamsUrl, JSON.stringify({name: name,description: description}), {headers: this.headers})
        .toPromise()
        .then(res => res.json().data)
        .catch(this.handleError);
}
```

### 通过http删除数据
```
<button class="delete" (click)="delete(gumdam); $event.stopPropagation()">x</button>
```
`$event.stopPropagation()`  字面意思：事件停止传播
不知道官方用意为何，不写这个有没有问题？

刚一写完，官方解释就来了：
除了调用组件的 delete 方法之外，这个 delete 按钮的 click 处理器还应该阻止 click 事件向上冒泡——我们并不希望触发 `<li>` 的事件处理器，否则它会选中我们要删除的这位英雄。

```
// 删除
delete(gumdam: Gumdam): void {
    this.gumdamService
        .delete(gumdam.id)
        .then(() => {
            this.gumdams = this.gumdams.filter(h => h !== gumdam);
            if (this.selectedGumdam === gumdam) { this.selectedGumdam = null; }
        });
}
```

```
// 删除Gumdam
delete(id: number): Promise<void> {
    const url = `${this.gumdamsUrl}/${id}`;
    return this.http.delete(url, {headers: this.headers})
        .toPromise()
        .then(() => null)
        .catch(this.handleError);
}
```

至此增删改查完毕，使用的是内存数据模拟服务器数据库数据，以及内存WebAPI模拟http请求。采用的是RESTful，配合上Promise完成了这一系列操作。虽然，我之前没有实践过，经过流程这么一走，熟悉了ts编码风格。但是，这个过程中还是保留了很多疑问。

### 可观察对象（Observables）

官方背景：

一个 可观察对象 是一个事件流，我们可以用数组型操作符（函数）来处理它。

Angular 内核中提供了对可观察对象的基本支持。而我们这些开发人员可以自己从 RxJS 可观察对象 库中引入操作符和扩展。 我们会简短的讲解下如何做。

快速回忆一下 HeroService ，它在 http.get 返回的 Observable 后面串联了一个 toPromise 操作符。 该操作符把 Observable 转换成了 Promise （承诺），并且我们把那个“承诺”返回给了调用者。

转换成承诺通常是更好地选择，我们通常会要求 http.get 获取单块数据。只要接收到数据，就算完成。 使用承诺这种形式的结果是让调用方更容易写，并且承诺已经在 JavaScript 程序员中被广泛接受了。

但是请求并非总是“一次性”的。我们可以开始一个请求，并且取消它，再开始另一个不同的请求——在服务器对第一个请求作出回应之前。 像这样一个 请求 - 取消 - 新请求 的序列用 承诺 是很难实现的，但接下来我们会看到，它对于 可观察对象 却很简单。

这么一说，瞬间清晰很多。

```
import { Injectable }     from '@angular/core';
import { Http, Response } from '@angular/http';
import { Observable } from 'rxjs';
import { Gumdam }           from './gumdam';

@Injectable()
export class GumdamSearchService {
    constructor(private http: Http) {}
    search(term: string): Observable<Gumdam[]> {
        return this.http
            .get(`app/gumdams/?name=${term}`)
            .map((r: Response) => r.json().data as Gumdam[]);
    }
}

<div id="search-component">
    <h4>Gumdam Search</h4>
    <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
    <div>
        <div *ngFor="let gumdam of gumdams | async"
             (click)="gotoDetail(gumdam)" class="search-result" >
            {{gumdam.name}}
        </div>
    </div>
</div>
```

直接撸代码。

官方解释：（把hero替换成gumdam）
看到 heroes 属性现在是英雄列表的 Observable 对象，而不再只是英雄数组。 *ngFor 不能用可观察对象做任何事，除非我们在它后面跟一个 async pipe (AsyncPipe) 。 这个 async 管道会订阅到这个可观察对象，并且为 *ngFor 生成一个英雄数组。

最后,还有个组件文件没有创建：

```
import { Component, OnInit } from '@angular/core';
import { Router }            from '@angular/router';
import { Observable }        from 'rxjs/Observable';
import { Subject }           from 'rxjs/Subject';
import { GumdamSearchService } from './gumdam-search.service';
import { Gumdam } from './gumdam';
import './rxjs-extensions';

@Component({
    moduleId: module.id,
    selector: 'gumdam-search',
    templateUrl: 'gumdam-search.component.html',
    styleUrls: [ 'gumdam-search.component.css' ],
    providers: [ GumdamSearchService ]
})
export class GumdamSearchComponent implements OnInit {

    gumdams: Observable<Gumdam[]>;

    private searchTerms = new Subject<string>();

    constructor(
        private gumdamSearchService: GumdamSearchService,
        private router: Router
    ) {}

    // Push a search term into the observable stream.
    search(term: string): void {
        this.searchTerms.next(term);
    }

    ngOnInit(): void {
        this.gumdams = this.searchTerms
            .debounceTime(300)        // wait for 300ms pause in events
            .distinctUntilChanged()   // ignore if next search term is same as previous
            .switchMap(term => term   // switch to new observable each time
                // return the http search observable
                ? this.gumdamSearchService.search(term)
                // or the observable of empty heroes if no search term
                : Observable.of<Gumdam[]>([]))
            .catch(error => {
                // TODO: real error handling
                console.log(error);
                return Observable.of<Gumdam[]>([]);
            });
    }

    gotoDetail(gumdam: Gumdam): void {
        let link = ['/detail', gumdam.id];
        this.router.navigate(link);
    }
}
```

```
// Observable class extensions
import 'rxjs/add/observable/of';
import 'rxjs/add/observable/throw';

// Observable operators
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/distinctUntilChanged';
import 'rxjs/add/operator/do';
import 'rxjs/add/operator/filter';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/switchMap';
```

关于上面的代码官网balabala讲了一大堆。没有搞懂switchMap和map的用处。
我感觉一切都和观察者模式有关

console的network看不到发送的请求，因为这是内存web api。。。

官方总结：

我们添加了在应用程序中使用 HTTP 的必备依赖。
我们重构了 HeroService ，以通过 web API 来加载英雄数据。
我们扩展了 HeroService 来支持 post 、 put 和 delete 方法。
我们更新了组件，以允许用户添加、编辑和删除英雄。
我们配置了一个内存 Web API 。
我们学会了如何使用“可观察对象”。