# Concurrency Control

> NOTE: 
>
> 这一节的内容是非常容易理解的

Anytime more than one query needs to change data at the same time, the problem of **concurrency control** arises. For our purposes in this chapter, MySQL has to do this at two levels: the **server level** and the **storage engine level**. **Concurrency control** is a big topic to which a large body of theoretical literature is devoted, so we will just give you a simplified overview of how MySQL deals with concurrent readers and writers, so you have the context you need for the rest of this chapter.

## Read/Write Locks

The solution to this classic problem of **concurrency control** is rather simple. Systems that deal with concurrent read/write access typically implement a locking system that consists of two lock types. These locks are usually known as **shared locks** and **exclusive locks**, or **read locks** and **write locks**.

In the database world, locking happens all the time: MySQL has to prevent one client from reading a piece of data while another is changing it. It performs this lock management internally in a way that is transparent much of the time.

## Lock Granularity

MySQL, on the other hand, does offer choices. Its **storage engines** can implement their own locking policies and lock granularities. Lock management is a very important decision in storage engine design; fixing the granularity at a certain level can give better performance for certain uses, yet make that engine less suited for other purposes. Because MySQL offers multiple storage engines, it doesn’t require a single general purpose solution. Let’s have a look at the two most important lock strategies.

> NOTE: 
>
> MySQL的lock是由storage engine来实现的；通过下面的描述可知，MySQL server本身也实现了一些lock
>
> 

### Table locks

The most basic locking strategy available in MySQL, and the one with the lowest overhead, is ***table locks***. A table lock is analogous to the mailbox locks described earlier: it locks the entire table. When a client wishes to write to a table (insert, delete, update, etc.), it acquires a write lock. This keeps all other read and write operations at bay. When nobody is writing, readers can obtain read locks, which don’t conflict with other read locks.

Although **storage engines** can manage their own locks, MySQL itself also uses a variety of locks that are effectively table-level for various purposes. For instance, the server uses a table-level lock for statements such as `ALTER TABLE`, regardless of the **storage engine**.

> NOTE: 
>
> MySQL本身也实现了一些lock

### Row locks

The locking style that offers the greatest concurrency (and carries the greatest overhead) is the use of ***row locks***. Row-level locking, as this strategy is commonly known, is available in the **InnoDB** and **XtraDB** storage engines, among others. Row locks are implemented in the storage engine, not the server (refer back to the logical architecture diagram if you need to). The server is completely unaware of locks implemented in the storage engines, and as you’ll see later in this chapter and throughout the book, the storage engines all implement locking in their own ways.

