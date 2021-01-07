# Consistency (database systems)

## wikipedia [Consistency (database systems)](https://en.wikipedia.org/wiki/Consistency_(database_systems))

**Consistency** in [database systems](https://en.wikipedia.org/wiki/Database_systems) refers to the requirement that any given [database transaction](https://en.wikipedia.org/wiki/Database_transaction) must change affected data only in allowed ways. Any data written to the database must be **valid** according to all defined **rules**, including [constraints](https://en.wikipedia.org/wiki/Integrity_constraints), [cascades](https://en.wikipedia.org/wiki/Cascading_rollback), [triggers](https://en.wikipedia.org/wiki/Database_trigger), and any combination thereof. This does not guarantee correctness of the **transaction** in all ways the application programmer might have wanted (that is the responsibility of application-level code) but merely that any programming errors cannot result in the violation of any defined database constraints.[[1\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-Date2012-1)

> NOTE: 写入database的数据必须是有效的，必须是符合数据库的规范的；

### As an ACID guarantee

Consistency is one of the four guarantees that define [ACID](https://en.wikipedia.org/wiki/ACID) [transactions](https://en.wikipedia.org/wiki/Database_transaction); however, significant ambiguity exists about the nature of this guarantee. It is defined variously as:

- The guarantee that any transactions started in the future necessarily see the effects of other transactions committed in the past[[2\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-CAP_Theorem_Paper-2)[[3\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-Ports_et_al-3)
- The guarantee that [database constraints](https://en.wikipedia.org/wiki/Relational_database#Constraints) are not violated, particularly once a transaction commits[[4\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-Haerder_&_Reuter-4)[[5\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-5)[[6\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-6)[[7\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-7)
- The guarantee that operations in transactions are performed accurately, correctly, and with validity, with respect to application semantics[[8\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-8)

As these various definitions are not mutually exclusive, it is possible to design a system that guarantees "consistency" in every sense of the word, as most [relational database management systems](https://en.wikipedia.org/wiki/Relational_database_management_system) in common use today arguably do.

### As a CAP trade-off

The [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) is based on three trade-offs, one of which is "atomic consistency" (shortened to "consistency" for the acronym), about which the authors note, "Discussing atomic consistency is somewhat different than talking about an ACID database, as **database consistency** refers to **transactions**, while **atomic consistency** refers only to a property of a single request/response operation sequence. And it has a different meaning than the Atomic in ACID, as it subsumes（包括） the database notions of both **Atomic** and **Consistent**."[[2\]](https://en.wikipedia.org/wiki/Consistency_(database_systems)#cite_note-CAP_Theorem_Paper-2)

> NOTE: 其实这篇文章的内容是比较不好的，它在这一段强调了在consistency在 **database consistency**和 [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) 中的差异所在。