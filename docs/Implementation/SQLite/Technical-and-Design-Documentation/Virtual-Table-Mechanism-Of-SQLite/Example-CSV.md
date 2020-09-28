# Example CSV

本文是阅读[ext/misc/csv.c](https://sqlite.org/src/finfo?name=ext/misc/csv.c&m=tip) from the [latest check-in](https://sqlite.org/src/info/tip)的笔记。

## Usage

> NOTE: 源代码中，头部给出的注释中说明的usage如下：

```sql
.load ./csv # 加载extension
CREATE VIRTUAL TABLE temp.csv USING csv(filename=FILENAME);
SELECT * FROM csv;
```

> NOTE: 我第一次错误的将`temp.csv`理解为`table-name`，但是`SELECT`语句中的`table-name`为`csv`，显然与前面的矛盾。后来查看了[1.1. Usage](https://sqlite.org/vtab.html)方知: `temp.csv`为`schema-name.table-name`。

The columns are named "c1", "c2", "c3", ... by default.  Or the application can define its own CREATE TABLE statement using the
`schema= parameter`, like this:

```SQL
   CREATE VIRTUAL TABLE temp.csv2 USING csv(
      filename = "../http.log",
      schema = "CREATE TABLE x(date,ipaddr,url,referrer,userAgent)"
   );
```
> NOTE: 上面的描述是想告诉我们，可以通过`schema= parameter`来自定义table的column。

Instead of specifying a file, the text of the CSV can be loaded using the `data= parameter`.

> NOTE: 可以通过file name来指定CSV数据，也可以通过`data= parameter`的方式来指定CSV数据

If the `columns=N` parameter is supplied, then the CSV file is assumed to have `N` columns.  If both the `columns=` and `schema=` parameters are omitted, then the number and names of the columns is determined by the first line of the CSV input.



> NOTE: 理解usage非常重要，`csvtabConnect`就是基于它实现的。





## 实现方式

采用的是[Run-Time Loadable Extensions](https://sqlite.org/loadext.html)，因为它的头文件包含了:

```C++
#include <sqlite3ext.h>
```

## `sqlite3_module`

它的`sqlite3_module`是`CsvModule`



## `xCreate` and `xConnect`



| call back  | 实现            |
| ---------- | --------------- |
| `xCreate`  | `csvtabCreate`  |
| `xConnect` | `csvtabConnect` |

需要注意的是，"The xConnect and xCreate methods do the same thing, but they must be different so that the virtual table is not an eponymous virtual table"，查看其实现可知，`csvtabCreate`直接调用的`csvtabConnect`，关于这样做的原因，在[2.2. The xConnect Method](https://sqlite.org/vtab.html)中进行了详细介绍。



## Virtual table struct: `CsvTable`

在1.2. Implementation中说明:"Virtual table implementations will normally **subclass** this structure to add additional private and implementation-specific fields"，在本实现中，就是按照这种方式做的:

```C
/* An instance of the CSV virtual table */
typedef struct CsvTable {
  sqlite3_vtab base;              /* Base class.  Must be first */
  char *zFilename;                /* Name of the CSV file */
  char *zData;                    /* Raw CSV data in lieu of zFilename */
  long iStart;                    /* Offset to start of data in zFilename */
  int nCol;                       /* Number of columns in the CSV file */
  unsigned int tstFlags;          /* Bit values used for testing */
} CsvTable;
```

需要对成员变量`zFilename`和`zData`进行特殊说明:

在"Usage"章节中，我们知道"Instead of specifying a file, the text of the CSV can be loaded using the `data= parameter`"，也就是说：可以通过file name来指定CSV数据，也可以通过`data= parameter`的方式来指定CSV数据，注释"Raw CSV data in lieu of zFilename"的意思是：原始CSV数据代替zFilename，也就是说:

| 成员变量    | 说明                                    |
| ----------- | --------------------------------------- |
| `zFilename` | 保存“Name of the CSV file"              |
| `zData`     | 保存通过`data= parameter`指定的CSV data |



## Virtual table cursor struct: `CsvCursor`

在1.2. Implementation中说明:"practical implementations will likely subclass this structure to add additional private fields."，在本实现中，就是按照这种方式做的:

```C
/* A cursor for the CSV virtual table */
typedef struct CsvCursor {
  sqlite3_vtab_cursor base;       /* Base class.  Must be first */
  CsvReader rdr;                  /* The CsvReader object */
  char **azVal;                   /* Value of the current row */
  int *aLen;                      /* Length of each entry */
  sqlite3_int64 iRowid;           /* The current rowid.  Negative for EOF */
} CsvCursor;
```



## `CsvReader`

封装的csv file

```c++
/* A context object used when read a CSV file. */
typedef struct CsvReader CsvReader;
struct CsvReader {
  FILE *in;              /* Read the CSV text from this input stream */
  char *z;               /* Accumulated text for a field */
  int n;                 /* Number of bytes in z */
  int nAlloc;            /* Space allocated for z[] */
  int nLine;             /* Current line number */
  int bNotFirst;         /* True if prior text has been seen */
  int cTerm;             /* Character that terminated the most recent field */
  size_t iIn;            /* Next unread character in the input buffer */
  size_t nIn;            /* Number of characters in the input buffer */
  char *zIn;             /* The input buffer */
  char zErr[CSV_MXERR];  /* Error message */
};
```

