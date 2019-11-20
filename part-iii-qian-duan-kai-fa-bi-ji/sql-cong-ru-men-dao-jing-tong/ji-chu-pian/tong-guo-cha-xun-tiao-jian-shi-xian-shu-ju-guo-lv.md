#### 短路运算 {#-4}

对于逻辑运算符 AND 和 OR，SQL 使用短路运算（short-circuit evaluation）。也就是说，只要前面的表达式能够决定最终的结果，不执行后面的计算。这样能够提高运算效率。因此，以下语句不会产生除零错误

```sql
SELECT 'AND'
  FROM employee
 WHERE 1 = 0 AND 1/0 = 1;

SELECT 'OR'
  FROM employee
 WHERE 1 = 1 OR 1/0 = 1;
```

第一个查询由于 AND 左边的结果为假，肯定不会返回任何结果，因此也就不会计算 1/0；第二个查询由于 OR 左边的结果为真，一定会返回结果，同样不会产生除零错误

#### 运算符优先级 {#-5}

以下是 SQL 中各种条件运算符按照优先级从高到低进行的排列；必要时可以使用圆括号进行调整

* =、!=、&lt;&gt;、&lt;、&lt;=、&gt;、&gt;=
* IS \[NOT\] NULL、\[NOT\] LIKE、\[NOT\] BETWEEN、\[NOT\] IN、\[NOT\] EXISTS
* NOT
* AND
* OR

### 去除重复值 {#-6}

SQL 使用 DISTINCT 关键字去除查询结果中的重复数据。例如，以下查询返回了员工表所有可能的性别：

```sql
SELECT DISTINCT sex  FROM employee;
```

首先，DISTINCT 位于 SELECT 之后而不是像其他过滤条件一样位于 WHERE 之后；其次，查询结果中重复的记录只会出现一次。与 DISTINCT 相反的是 ALL，用于返回不去重的结果。我们通常不需要加上 ALL 关键字，因为它是默认的行为。另外，为了消除重复值，数据库系统需要对结果进行排序，然后扫描重复值；因此，大量数据的重复值处理可能会降低查询的速度。

### 正则表达式 {#-2}

Oracle 和 MySQL 支持类似的正则表达式函数：

```sql
-- MySQL 实现
SELECT email
  FROM t_regexp
 WHERE REGEXP_LIKE(email, '^[a-zA-Z0-9]+[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}$');

email                     |
--------------------------|
TEST@shuguo.com           |
123.test@shuguo-sanguo.org|

-- Oracle 实现
SELECT email
  FROM t_regexp
 WHERE REGEXP_LIKE(email, '^[a-zA-Z0-9]+[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$');

EMAIL                     |
--------------------------|
TEST@shuguo.com           |
123.test@shuguo-sanguo.org|
```

### 多列排序 {#-1}

多列排序是指基于多个字段或表达式的排序，使用逗号进行分隔

```sql
SELECT col1, col2, ...
  FROM t
 ORDER BY col1 ASC, col2 DESC, ...;
 
#首先基于第一个字段进行排序；对于第一个字段排序相同的数据，再基于第二个字段进行排序；依此类推

SELECT emp_name, salary, hire_date
  FROM employee
 WHERE dept_id = 4
 ORDER BY salary DESC, hire_date;

```



