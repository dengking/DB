# zhihu [图解SQL的inner join、left /right join、 outer join区别](https://zhuanlan.zhihu.com/p/59656673)

> NOTE: 
>
> 一、作者参考的文章: 
>
> codinghorror [A Visual Explanation of SQL Joins](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)
>
> 二、分类
>
> 1、inner join
>
> 交集
>
> 2、outer join: 
>
> 并集
>
> **FULL [OUTER] JOIN**
>
> **LEFT [OUTER] JOIN**
>
> **RIGHT [OUTER] JOIN**
>
> 

Coding Horror上有一篇文章,通过文氏图 Venn diagrams 解释了SQL的Join。我觉得清楚易懂，转过来。

假设我们有两张表。Table A 是左边的表。Table B 是右边的表。其各有四条记录，其中有两条记录name是相同的，如下所示：让我们看看不同JOIN的不同

![img](https://pic3.zhimg.com/80/v2-ee850fa0f5308b868c1a2d2d0a0ec7e2_720w.jpg)

## 1、INNER JOIN

```SQL
SELECT * FROM TableA INNER JOIN TableB ON TableA.name = TableB.name
```

![img](https://pic3.zhimg.com/80/v2-91e09e3eda12839df934dda8aa5368ee_720w.jpg)

## 2、FULL [OUTER] JOIN

### (1) 并集

```SQL
SELECT * FROM TableA FULL OUTER JOIN TableB ON TableA.name = TableB.name
```



![img](https://pic1.zhimg.com/80/v2-cd6c8efb1d1a8b3c19799cb86140c1d4_720w.jpg)

### (2) 没有交集的数据集合

![img](https://pic3.zhimg.com/80/v2-169e32f93f651cc28af28bb93e343b02_720w.jpg)

## 3、Left outer join



![img](https://pic1.zhimg.com/80/v2-8d1e6fabd728209a0487ee3269531134_720w.jpg)

## 4、RIGHT [OUTER] JOIN

RIGHT OUTERJOIN 是后面的表为基础，与LEFT OUTER JOIN用法类似。这里不介绍了。

## 5、UNION 与 UNION ALL

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。UNION 只选取记录，而UNION ALL会列出所有记录。

### (1)

```sql
SELECT name FROM TableA UNION SELECT name FROM TableB
```

![img](https://pic4.zhimg.com/80/v2-de43d8c5e5301be5465395465d42c3e7_720w.jpg)


选取不同值

### (2)

```sql
SELECT name FROM TableA UNION ALL SELECT name FROM TableB
```

![img](https://pic4.zhimg.com/80/v2-07c0bb377ee13fc15b596852a3036313_720w.jpg)


全部列出来

### (3)

```sql
SELECT * FROM TableA UNION SELECT * FROM TableB
```

![img](https://pic1.zhimg.com/80/v2-56c01a130190c4cc5ef9ceba7c53424c_720w.jpg)

由于 `id 1 Pirate` 与 `id 2 Pirate` 并不相同，不合并

还需要注册的是我们还有一个是“交差集” cross join, 这种Join没有办法用文式图表示，因为其就是把表A和表B的数据进行一个`N*M`的组合，即笛卡尔积。表达式如下：

```sql
SELECT * FROM TableA CROSS JOIN TableB
```

这个笛卡尔乘积会产生 4 x 4 = 16 条记录，一般来说，我们很少用到这个语法。但是我们得小心，如果不是使用嵌套的select语句，一般系统都会产生笛卡尔乘积然再做过滤。这是对于性能来说是非常危险的，尤其是表很大的时候。



![img](https://pic2.zhimg.com/80/v2-35324035b93e1a6ba9ccf9d15a299add_720w.jpg)
