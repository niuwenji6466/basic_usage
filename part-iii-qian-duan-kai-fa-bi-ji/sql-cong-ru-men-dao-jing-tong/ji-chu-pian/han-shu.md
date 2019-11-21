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

### 字符串连接 {#-1}

连接操作用于将两个或者多个字符串拼接到一起，CONCAT 函数是执行连接操作的标准函数：

```sql
SELECT CONCAT('SQL', ' World')
  FROM employee
 WHERE emp_id = 1;

CONCAT('SQL', ' World')|
-----------------------|
SQL World              |

#Oracle 中的 CONCAT 函数一次只能连接两个字符串；其他数据库可以一次连接多个字符串：CONCAT(str1, str2, ...)。
```

CONCAT 函数还有一个扩展形式：CONCAT\_WS，可以指定连接的分隔符：

```sql
-- MySQL、SQL Server 以及 PostgreSQL 实现
SELECT CONCAT_WS('-','S', 'Q', 'L')
  FROM employee
 WHERE emp_id = 1;

CONCAT_WS('-','S', 'Q', 'L')|
----------------------------|
S-Q-L                       |

#该语句使用“-”将多个字符串连接到一起。Oracle 不支持 CONCAT_WS 函数。
```



