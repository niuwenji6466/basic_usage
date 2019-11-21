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

### 日期时间函数 {#-1}

#### 返回当前日期时间 {#-2}

CURRENT\_DATE、CURRENT\_TIME 以及 CURRENT\_TIMESTAMP 函数分别返回数据库系统当前的日期、时间以及时间戳。以下是不同数据库中的实现示例：

```sql
-- MySQL 和 PostgreSQL 实现
SELECT CURRENT_DATE, CURRENT_TIME, CURRENT_TIMESTAMP
  FROM employee
 WHERE emp_id = 1;

CURRENT_DATE|CURRENT_TIME|CURRENT_TIMESTAMP  |
------------|------------|-------------------|
  2019-10-23|    11:29:55|2019-10-23 11:29:55|

-- Oracle 实现
SELECT CURRENT_DATE, CURRENT_TIMESTAMP
  FROM employee
 WHERE emp_id = 1;

CURRENT_DATE       |CURRENT_TIMESTAMP  |
-------------------|-------------------|
2019-10-23 11:31:58|2019-10-23 11:31:58|

-- SQL Server 实现
SELECT CAST(GETDATE() AS DATE), CAST(GETDATE() AS TIME), CURRENT_TIMESTAMP
  FROM employee
 WHERE emp_id = 1;

          |        |                   |
----------|--------|-------------------|
2019-10-23|11:35:47|2019-10-23 11:35:47|
```

除此之外，各种数据库还提供了一些扩展的函数：

* Oracle 支持 SYSDATE、SYSTIMESTAMP 获取当前日期和时间戳
* MySQL 支持 CURDATE\(\)、CURRENTDATE\(\) 获取当前日期，CURTIME\(\)、CURRENTTIME\(\) 获取当前时间，NOW\(\)、CURRENT\_TIMESTAMP\(\) 获取当前时间戳

#### 提取日期时间信息 {#-3}

EXTRACT\(part FROM dt\) 函数可以返回日期时间中的某个部分，例如年、月、小时等。以下示例查找 2018 年入职的员工：

```sql
-- Oracle、MySQL 以及 PostgreSQL 实现
SELECT emp_name, hire_date
  FROM employee
 WHERE EXTRACT(YEAR FROM hire_date) = 2018;

 #EXTRACT 函数中的 YEAR 表示提取年份信息
```

SQL Server 使用 DATEPART\(part, dt\) 函数实现相同的功能：

```
-- SQL Server 实现
SELECT emp_name, hire_date
  FROM employee
 WHERE DATEPART(YEAR, hire_date) = 2018;
```

EXTRACT 和 DATEPART 函数可以支持更多的选项，例如 MONTH、DAY、WEEK、HOUR、MINUTE、SECOND 等。每个数据库支持的选项可能不同，使用时可以参考数据库文档。

#### 日期和时间的数学运算 {#-4}

日期和时间的运算主要包括两个日期相减以及一个日期加/减一个时间间隔。以下示例计算员工从入职到当前日期之间的天数：

```sql
-- Oracle 以及 PostgreSQL 实现
SELECT emp_name,hire_date, CURRENT_DATE, CURRENT_DATE - hire_date AS days
  FROM employee;

#Oracle 以及 PostgreSQL 中两个日期相减就可以得到它们之间相差的天数
```

MySQL 和 SQL Server 使用 DATEDIFF 函数计算两个日期之间的间隔：

```sql
-- MySQL 实现
SELECT emp_name,hire_date, CURRENT_DATE, DATEDIFF(CURRENT_DATE, hire_date) AS days
  FROM employee;

-- SQL Server 实现
SELECT emp_name,hire_date, GETDATE(), DATEDIFF(DAY, hire_date, GETDATE()) AS days
  FROM employee;

  # MySQL 中的 DATEDIFF 函数返回第一个日期减去第二个日期的天数。SQL Server 中的 DATEDIFF 函数接受三个参数，
  # 可以返回第二个日期减去第一个日期的天数（DAY）、月数（MONTH）或者年数（YEAR）等。
```

另外，日期时间加上/减去一个时间间隔，可以得到一个新的日期时间。以下示例用于计算员工入职一周年的纪念日：

```
-- Oracle、MySQL 以及 PostgreSQL 实现
SELECT hire_date, hire_date + INTERVAL '1' YEAR AS anniversary
  FROM employee;

-- SQL Server 实现
SELECT hire_date, DATEADD(YEAR, 1, hire_date) AS anniversary
  FROM employee;

  #INTERVAL 表示一段时间，例如 5 分钟（MINUTE）、1 个月（MONTH）等。SQL Server 使用 DATEADD 函数为日期增加一段时间
```

### 类型转换函数 {#-5}

CAST\(expr AS type\) 函数用于将数据转换为不同的类型。以下是一个类型转换的示例：

```sql
-- Oracle 实现
-- 修改日期显示格式
ALTER SESSION SET nls_date_format = 'yyyy-mm-dd';
SELECT CAST('666' AS INTEGER), CAST(hire_date AS CHAR(10))
  FROM employee
 WHERE emp_id = 1;

-- SQL Server 和 PostgreSQL 实现
SELECT CAST('666' AS INTEGER), CAST(hire_date AS CHAR(10))
  FROM employee
 WHERE emp_id = 1;

-- MySQL 实现
SELECT CAST('666' AS SIGNED INTEGER), CAST(hire_date AS CHAR(10))
  FROM employee
 WHERE emp_id = 1;
```

类型转换可能导致精度的丢失，并且 CAST 函数在各种数据库中支持的转换类型取决于数据库的实现。

除了明确指定的类型转换之外，数据库可能在执行某些操作时尝试隐式的类型转换。例如以下语句：

```sql
SELECT '666' + 123, CONCAT('Hire Date: ', hire_date)
  FROM employee
 WHERE emp_id = 1;

'666' + 123|CONCAT('Date: ', hire_date)|
-----------|---------------------------|
        789|Hire Date: 2000-01-01      |
```

该查询中存在 2 个隐式类型转换。第一个转换将字符串“666”转换为数字 666，第二个转换将日期类型的 hire\_date 转换为字符串。

### 使用 COUNT 函数统计数量 {#count}

COUNT\(\*\) 函数用于统计行数。以下示例统计员工的数量：

```sql
SELECT COUNT(*) AS "员工数量"
  FROM employee;

员工数量
------|
    25|
```

使用聚合函数时需要注意两点：

* 在聚合函数的参数中加上 DISTINCT 关键字，可以在计算之前排除重复值

* 聚合函数在计算时，忽略输入值为 NULL 的数据行；COUNT\(\*\) 除外

我们先来看一下 COUNT 函数加上 DISTINCT 之后的效果：

```sql
SELECT COUNT(sex), COUNT(DISTINCT sex)
  FROM employee;

COUNT(sex)|COUNT(DISTINCT sex)|
----------|-------------------|
        25|                  2|
```



