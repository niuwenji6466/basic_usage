### Top-N 排行榜 {#topn}

实现 Top-N 排行榜的方式主要有两种：

* 标准 SQL 提供的 FETCH 语法

* 另一种常见的 LIMIT 语法

使用标准 SQL 实现 Top-N 排行榜

```sql
-- Oracle、SQL Server 以及 PostgreSQL 实现
SELECT emp_name, salary
  FROM employee
 ORDER BY salary DESC
OFFSET 0 ROWS
 FETCH FIRST 5 ROWS ONLY;
```

其中，ORDER BY 按照月薪从高到低进行排序；OFFSET 表示跳过 0 行；然后 FETCH 返回前 5 条数据，Oracle、SQL Server 以及 PostgreSQL 支持这种 FETCH 语法。Oracle 和 PostgreSQL 中的 OFFSET 可以省略，同样表示跳过 0 行数据

