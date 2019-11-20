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



