# Database transaction

Transaction是DBMS中的核心概念，DBMS中的如下内容的基石:

1) unit of work

2) unit of concurrency control

## wikipedia [Database transaction](https://en.wikipedia.org/wiki/Database_transaction)

A **database transaction** symbolizes(象征、表示) a **unit of work** performed within a [database management system](https://en.wikipedia.org/wiki/Database_management_system) (or similar system) against a database, and treated in a coherent(连贯的、一致的) and reliable way independent of other transactions. A transaction generally represents any change in a database. 

> NOTE: **unit of work**是理解上面这段话的关键所在，需要以文章《Unit》中描述的思想来看待transaction:
>
> 1、transaction是DB中work的单位，即在DB中，以transaction为单位来度量work，显然，它是比SQL语句更大的一个量级
>
> 2、显然transaction是一个非常好的抽象，它抽象地表示了database work，符合DB的诸多需求，下面进行了描述

### Purpose

Transactions in a database environment have two main purposes:

1、To provide reliable units of work that allow correct recovery from failures and keep a database consistent even in cases of system failure, when execution stops (completely or partially) and many operations upon a database remain uncompleted, with unclear status.

2、To provide isolation between programs accessing a database concurrently. If this isolation is not provided, the programs' outcomes are possibly erroneous.

> NOTE: 如何实现isolation呢？



### Property

A database transaction, by definition, must be [atomic](https://en.wikipedia.org/wiki/Atomicity_(database_systems)) (it must either complete in its entirety or have no effect whatsoever), [consistent](https://en.wikipedia.org/wiki/Consistency_(database_systems)) (it must conform to existing constraints in the database), [isolated](https://en.wikipedia.org/wiki/Isolation_(database_systems)) (it must not affect other transactions) and [durable](https://en.wikipedia.org/wiki/Durability_(database_systems)) (it must get written to persistent storage).[[1\]](https://en.wikipedia.org/wiki/Database_transaction#cite_note-1) Database practitioners often refer to these properties of database transactions using the acronym [ACID](https://en.wikipedia.org/wiki/ACID).

> NOTE: 在`Theory\Database-transaction\ACID`中进行了专门的介绍。

### Distributed transactions

> NOTE: 放到了distributed computing章节中了

### Transactional filesystems

> NOTE: 这其实是将transaction概念使用到filesystem中

## ucsd [Lecture 8: Transactions, ACID, 2PC, 2PL, Serializability](https://cseweb.ucsd.edu/classes/sp16/cse291-e/applications/ln/lecture8.html)

### ACID Transactions

Traditional database systems have relied upon bundling work into *transactions* that have the *ACID* properties. In so doing, they guarantee **consistency** at the expense of availability and/or partition tolerance. 

> NOTE: CAP theory

*ACID* is an acronym for:

1、Atomicity

2、Consistency (serializability)

3、Isolation

4、Durability



1、Acid - "All or nothing"

2、Consistency -- This implies two types of consistency. It implies that a single system is consistent and that there is consistency across systems. In other words, if $100 is moved from one bank account to another, not only is it subtracted from one and added to another on one host -- it appears this way everywhere. It is this property that allows one transaction to safely follow another.

3、Isolation - Regardless of the level of concurrency, transactions must yields the same results as if they were executed one at a time (but any one of perhaps several orderings).

4、Durability - permanance. Changes persist over crashes, &c.

### Transactions, Detail and Example

*Transactions* are sequences of actions such that **all** of the operations within the **transaction** succeed (on all recipients) and their effects are permanantly visible, or none of **none** of the operations suceed anywhere and they have no visible effects; this might be because of failure (unintentional) or an abort (intentional).

> NOTE: 简而言之: "All or nothing"

### Mommit point

Characterisitically, transactions have a *commit point*. This is the point of no return. Before this point, we can **undo** a transaction. After this point, all changes are permanant. If problems occur after the **commit point**, we can take compensating(补偿；修正；抵消) or corrective action, but we can't wave a magic wand(魔法棒) and **undo** it.

> NOTE: commit后是不能redo的

### Distributed Transactions and Atomic Commit Protocols

> NOTE: 放到了distributed computing章节中了