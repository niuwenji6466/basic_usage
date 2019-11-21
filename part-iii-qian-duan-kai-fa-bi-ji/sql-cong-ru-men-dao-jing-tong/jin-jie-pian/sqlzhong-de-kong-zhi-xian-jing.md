### 空值比较与运算 {#-1}

任何值与 NULL 值进行比较时，结果无法确认是真还是假。以下比较运算的结果都是未知：

```
NULL = 0
NULL != 0
NULL > 1
NULL = NULL
NULL != NULL
```

NULL 等于 NULL 的运算结果未知，NULL 不等于 NULL 的运算结果也未知。因此，为了判断某个值是否为空，SQL 提供了 IS NULL 和 IS NOT NULL 谓词：

```sql
SELECT emp_id
  FROM employee
 WHERE NULL = NULL;

emp_id|
------|

SELECT emp_id
  FROM employee
 WHERE NULL IS NULL;

emp_id|
------|
     1|
     2|
     3|
   ...
```

### 空值函数 {#-3}

NULLIF\(expr1, expr2\) 函数接受 2 个参数，如果第一个参数等于第二个参数，返回 NULL；否则，返回第一个参数的值。

```sql
SELECT NULLIF(1, 2),
       NULLIF(3, 3)
  FROM employee
 WHERE emp_id = 1;

NULLIF(1, 2)|NULLIF(1, 1)|
------------|------------|
           1|      [NULL]|

#因为 1 不等于 2，该查询的第一列结果为 1；因为 3 等于 3，第二列结果为 NULL
```

NULLIF 函数的一个常见用途是防止除零错误：

```sql
-- 除零错误
SELECT *
  FROM employee
 WHERE 1 / 0 = 1;

-- 避免除零错误
SELECT *
  FROM employee
 WHERE 1 / NULLIF(0 , 0) = 1;
```

第一个查询中被除数为 0，出现除零错误（MySQL 可能不会提示错误）；第二个查询使用 NULLIF 函数将被除数 0 转换为 NULL，整个结果为 NULL。

COALESCE\(expr1, expr2, expr3, ...\) 函数接受一个参数列表，并且返回第一个非空的参数；如果所有参数都为空，返回 NULL。

以下示例用于查询员工的总收入：

```sql
SELECT emp_name,
       salary*12 + bonus AS "年收入",
       salary*12 + COALESCE(bonus, 0) AS "年收入"
  FROM employee
 WHERE emp_id <= 6;

emp_name|年收入     |年收入    |
--------|---------|---------|
刘备      |370000.00|370000.00|
关羽      |322000.00|322000.00|
张飞      |298000.00|298000.00|
诸葛亮    |296000.00|296000.00|
黄忠      |   [NULL]| 96000.00|
魏延      |   [NULL]| 90000.00|
```

该查询中的第三列利用 COALESCE 函数将奖金为空的数据转换为 0，得到了正确的年收入。

除了SQL 标准中定义的表达式之外，许多数据库还实现了一些类似的扩展函数：

* Oracle 提供了 NVL\(expr1, expr2\) 以及 NVL2\(expr1, expr2, expr3\) 函数
* MySQL 提供了 IFNULL\(expr1, expr2\) 以及 IF\(expr1, expr2, expr3\) 函数
* SQL Server 提供了 ISNULL\(expr1, expr2\) 函数





