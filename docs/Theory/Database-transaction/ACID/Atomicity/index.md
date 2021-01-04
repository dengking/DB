# Atomicity (database systems)

需要注意的是，只有DB transaction才具备ACID特性，其他的transaction不一定具备ACID特性。

## cnblogs [数据库事务的四大特性以及事务的隔离级别](https://www.cnblogs.com/fjdingsd/p/5273008.html)

本篇讲诉数据库中事务的四大特性（ACID），并且将会详细地说明事务的隔离级别。

如果一个数据库声称支持事务的操作，那么该数据库必须要具备以下四个特性：

### ⑴ 原子性（Atomicity）

　　原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚，这和前面两篇博客介绍事务的功能是一样的概念，因此事务的操作如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。

## wikipedia [Atomicity (database systems)](http://en.wiki.sxisa.org/wiki/Atomicity_(database_systems))

An **atomic transaction** is an *indivisible* and *[irreducible](http://en.wiki.sxisa.org/wiki/Irreducibility)* series of database operations such that either *all* occur, or *nothing* occurs.[[1\]](http://en.wiki.sxisa.org/wiki/Atomicity_(database_systems)#cite_note-1) A guarantee of atomicity prevents updates to the database occurring only partially, which can cause greater problems than rejecting the whole series outright. As a consequence, the transaction cannot be observed to be in progress by another database client. At one moment in time, it has not yet happened, and at the next it has already occurred in whole (or nothing happened if the transaction was cancelled in progress).

> NOTE: transaction的atomicity意味着"all-or-nothing"，它并不表示DB对transaction的执行是完全isolation的，DB对transaction的执行是允许一定程度的concurrency的。
>
> 这和我们平时所说的atomic的概念是有些不同的；





