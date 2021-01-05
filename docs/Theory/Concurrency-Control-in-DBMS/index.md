# Concurrency control in DBMS



## Multiple model

使用multiple model来描述DBMS中的concurrency model:

Concurrency unit: transaction



### Shared data

table

## Unit

transaction。

## wikipedia [Non-lock concurrency control](http://en.wiki.sxisa.org/wiki/Non-lock_concurrency_control)

In [Computer Science](http://en.wiki.sxisa.org/wiki/Computer_Science), in the field of [databases](http://en.wiki.sxisa.org/wiki/Database), **non-lock concurrency control** is a [concurrency control](http://en.wiki.sxisa.org/wiki/Concurrency_control) method used in [relational databases](http://en.wiki.sxisa.org/wiki/Relational_database) without using [locking](http://en.wiki.sxisa.org/wiki/Lock_(computer_science)).

There are several non-lock concurrency control methods, which involve the use of timestamps on transaction to determine transaction priority:

- [Optimistic concurrency control](http://en.wiki.sxisa.org/wiki/Optimistic_concurrency_control)
- [Timestamp-based concurrency control](http://en.wiki.sxisa.org/wiki/Timestamp-based_concurrency_control)
- [Multiversion concurrency control](http://en.wiki.sxisa.org/wiki/Multiversion_concurrency_control)

> NOTE: 由于这些concurrency control方式不仅仅局限于DBMS，还可以用于其他的领域，所以并没有将它们的实现放到本工程，而是放到了工程parallel-computing中。



## geeksforgeeks [Concurrency Control in DBMS](https://www.geeksforgeeks.org/concurrency-control-in-dbms/)

Concurrency Control deals with **interleaved execution** of more than one transaction. In the next article, we will see what is serializability and how to find whether a schedule is serializable or not.

