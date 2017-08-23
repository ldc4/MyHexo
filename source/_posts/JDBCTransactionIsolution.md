title: JDBC事务、并发控制基本概念
date: 2016-03-31 21:11:11
categories:
- JDBC
tags:
- jdbc
- transaction
---

有的东西，看起来很庞大。实际上，也很庞大。
但是一些基本性的东西，还是不要忘。

<!--more-->

银行转账问题。(脑补)

事务是指一个工作单元，它包含了一组添加、删除、修改等数据操作命令，这组命令作为一个整体向系统提交执行，
**要么都执行成功，要么全部恢复**

JDBC中使用事务

	con.setAutoCommit(false)  先取消自动提交
	对数据库执行一个或多个操作
	con.commit() 提交事务
	如果某个操作失败，通过con.rollback()回滚所有操作

事务处理依赖于底层的数据库实现，不同的驱动程序对事务处理的支持程度可能不同

>思考：如果是在正在执行事务中宕机了，其实数据还是丢失了，会有这种风险。这种情况在分布式环境下或许不存在问题。只是说这种控制降低了风险，一定程度上控制了数据的完整提交。

数据库的并发就是不同的事务对同一部分数据执行操作

	导致的问题：
		- 读脏数据：事务T1修改某一数据，并将其写回数据库，事务T2读取同一数据后，T1由于某种原因被撤销，这时T1已修改过的数据恢复原值，T2读到的数据就与数据库中的数据不一致，我们称T2读到的数据就为”脏“数据，即不正确的数据
		- 不可重复读：事务T2读取数据后，事务T1执行更新操作，使T2无法再现前一次读取结果。
		- 幻读：事务T1按一定条件从数据库中读取某些数据记录后，事务T2插入了一些记录，当T1再次按相同条件读数据时，发现多了一些记录。


so,就需要

数据库并发控制：设置事务隔离级别

事务隔离级别就是事务执行时受打扰的程度，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越差，效率越低。

Connection对象提供如下方法：
getTransactionIsolation()  查看隔离级别
setTransactionIsolation(int level)  设置隔离级别
```
<code>Connection.TRANSACTION_READ_UNCOMMITTED</code>,
此级别允许某一事务读其他事务还没有更改完的数据。允许发生脏读、不可重复读和幻读
<code>Connection.TRANSACTION_READ_COMMITTED</code>,
此级别要求某一事务只能等别的事务全部更改完才能读。可以防止发生脏读，但不可重复读和幻读有可能发生
<code>Connection.TRANSACTION_REPEATABLE_READ</code>,
此级别要求某一事物只能等别的事务全部更改完才能读而且禁止不可重复读。
<code>Connection.TRANSACTION_SERIALIZABLE</code>,  
此级别事务只能一个接着一个执行
<code>Connection.TRANSACTION_NONE</code>.
此级别不支持事务
```

（全文完）

啊啊啊啊啊啊，貌似网易笔试挂啦。