### 字符函数

ASCII 函数用于返回字符串中第一个字符的 ASCII 编码，CHR 和 CHAR 函数用于将 ASCII 码转换为对应的字符：

```sql
-- Oracle 和 PostgreSQL 实现
SELECT ASCII('SQL'), CHR(83)
  FROM employee
 WHERE emp_id = 1;

ASCII('SQL')|CHAR(83)|
------------|--------|
          83|S       |

-- MySQL 和 SQL Server 实现
SELECT ASCII('SQL'), CHAR(83)
  FROM employee
 WHERE emp_id = 1;

ASCII('SQL')|CHAR(83)|
------------|--------|
          83|S       |
```

大写字母 S 的 ASCII 编码为 83。Oracle 和 PostgreSQL 实现了 CHR 函数，MySQL 和 SQL Server 实现了 CHAR 函数。

