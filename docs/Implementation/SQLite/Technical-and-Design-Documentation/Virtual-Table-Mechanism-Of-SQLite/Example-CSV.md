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

