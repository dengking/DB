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

## 1.1. Usage

### [CREATE VIRTUAL TABLE](https://www.sqlite.org/lang_createvtab.html) 

A virtual table is created using a [CREATE VIRTUAL TABLE](https://www.sqlite.org/lang_createvtab.html) statement.

> NOTE:思考：如何register module？这在下面的“1.2. Implementation”章节中进行了详细介绍。

### Module-argument 

The format of the arguments to the module is very general. Each **module-argument** may contain keywords, string literals, identifiers, numbers, and punctuation. Each **module-argument** is passed as written (as text) into the [constructor method](https://sqlite.org/vtab.html#xcreate) of the virtual table implementation when the virtual table is created and that constructor is responsible for parsing and interpreting the arguments. The argument syntax is sufficiently general that a virtual table implementation can, if it wants to, interpret its arguments as [column definitions](https://sqlite.org/lang_createtable.html#tablecoldef) in an ordinary [CREATE TABLE](https://sqlite.org/lang_createtable.html) statement. The implementation could also impose some other interpretation on the arguments.

> NOTE: 由constructor method来实现对module-argument的parsing and interpreting；
>
> 可以将column definition通过module-argument传入。

### 1.1.1. Temporary virtual tables

### 1.1.2. Eponymous virtual tables

> NOTE: "eponymous"的意思是: 以本名命名的
>
> 所谓的Eponymous virtual table，其实就是sqlite实现中，使用到的virtual table，比如非常典型的：[dbstat virtual table](https://sqlite.org/dbstat.html).



## 1.2. Implementation



### The [sqlite3_module](https://sqlite.org/c3ref/module.html) structure

> NOTE: 更加准确地说是“virtual table module”，这个名称更好理解。

Think of a module as a class from which one can construct multiple virtual tables having similar properties. For example, one might have a module that provides read-only access to comma-separated-value (CSV) files on disk. That one module can then be used to create several virtual tables where each virtual table refers to a different CSV file.

> NOTE: 也就是说，一类virtual table，需要实现一个virtual table module，比如上面提及的： read-only access to comma-separated-value (CSV) files。

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

The module structure defines all of the methods for each virtual table object. 



### The [sqlite3_vtab](https://sqlite.org/c3ref/vtab.html) structure

```C
struct sqlite3_vtab {
  const sqlite3_module *pModule;
  int nRef;
  char *zErrMsg;
};
```



Virtual table implementations will normally **subclass** this structure to add additional private and implementation-specific fields. 

> NOTE: 如何实现subclass？C中貌似没有subclass机制；在File [ext/misc/csv.c](https://sqlite.org/src/finfo?name=ext/misc/csv.c&m=tip) from the [latest check-in](https://sqlite.org/src/info/tip)中给出了这样的一个例子:
>
> ```C
> /* An instance of the CSV virtual table */
> typedef struct CsvTable {
>   sqlite3_vtab base;              /* Base class.  Must be first */
>   char *zFilename;                /* Name of the CSV file */
>   char *zData;                    /* Raw CSV data in lieu of zFilename */
>   long iStart;                    /* Offset to start of data in zFilename */
>   int nCol;                       /* Number of columns in the CSV file */
>   unsigned int tstFlags;          /* Bit values used for testing */
> } CsvTable;
> ```
>
> 

The `nRef` field is used internally by the SQLite core and should not be altered by the virtual table implementation. The virtual table implementation may pass error message text to the core by putting an error message string in `zErrMsg`. Space to hold this error message string must be obtained from an SQLite memory allocation function such as [sqlite3_mprintf()](https://sqlite.org/c3ref/mprintf.html) or [sqlite3_malloc()](https://sqlite.org/c3ref/free.html). Prior to assigning a new value to `zErrMsg`, the virtual table implementation must free any preexisting content of `zErrMsg` using [sqlite3_free()](https://sqlite.org/c3ref/free.html). Failure to do this will result in a memory leak.

> NOTE: 看了，sqlite是自己实现的memory management。



### The [sqlite3_vtab_cursor](https://sqlite.org/c3ref/vtab_cursor.html) structure

> NOTE: [sqlite3_vtab_cursor](https://sqlite.org/c3ref/vtab_cursor.html) 其实相当于一个 pointer

Once again, practical implementations will likely subclass this structure to add additional private fields.

> NOTE:在File [ext/misc/csv.c](https://sqlite.org/src/finfo?name=ext/misc/csv.c&m=tip) from the [latest check-in](https://sqlite.org/src/info/tip)中给出了这样的一个例子:
>
> ```C
> /* A cursor for the CSV virtual table */
> typedef struct CsvCursor {
>   sqlite3_vtab_cursor base;       /* Base class.  Must be first */
>   CsvReader rdr;                  /* The CsvReader object */
>   char **azVal;                   /* Value of the current row */
>   int *aLen;                      /* Length of each entry */
>   sqlite3_int64 iRowid;           /* The current rowid.  Negative for EOF */
> } CsvCursor;
> ```
>
> 

### Register module: `sqlite3_create_module`

Before a [CREATE VIRTUAL TABLE](https://sqlite.org/lang_createvtab.html) statement can be run, the module specified in that statement must be registered with the database connection. This is accomplished using either of the [sqlite3_create_module()](https://sqlite.org/c3ref/create_module.html) or [sqlite3_create_module_v2()](https://sqlite.org/c3ref/create_module.html) interfaces.

```c
int sqlite3_create_module(
  sqlite3 *db,               /* SQLite connection to register module with */
  const char *zName,         /* Name of the module */
  const sqlite3_module *,    /* Methods for the module */
  void *                     /* Client data for xCreate/xConnect */
);
int sqlite3_create_module_v2(
  sqlite3 *db,               /* SQLite connection to register module with */
  const char *zName,         /* Name of the module */
  const sqlite3_module *,    /* Methods for the module */
  void *,                    /* Client data for xCreate/xConnect */
  void(*xDestroy)(void*)     /* Client data destructor function */
);
```



### 1.3. Virtual Tables And Shared Cache



### 1.4. Creating New Virtual Table Implementations

> NOTE: virtual table module有两种实现方式:
>
> | 实现方式                                                     | 说明 |
> | ------------------------------------------------------------ | ---- |
> | [Run-Time Loadable Extensions](https://sqlite.org/loadext.html) |      |
> | 和SQLite编译到一起                                           |      |
>
> 

## 2. Virtual Table Methods

### 2.1. The `xCreate` Method

```c++
int (*xCreate)(sqlite3 *db, void *pAux,
             int argc, char *const*argv,
             sqlite3_vtab **ppVTab,
             char **pzErr);
```



#### `argv`

|                        | 是否必传 | 说明                                                         |
| ---------------------- | -------- | ------------------------------------------------------------ |
| `argv[0]`              | yes      | the name of the **module** being invoked                     |
| `argv[1]`              | yes      | the name of the **database** in which the new **virtual table** is being created |
| `argv[2]`              | yes      | the name of the new virtual table, as specified following the TABLE keyword in the [CREATE VIRTUAL TABLE](https://sqlite.org/lang_createvtab.html) statement |
| `argv[4]-argv[argc-1]` | no       | the fourth and subsequent strings in the argv[] array report the arguments to the module name in the [CREATE VIRTUAL TABLE](https://sqlite.org/lang_createvtab.html) statement. |

#### `ppVTab`

The job of this method is to construct the new virtual table object (an [sqlite3_vtab](https://sqlite.org/c3ref/vtab.html) object) and return a pointer to it in `*ppVTab`.



#### [sqlite3_declare_vtab()](https://sqlite.org/c3ref/declare_vtab.html)

The second argument to [sqlite3_declare_vtab()](https://sqlite.org/c3ref/declare_vtab.html) must a zero-terminated UTF-8 string that contains a well-formed [CREATE TABLE](https://sqlite.org/lang_createtable.html) statement that defines the columns in the virtual table and their data types. The name of the table in this CREATE TABLE statement is ignored, as are all constraints. Only the column names and datatypes matter. The CREATE TABLE statement string need not to be held in persistent memory. The string can be deallocated and/or reused as soon as the [sqlite3_declare_vtab()](https://sqlite.org/c3ref/declare_vtab.html) routine returns.

> NOTE: 参数`zCreateTable`是有谁来组织？



If the xCreate method is the exact same pointer as the [xConnect](https://sqlite.org/vtab.html#xconnect) method, that indicates that the virtual table does not need to initialize backing store. Such a virtual table can be used as an [eponymous virtual table](https://sqlite.org/vtab.html#epovtab) or as a named virtual table using [CREATE VIRTUAL TABLE](https://sqlite.org/lang_createvtab.html) or both.

> NOTE: backing store在2.2. The xConnect Method中也有所提及，并且其中进行了更加详细的说明

### 2.1.1. Hidden columns in virtual tables



### 2.1.2. Table-valued functions



### 2.1.3. WITHOUT ROWID Virtual Tables



### 2.2. The xConnect Method

