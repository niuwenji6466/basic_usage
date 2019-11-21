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





