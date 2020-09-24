# [The Virtual Table Mechanism Of SQLite](https://www.sqlite.org/vtab.html)

## 1. Introduction

A **virtual table** is an object that is registered with an open SQLite [database connection](https://www.sqlite.org/c3ref/sqlite3.html). From the perspective of an **SQL statement**, the **virtual table object** looks like any other table or view. But behind the scenes, queries and updates on a virtual table invoke callback methods of the virtual table object instead of reading and writing on the database file.

> NOTE: 显然，virtual table is an abstraction；virtual table的callback method，在“2. Virtual Table Methods”中进行了介绍。



- One cannot create a trigger on a virtual table.

- One cannot create additional indices on a virtual table. (Virtual tables can have indices but that must be built into the virtual table implementation. Indices cannot be added separately using [CREATE INDEX](https://www.sqlite.org/lang_createindex.html) statements.)

- One cannot run [ALTER TABLE ... ADD COLUMN](https://www.sqlite.org/lang_altertable.html) commands against a virtual table.


> NOTE: 上述cannot要如何理解？是指一旦用户提交了这样的语句，就会导致崩溃？还是对应的语句无法被执行？需要实践来验证。

Individual virtual table implementations might impose additional constraints. For example, some virtual implementations might provide read-only tables. Or some virtual table implementations might allow [INSERT](https://www.sqlite.org/lang_insert.html) or [DELETE](https://www.sqlite.org/lang_delete.html) but not [UPDATE](https://www.sqlite.org/lang_update.html). Or some virtual table implementations might limit the kinds of UPDATEs that can be made.

> NOTE: 这些constrain要如何来实现？

See the [list of virtual tables](https://www.sqlite.org/vtablist.html) page for a longer list of actual virtual table implementations.

### 1.1. Usage

A virtual table is created using a [CREATE VIRTUAL TABLE](https://www.sqlite.org/lang_createvtab.html) statement.

#### 1.1.1. Temporary virtual tables

#### 1.1.2. Eponymous virtual tables

> NOTE: "eponymous"的意思是: 以本名命名的



### 1.2. Implementation



### 1.3. Virtual Tables And Shared Cache

### 1.4. Creating New Virtual Table Implementations



## 2. Virtual Table Methods