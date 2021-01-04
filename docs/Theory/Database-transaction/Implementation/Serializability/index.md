# Serializability

"serializability"的含义是 "可串行性"，本文介绍的"serializability"是DB中的一个概念。

# wikipedia [Serializability](http://en.wiki.sxisa.org/wiki/Serializability)

In [concurrency control](http://en.wiki.sxisa.org/wiki/Concurrency_control) of [databases](http://en.wiki.sxisa.org/wiki/Database),[[1\]](http://en.wiki.sxisa.org/wiki/Serializability#cite_note-Bernstein87-1)[[2\]](http://en.wiki.sxisa.org/wiki/Serializability#cite_note-Weikum01-2) [transaction processing](http://en.wiki.sxisa.org/wiki/Transaction_processing) (transaction management), and various [transactional](http://en.wiki.sxisa.org/wiki/Database_transaction) applications (e.g., [transactional memory](http://en.wiki.sxisa.org/wiki/Transactional_memory)[[3\]](http://en.wiki.sxisa.org/wiki/Serializability#cite_note-Herlihy1993-3) and [software transactional memory](http://en.wiki.sxisa.org/wiki/Software_transactional_memory)), both centralized and [distributed](http://en.wiki.sxisa.org/wiki/Distributed_computing), a transaction [schedule](http://en.wiki.sxisa.org/wiki/Schedule_(computer_science)) is **serializable** if its outcome (e.g., the resulting database state) is equal to the outcome of its transactions executed serially(串行的), i.e. without overlapping in time. Transactions are normally executed concurrently (they overlap), since this is the most efficient way. Serializability is the major correctness criterion for concurrent transactions' executions[*[citation needed](http://en.wiki.sxisa.org/wiki/Wikipedia:Citation_needed)*]. It is considered the highest level of [isolation](http://en.wiki.sxisa.org/wiki/Isolation_(computer_science)) between [transactions](http://en.wiki.sxisa.org/wiki/Database_transaction), and plays an essential role in [concurrency control](http://en.wiki.sxisa.org/wiki/Concurrency_control). As such it is supported in all general purpose database systems. *[Strong strict two-phase locking](http://en.wiki.sxisa.org/wiki/Two-phase_locking)* (SS2PL) is a popular serializability mechanism utilized in most of the database systems (in various variants) since their early days in the 1970s.

**Serializability theory** provides the formal framework to reason about and analyze serializability and its techniques. Though it is [mathematical](http://en.wiki.sxisa.org/wiki/Mathematics) in nature, its fundamentals are informally (without mathematics notation) introduced below.



## Linearizability and serializability

在 wikipedia [Serializability](http://en.wiki.sxisa.org/wiki/Serializability) 的"See also"段中，有这样的描述: 

> [Linearizability](http://en.wiki.sxisa.org/wiki/Linearizability), a more general concept in [concurrent computing](http://en.wiki.sxisa.org/wiki/Concurrent_computing)