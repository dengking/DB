# [Run-Time Loadable Extensions](https://sqlite.org/loadext.html)

## 1. Overview

Extensions can also be **statically linked** with the application. The code template shown below will work just as well as a statically linked extension as it does as a run-time loadable extension except that you should give the **entry point function** ("`sqlite3_extension_init`") a different name to avoid name collisions if your application contains two or more extensions.

> NOTE: 上面这段话是什么意思？结合下面的内容来进行理解: sqlite是允许load多个extension的，每个extension都需要有一个entry point function，如果都指定为`sqlite3_extension_init`的话，则就导致name collision了。为了解决这个问题，sqlite采用了一定的命名方案: `sqlite3_X_init`，参见原文的"2. Loading An Extension"章节。



> NOTE: Extensions的两种使用方式:
>
> 1) statically linked 
>
> 2) run-time load

## 2. Loading An Extension

> NOTE: 本节介绍run-time load方法。下面是sqlite提供的方法: 
>
> 1) C: [sqlite3_load_extension()](https://sqlite.org/c3ref/load_extension.html) 
>
> 2) SQL function: [Load_extension(X,Y)](https://sqlite.org/lang_corefunc.html#load_extension)
>
> 3) [command-line shell](https://sqlite.org/cli.html), extensions can be loaded using the "`.load`" dot-command
>
> 无论哪种方式，非常重要的是对**entry point function**的指定。其实也可以不指定，sqlite的默认寻找规则能够自动找到**entry point function**。



For security reasons, extension loading is turned off by default. In order to use either the C-language or SQL extension loading functions, one must first enable extension loading using the [sqlite3_db_config](https://sqlite.org/c3ref/db_config.html)(db,[SQLITE_DBCONFIG_ENABLE_LOAD_EXTENSION](https://sqlite.org/c3ref/c_dbconfig_defensive.html#sqlitedbconfigenableloadextension),1,NULL) C-language API in your application.

> NOTE: 必须要首先开启才能够使用

## 3. Compiling A Loadable Extension

```C++
gcc -g -fPIC -shared YourCode.c -o YourCode.so
```

> NOTE: 其实就是普通的编译so的方式

## 4. Programming Loadable Extensions

> NOTE: 不同的类型的extension使用不同的实现

## 5. Persistent Loadable Extensions

The default behavior for a loadable extension is that it is unloaded from process memory when the **database connection** that originally invoked [sqlite3_load_extension()](https://sqlite.org/c3ref/load_extension.html) closes.

However, if the initialization procedure returns [SQLITE_OK_LOAD_PERMANENTLY](https://sqlite.org/rescode.html#ok_load_permanently) instead of `SQLITE_OK`, then the extension will not be unloaded (`xDlClose` will not be invoked) and the extension will remain in process memory indefinitely. The `SQLITE_OK_LOAD_PERMANENTLY` return value is useful for extensions that want to register new [VFSes](https://sqlite.org/vfs.html).

To clarify: an extension for which the initialization function returns `SQLITE_OK_LOAD_PERMANENTLY` continues to exist in memory after the database connection closes. However, the extension is *not* automatically registered with subsequent database connections. This makes it possible to load extensions that implement new [VFSes](https://sqlite.org/vfs.html). To persistently **load** and **register** an **extension** that implements new SQL functions, collating sequences, and/or virtual tables, such that those added capabilities are available to all subsequent database connections, then the initialization routine should also invoke [sqlite3_auto_extension()](https://sqlite.org/c3ref/auto_extension.html) on a subfunction that will register those services.

> NOTE:自动load、register特性非常重要

The [vfsstat.c](https://sqlite.org/src/file/ext/misc/vfsstat.c) extension show an example of a loadable extension that persistently registers both a new VFS and a new virtual table. The [sqlite3_vfsstat_init()](https://sqlite.org/src/info/77b5b4235c9f7f11?ln=801-819) initialization routine in that extension is called only once, when the extension is first loaded. It registers the new "vfslog" VFS just that one time, and it returns SQLITE_OK_LOAD_PERMANENTLY so that the code used to implement the "vfslog" VFS will remain in memory. The initialization routine also invokes [sqlite3_auto_extension()](https://sqlite.org/c3ref/auto_extension.html) on a pointer to the "vstatRegister()" function so that all subsequent database connections will invoke the "vstatRegister()" function as they start up, and hence register the "vfsstat" virtual table.



## 6. Statically Linking A Run-Time Loadable Extension



