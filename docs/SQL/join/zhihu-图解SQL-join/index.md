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

