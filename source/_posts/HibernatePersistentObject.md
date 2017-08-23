title: 深入理解Hibernate持久化对象
date: 2016-03-30 18:03:05
categories:
- Hibernate
tags:
- hibernate
- j2ee
---
当开发者需要深入了解Hibernate底层运行时，对Hibernate的数据访问进行优化时，就需要了解Hibernate的底层SQL操作了。
而对于绝大部分应用来说，数据访问是一个巨大的、耗时的操作，因此深入掌握Hibernate底层对应的SQL操作非常必要。

<!--more-->
# 持久化类的要求
- 提供一个无参数的构造器
- 提供一个标识属性
     虽然Hibernate允许使用8个基本数据类型作为标识属性的类型，但使用基本类型作为标识属性分类型在很多地方都不太方便，因此还是建议使用基本类型的包装类型作为标识属性
     虽然Hibernate可以允许持久化类没有标识符属性，而是让Hibernate内部来追踪对象的识别。但这样做将导致Hibernate许多功能无法使用。而且，Hibernate建议使用可以为空的类型来作为标识属性的类型，因此应该尽量避免使用基本数据类型
- 为持久化类的每个属性提供setter和getter方法
- 使用非final的类
     在运行时生成代理是Hibernate的一个重要功能。

- (重写equals和hashCode方法)(对于自动生成标识属性不适用)
     通常使用业务键值相等来实现equals和hashCode方法

# 持久化类的状态

- 瞬态：如果PO实例从未与Session关联过，该PO实例处于瞬态状态。
- 持久化：如果PO实例与Session关联起来，且该实例对应到数据库记录，则该实例处于持久化状态
- 托管：如果PO实例曾经与Session关联过，但因为Session的关闭等原因，PO实例脱离了Session的管理，这种状态被称为托管状态

# 改变持久化对象状态的方法

## 瞬态->持久化
Hibrenate Session提供了如下几个方法：
```
- Serializable save(Object obj)
     将obj对象变为持久化状态，该对象的属性将被保存到数据库
- void persist(Object obj)
     将obj对象转化为持久化状态，该对象的属性将被保存到数据库
- Serializable save(Object obj,Object pk)
- void persist(Object obj,Object pk)
```

例：
`session.save(user);`

Hibernate会在底层对应地生成一条insert语句

svae和persist区别：
一方面persist是为了照顾JPA的用法习惯（呸，照顾个毛线啊）
另一方面是sava有返回值，需要立即插入，而persist没有返回值，可以保证当它在一个事务外部被调用时，并不立即转换成insert语句。可用于长会话流程

## 加载一个持久化实例
例如：`User user = session.load(User.class,new Integer(pk));`

如果没有匹配的数据库记录，load方法可能（不是很理解，可能，难道有不确定性？）抛出HibernateException异常
如果设置了延迟加载，则load方法会返回一个未初始化的代理对象（Mock,模仿，替身，...），代理对象并没有装载数据记录，直到程序调用该代理对象的某方法时，Hibernate才会去访问数据库
如果希望在某对象中创建一个指向另一个对象的关联，又不想在从数据库中装载该对象时同时装载相关的所有对象，这种延迟加载的方式就非常有用了。

get方法与load方法。

get会立即访问数据库，如果没有记录，返回null,而返回不是一个代理对象

Hibernate会在底层对应地生成一条select语句

2者的区别在于是否延迟加载

## 托管->持久化
update、merge、updateOrSave

如果不清楚该对象是否曾经初始化过，那么可以选择使用updateOrSave

merge就是所谓的合并，但是该方法只是将当前对象的状态信息保存到数据库，并不会将该对象转换成持久化状态！

## delete方法删除持久化实例
`session.delete(user);`

## lock方法也可以将某个托管对象重新持久化
前提是：该对象没有被修改过

## 写在最后
Hibernate本身不提供直接执行update或delete语句的API，Hibernate提供的是一种面向对象的状态管理。
如果确实需要直接执行DML风格的update或delete类似的语句，建议使用Hibernate的批处理功能。
