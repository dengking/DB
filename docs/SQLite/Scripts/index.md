# 常用命令

查看数据库中所有的表

```sqlite
select name from sqlite_master where type='table' order by name;
```



```sqlite
sqlite>.tables                                                --查看当前数据库所有表

sqlite>.tables table_name                                     --查看当前数据库指定表

sqlite>.schema                                                --查看当前数据库所有表的建表(CREATE)语句  

sqlite>.schema table_name                                     --查看指定数据表的建表语句  

sqlite>select * from sqlite_master from;                      --查看所有表结构及索引信息  

sqlite>select * from sqlite_master where type='table';        --查看所有表结构信息  

sqlite>select name from sqlite_master where type='table';     --对于表来说，name字段指表名，查询所有表  

sqlite>select * from sqlite_master where type='table' and name='table_name';     --查看指定表结构信息  

sqlite>select * from sqlite_master where type='index';        --查看所有表索引信息，查询所有索引  

sqlite>select name from sqlite_master where type='table';     --对于索引来说，name字段指索引名  

sqlite>select * from sqlite_master where type='index' and name='table_name'; --查看指定表索引信息  

sqlite>pragma table_info ('table_name')                       --查看指定表所有字段信息，类似于msyql：desc table_name  

sqlite>select typeof('column') from table_name;               --查看指定表字段【column】类型，括号内可不输引号  
```





## sqlite group by comma join

### stackoverflow [GROUP BY clause to get comma-separated values in sqlite](https://stackoverflow.com/questions/19332722/group-by-clause-to-get-comma-separated-values-in-sqlite)

[A](https://stackoverflow.com/a/19332782)

This should work:

```sql
SELECT GROUP_CONCAT(eng), hindi FROM enghindi GROUP BY hindi;
```
