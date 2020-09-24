# [The Virtual Table Mechanism Of SQLite](https://www.sqlite.org/vtab.html)

## 1. Introduction

A **virtual table** is an object that is registered with an open SQLite [database connection](https://www.sqlite.org/c3ref/sqlite3.html). From the perspective of an SQL statement, the virtual table object looks like any other table or view. But behind the scenes, queries and updates on a virtual table invoke callback methods of the virtual table object instead of reading and writing on the database file.

> NOTE: 显然，virtual table is an abstraction；virtual table的callback method，在“2. Virtual Table Methods”中进行了介绍。

### 1.1. Usage



## 2. Virtual Table Methods