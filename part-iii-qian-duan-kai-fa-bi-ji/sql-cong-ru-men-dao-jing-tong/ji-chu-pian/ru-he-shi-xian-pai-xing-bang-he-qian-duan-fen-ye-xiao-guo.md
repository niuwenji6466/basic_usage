### Top-N 排行榜 {#topn}

实现 Top-N 排行榜的方式主要有两种：

* 标准 SQL 提供的 FETCH 语法

* 另一种常见的 LIMIT 语法

使用标准 SQL 实现 Top-N 排行榜：

```sql
-- Oracle、SQL Server 以及 PostgreSQL 实现
SELECT emp_name, salary
  FROM employee
 ORDER BY salary DESC
OFFSET 0 ROWS
 FETCH FIRST 5 ROWS ONLY;
```

其中，ORDER BY 按照月薪从高到低进行排序；OFFSET 表示跳过 0 行；然后 FETCH 返回前 5 条数据，Oracle、SQL Server 以及 PostgreSQL 支持这种 FETCH 语法。Oracle 和 PostgreSQL 中的 OFFSET 可以省略，同样表示跳过 0 行数据

使用 LIMIT 实现 Top-N 排行榜：

MySQL 和 PostgreSQL 支持使用 LIMIT 替代 FETCH 实现相同的功能

```sql
-- MySQL 以及 PostgreSQL 实现
SELECT emp_name, salary
  FROM employee
 ORDER BY salary DESC
 LIMIT 5 OFFSET 0;
```

其中，ORDER BY 按照月薪从高到低进行排序；OFFSET 表示跳过 0 行，LIMIT 返回前 5 条数据，也就是月薪 Top-5 的员工。另外，OFFSET 可以省略，同样表示跳过 0 行数据。该语句的结果与上面的 FETCH 示例相同

### 分页查询

**使用标准 SQL 实现分页查询：**

假设前端页面每次显示 5 条记录；当用户点击按钮显示第 3 页的数据，也就是第 11 条到第 15 条记录时，使用 FETCH 子句实现如下：

```sql
-- Oracle、SQL Server 以及 PostgreSQL 实现
SELECT emp_name, salary
  FROM employee
 ORDER BY salary DESC
OFFSET 10 ROWS
 FETCH FIRST 5 ROWS ONLY;
```

其中，ORDER BY 按照月薪从高到低进行排序；OFFSET 跳过 10 行；然后 FETCH 返回随后的 5 条数据。

通过这种分页查询的方式，还可以实现其他功能。例如，以下语句可以找出哪些员工的月薪排名第 3 高：

```sql
-- Oracle、SQL Server 以及 PostgreSQL 实现
SELECT emp_name, salary
  FROM employee
 WHERE salary = (SELECT salary
                   FROM employee
                  ORDER BY salary DESC
                 OFFSET 2 ROWS
                  FETCH FIRST 1 ROWS ONLY);
```

首先，括号中是一个子查询，返回了第 3 高的月薪值（24000）；然后外部查询返回了月薪等于该值的所有员工。在这里我们可以将它看作一个常量 24000

完整的 FETCH 语法：

```
[OFFSET M {ROW | ROWS}]
FETCH { FIRST | NEXT } [ num_rows | N PERCENT ] { ROW | ROWS } { ONLY | WITH TIES };
```

其中，方括号（\[ \]）表示可选项；大括号（{ }）是必选项，竖线（\|）表示可以二选一。每个参数的作用如下：

* OFFSET 表示偏移量，即从第 M+1 行开始返回；如果不指定，表示从第 1 行开始返回；ROW 和 ROWS 作用相同
* FETCH 指定返回多少行，FIRST 和 NEXT 作用相同
* num\_rows 表示按照行数计算返回的数据量，N PERCENT 表示按照百分比计算返回的数据量，ROW 和 ROWS 作用相同

* ONLY 和 WITH TIES 的差别在于，如果在最后有多个排名相同的数据行，WITH TIES 会返回更多的数据；默认为 ONLY

#### 使用 LIMIT 实现分页查询 {#limit}



