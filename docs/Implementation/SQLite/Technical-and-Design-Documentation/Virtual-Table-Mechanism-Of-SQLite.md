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





```C++
struct sqlite3_module {
  int iVersion;
  int (*xCreate)(sqlite3*, void *pAux,
               int argc, char *const*argv,
               sqlite3_vtab **ppVTab,
               char **pzErr);
  int (*xConnect)(sqlite3*, void *pAux,
               int argc, char *const*argv,
               sqlite3_vtab **ppVTab,
               char **pzErr);
  int (*xBestIndex)(sqlite3_vtab *pVTab, sqlite3_index_info*);
  int (*xDisconnect)(sqlite3_vtab *pVTab);
  int (*xDestroy)(sqlite3_vtab *pVTab);
  int (*xOpen)(sqlite3_vtab *pVTab, sqlite3_vtab_cursor **ppCursor);
  int (*xClose)(sqlite3_vtab_cursor*);
  int (*xFilter)(sqlite3_vtab_cursor*, int idxNum, const char *idxStr,
                int argc, sqlite3_value **argv);
  int (*xNext)(sqlite3_vtab_cursor*);
  int (*xEof)(sqlite3_vtab_cursor*);
  int (*xColumn)(sqlite3_vtab_cursor*, sqlite3_context*, int);
  int (*xRowid)(sqlite3_vtab_cursor*, sqlite_int64 *pRowid);
  int (*xUpdate)(sqlite3_vtab *, int, sqlite3_value **, sqlite_int64 *);
  int (*xBegin)(sqlite3_vtab *pVTab);
  int (*xSync)(sqlite3_vtab *pVTab);
  int (*xCommit)(sqlite3_vtab *pVTab);
  int (*xRollback)(sqlite3_vtab *pVTab);
  int (*xFindFunction)(sqlite3_vtab *pVtab, int nArg, const char *zName,
                     void (**pxFunc)(sqlite3_context*,int,sqlite3_value**),
                     void **ppArg);
  int (*Rename)(sqlite3_vtab *pVtab, const char *zNew);
  /* The methods above are in version 1 of the sqlite_module object. Those 
  ** below are for version 2 and greater. */
  int (*xSavepoint)(sqlite3_vtab *pVTab, int);
  int (*xRelease)(sqlite3_vtab *pVTab, int);
  int (*xRollbackTo)(sqlite3_vtab *pVTab, int);
  /* The methods above are in versions 1 and 2 of the sqlite_module object.
  ** Those below are for version 3 and greater. */
  int (*xShadowName)(const char*);
};
```

> NOTE: 传入一个 `sqlite3_module` 实例，该实例的每个成员指向对应的实现函数

### 1.3. Virtual Tables And Shared Cache

### 1.4. Creating New Virtual Table Implementations



## 2. Virtual Table Methods