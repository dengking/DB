# Atomicity (database systems)

需要注意的是，只有DB transaction才具备ACID特性，其他的transaction不一定具备ACID特性。

## cnblogs [数据库事务的四大特性以及事务的隔离级别](https://www.cnblogs.com/fjdingsd/p/5273008.html)

本篇讲诉数据库中事务的四大特性（ACID），并且将会详细地说明事务的隔离级别。

如果一个数据库声称支持事务的操作，那么该数据库必须要具备以下四个特性：

### ⑴ 原子性（Atomicity）

　　原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚，这和前面两篇博客介绍事务的功能是一样的概念，因此事务的操作如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。

## wikipedia [Atomicity (database systems)](http://en.wiki.sxisa.org/wiki/Atomicity_(database_systems))

An **atomic transaction** is an *indivisible* and *[irreducible](http://en.wiki.sxisa.org/wiki/Irreducibility)* series of database operations such that either *all* occur, or *nothing* occurs.[[1\]](http://en.wiki.sxisa.org/wiki/Atomicity_(database_systems)#cite_note-1) A guarantee of atomicity prevents updates to the database occurring only partially, which can cause greater problems than rejecting the whole series outright. As a consequence, the transaction cannot be observed to be in progress by another database client. At one moment in time, it has not yet happened, and at the next it has already occurred in whole (or nothing happened if the transaction was cancelled in progress).

> NOTE: transaction的atomicity意味着"all-or-nothing"，它并不表示DB对transaction的执行是完全isolation的(在isolation章节会进行专门描述)，DB对transaction的执行是允许一定程度的concurrency的，这在`Theory\Concurrency-Control-in-DBMS`章节中进行了描述。
>
> 这和我们平时所说的atomic的概念是有些不同的，主要是因为DBMS中，transaction是调度的单位，而不是执行的单位。





### Orthogonality

> NOTE: 这一段的分析是非常好的，描述清楚了ACID之间的关系

Atomicity does not behave completely [orthogonally](http://en.wiki.sxisa.org/wiki/Orthogonal_(computing)) with regard to the other [ACID](http://en.wiki.sxisa.org/wiki/ACID) properties of the transactions. For example, [isolation](http://en.wiki.sxisa.org/wiki/Isolation_(database_systems)) relies on atomicity to roll back changes in the event of isolation failures such as [deadlock](http://en.wiki.sxisa.org/wiki/Deadlock); [consistency](http://en.wiki.sxisa.org/wiki/Consistency_(database_systems)) also relies on rollback in the event of a consistency-violation by an illegal transaction. Finally, atomicity itself relies on [durability](http://en.wiki.sxisa.org/wiki/Durability_(database_systems)) to ensure the atomicity of transactions even in the face of external failures.

As a result of this, failure to detect errors and roll back the enclosing transaction may cause failures of isolation and consistency.