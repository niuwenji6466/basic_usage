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

除了 CONCAT 函数之外，还可以使用连接符将字符串进行连接：

```sql
-- Oracle 和 PostgreSQL 实现
SELECT 'S' || '-' || 'Q' || '-' || 'L'
  FROM employee
 WHERE emp_id = 1;

'S'||'-'||'Q'||'-'||'L'|
-----------------------|
S-Q-L                  |

-- SQL Server 实现
SELECT 'S' + '-' + 'Q' + '-' + 'L'
  FROM employee
 WHERE emp_id = 1;

     |
-----|
S-Q-L|

#Oracle 和 PostgreSQL 使用 || 连接字符串，SQL Server 使用 + 连接字符串
```

### 字符串长度 {#-3}

字符串的长度可以按照两种方式进行计算：字符数量和字节数量。在多字节编码中，一个字符可能占用多个字节。CHARLENGTH 函数用于计算字符串包含的字符数量，OCTETLENGTH 用于计算字符串包含的字节数量：

```sql
-- MySQL 和 PostgreSQL 实现
SELECT CHAR_LENGTH('数据库'), OCTET_LENGTH('数据库')
  FROM employee
 WHERE emp_id = 1;

CHAR_LENGTH('数据库')|OCTET_LENGTH('数据库')|
------------------|-------------------|
                 3|                  9|

#字符串“数据库”包含 3 个字符，在 UTF-8 编码中占用了 9 个字节。MySQL 和 PostgreSQL 实现了这两个标准的函数。
```

Oracle 中使用 LENGTH 函数和 LENGTHB 函数计算字符数量和字节数量：

```
-- Oracle 实现
SELECT LENGTH('数据库'), LENGTHB('数据库')
  FROM employee
 WHERE emp_id = 1;

LENGTH('数据库')|LENGTHB('数据库')|
-------------|--------------|
            3|             9|

#PostgreSQL 中也可以使用 LENGTH 函数计算字符数量。 MySQL 中的 LENGTH 函数计算的是字节数量。
```

SQL Server 中使用 LEN 函数和 DATALENGTH 函数计算字符数量和字节数量：

```sql
-- SQL Server 实现
SELECT LEN('数据库') AS LEN,
       DATALENGTH('数据库') AS DATALENGTH
  FROM employee
 WHERE emp_id = 1;

LEN|DATALENGTH|
---|----------|
  3|         6|

#字符串“数据库”在“ChinesePRCCIAIWS”字符集中占用 6 个字节，因为每个汉字占用 2 个字节。
```

### 获取子串 {#-4}

SUBSTRING 函数或者 SUBSTR 函数用于返回字符串中的子串：

```
-- MySQL、SQL Server 以及 PostgreSQL 实现
SELECT emp_name, SUBSTRING(emp_name, 1, 1)
  FROM employee
 WHERE emp_id = 1;

-- Oracle、MySQL 以及 PostgreSQL 实现
SELECT emp_name, SUBSTR(emp_name, 1, 1)
  FROM employee
 WHERE emp_id = 1;

 #函数中的第二个参数表示子串的起始位置，第三个参数表示子串的长度。以上查询返回了员工姓名中的第一个字符
 #在 Oracle 和 MySQL 中，起始位置可以是负数，表示从右往左计算起始位置。
```

### 截断字符串 {#-5}

TRIM 函数用于删除字符串开头和结尾的指定字符：

```sql
SELECT TRIM('-' FROM '--S-Q-L--'), TRIM('  S-Q-L  ')
  FROM employee
 WHERE emp_id = 1;

TRIM('-' FROM '--S-Q-L--')|TRIM('  S-Q-L  ')|
--------------------------|-----------------|
S-Q-L                     |S-Q-L            |

#第一个 TRIM 函数删除了两端的“-”；第二个 TRIM 删除了两端的空白字符。
#另外，LTRIM 函数可以用于删除字符串左侧的字符，RTRIM 函数可以用于删除字符串右侧的字符。
```

### 查找与替换 {#-6}

INSTR 函数用于在字符串中查找并返回子串的位置，没有找到时返回 0。REPLACE 函数用于替换字符串中的子串。以下是各种数据库中的具体实现：

```sql
-- Oracle 和 MySQL 实现
SELECT email, INSTR(email, '@'), REPLACE(email, '@', '.')
  FROM employee
 WHERE emp_id = 1;

-- SQL Server 实现
SELECT email, PATINDEX('%@%', email), REPLACE(email, '@', '.')
  FROM employee
 WHERE emp_id = 1;

-- PostgreSQL 实现
SELECT email, POSITION('@' IN email), REPLACE(email, '@', '.')
  FROM employee
 WHERE emp_id = 1;
 # “@”是“liubei@shuguo.com”中的第 7 个字符，INSTR 函数返回了数字 7。
 # SQL Server 使用 PATINDEX 函数查找子串，PostgreSQL 使用 POSITION 函数查找子串。REPLACE 函数将电子邮箱中的“@”替换为“.”
```

### 日期和时间的存储

在数据库中，日期时间类型存在 3 种形式：

* DATE，日期类型，包含年、月、日。可以用于存储出生日期、入职日期等。
* TIME，时间类型，包含时、分、秒，以及小数秒。一般使用较少。

* **TIMESTAMP**，时间戳类型，包含年、月、日、时、分、秒，以及小数秒。用于对时间精度要求比较高的场景，比如存储订单时间





