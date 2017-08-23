title: Spring基本用法复习
date: 2016-05-17 21:37:59
categories:
- J2EE
tags:
- spring
---

Spring 最认同的技术是控制反转的依赖注入（DI）模式。
控制反转（IoC）是一个通用的概念，它可以用许多不同的方式去表达，依赖注入仅仅是控制反转的一个具体的例子。

IoC AOP IoC AOP...

<!--more-->

IoC容器
BeanFactory
ApplicationContext


Bean定义：
id/name class scope
init-method destroy-method lazy-init

Bean作用域：
singleton、prototype、request、session

Bean生命周期：
初始化回调：
1.implements InitializationBean
public void afterPropertiesSet(){}

2.init-method指定一个void无参的方法名

销毁回调：
1.implements DisposableBean
public void distroy(){}

2.destroy-method指定一个void无参的方法名

疑惑点：destroy-method指定的方法在prototype作用域不会被执行
There is one quite important thing to be aware of when deploying a bean in the prototype scope, in that the lifecycle of the bean changes slightly. Spring does not manage the complete lifecycle of a prototype bean: the container instantiates, configures, decorates and otherwise assembles a prototype object, hands it to the client and then has no further knowledge of that prototype instance. This means that while initialization lifecycle callback methods will be called on all objects regardless of scope, in the case of prototypes, any configured destruction lifecycle callbacks will not be called.

Spring不能对一个prototype bean的整个生命周期负责，容器在初始化、配置、装饰或者是装配完一个prototype实例后，将它交给客户端，随后就对该prototype实例不闻不问了。不管何种作用域，容器都会调用所有对象的初始化生命周期回调方法，而对prototype而言，任何配置好的析构生命周期回调方法都将不会被调用。 清除prototype作用域的对象并释放任何prototype bean所持有的昂贵资源，都是客户端代码的职责。

BeanPostProcessor后置处理器

bean的继承
parent="" 以及 abstract="true"


依赖注入的两种方式：1.通过构造函数 2.通过setter方法

注入多个bean:<list>,<set>,<map>,<props>

其实spring依赖注入就是玩配置
然后为了使配置文件不那么大，自动装配就来了

自动装配：autowire

装配模式：
byName
byType

基于注解的配置
<context:annotation-config/>

@Required
@Autowire  是基于byType
@Qualifier

@PostContruct
@PreDestroy
@Resource  是基于byName

基于java的配置真sb..

spring事件处理与自定义事件

spring AOP
基本概念：
```
Aspect 切面
Joinpoint 连接点->
                 Introduction引入、Weaving织入-> pointcut 切入点
advice 增强处理 -> 
--------------------------------------------------------------
                  基于 TargetObject 目标对象
```


参考资料：
http://wiki.jikexueyuan.com/project/spring/





