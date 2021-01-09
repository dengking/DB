# Isolation



## wikipedia [Isolation](https://en.wikipedia.org/wiki/Isolation_(database_systems))

### Concurrency control

> NOTE: isolation是和concurrency control密切相关的



## wikipedia [Multiversion concurrency control](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)

[Isolation](https://en.wikipedia.org/wiki/ACID#Isolation) is the property that provides guarantees in the concurrent accesses to data. Isolation is implemented by means of a [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) protocol. The simplest way is to make all readers wait until the writer is done, which is known as a read-write [lock](https://en.wikipedia.org/wiki/Lock_(database)). Locks are known to create contention especially between long read transactions and update transactions. 



## Isolation and concurrency control

Isolation是和concurrency control密切相关的，在下面文章中，谈及了这个话题:

1、wikipedia [Multiversion concurrency control](https://en.wikipedia.org/wiki/Multiversion_concurrency_control)