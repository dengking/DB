# SQLite



## [About SQLite](https://sqlite.org/about.html)

SQLite is an in-process library that implements a [self-contained](https://sqlite.org/selfcontained.html), [serverless](https://sqlite.org/serverless.html), [zero-configuration](https://sqlite.org/zeroconf.html), [transactional](https://sqlite.org/transactional.html) SQL database engine. 

> NOTE: SQLite 编译生成的是一个so，Linux下的`sqlite3` dynamic link对应的so:
>
> ```C++
> [root@localhost ~]# ldd -r /usr/bin/sqlite3 
> 	linux-vdso.so.1 =>  (0x00007ffe52bb9000)
> 	libsqlite3.so.0 => /lib64/libsqlite3.so.0 (0x00007f6640589000)
> 	libreadline.so.6 => /lib64/libreadline.so.6 (0x00007f6640342000)
> 	libncurses.so.5 => /lib64/libncurses.so.5 (0x00007f664011b000)
> 	libtinfo.so.5 => /lib64/libtinfo.so.5 (0x00007f663fef1000)
> 	libdl.so.2 => /lib64/libdl.so.2 (0x00007f663fcec000)
> 	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f663fad0000)
> 	libc.so.6 => /lib64/libc.so.6 (0x00007f663f703000)
> 	/lib64/ld-linux-x86-64.so.2 (0x00007f6640856000)
> ```
>
> 