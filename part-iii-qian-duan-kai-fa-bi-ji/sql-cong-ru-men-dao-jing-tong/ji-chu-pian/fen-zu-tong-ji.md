### 数据分组

SQL 中的 GROUP BY 子句可以将数据按照某种规则进行分组。以下示例使用 GROUP BY 将员工按照性别进行分组：

```sql
SELECT sex
  FROM employee
 GROUP BY sex;

 sex|
----|
男  |
女  |
```

GROUP BY 将性别的每个不同取值分为一组，每个组返回一条记录。由于员工表中只存在两种不同的性别，返回的结果只有两条记录。该语句与下面 DISTINCT 关键字的效果相同

#### 多字段分组 {#-1}

GROUP BY 也可以基于多个字段或表达式进行分组。以下语句按照部门和性别的组合进行分组：

```sql
SELECT dept_id, sex
  FROM employee
 GROUP BY dept_id, sex;

dept_id| sex|
-------|----|
      1|男  |
      2|男  |
      3|女  |
      4|男  |
      4|女  |
      5|男  |
```

研发部（部门编号为 4）既有男性员工又有女性员工，因此分为 2 个组

将 GROUP BY 子句与聚合函数一起使用可以实现数据的分组汇总功能

### 分组汇总 {#-2}

首先，基于分组字段的不同取值将数据分成 N 个组；然后在组内分别应用聚合函数进行统计，每个组返回一个分析结果。

我们来看一个示例。以下语句按照性别统计员工的数量和平均月薪：

```sql
SELECT sex, COUNT(*), AVG(salary)
  FROM employee
 GROUP BY sex;

sex| COUNT(*)|AVG(salary) |
---|---------|------------|
男  |      22|10145.454545|
女  |       3| 8200.000000|
```

先将员工按照性别分为两个组，然后使用聚合函数分别计算男女员工的总数和平均月薪。该查询的结果显示：男性员工有 22 人，平均月薪约为 10145；女性员工有 3 人，平均月薪为 8200。

我们再来看一个多字段分组统计的示例。以下语句按照部门和职位统计员工的数量和平均月薪，并且按照平均月薪从高到低进行排序：

```sql
SELECT dept_id, job_id, COUNT(*), AVG(salary)
  FROM employee
 GROUP BY dept_id, job_id
 ORDER BY AVG(salary) DESC;
```

#### 基于表达式分组统计 {#-4}

GROUP BY 也可以基于表达式的值进行分组统计。以下示例统计每年入职的员工数量：

```sql
-- Oracle、MySQL 以及 PostgreSQL 实现
SELECT EXTRACT(YEAR FROM hire_date) AS "入职年份",
       COUNT(*) AS "员工数量"
  FROM employee
 GROUP BY EXTRACT(YEAR FROM hire_date)
 ORDER BY EXTRACT(YEAR FROM hire_date);

-- SQL Server 实现
SELECT DATEPART(YEAR, hire_date) AS "入职年份",
       COUNT(*) AS "员工数量"
  FROM employee
 GROUP BY DATEPART(YEAR, hire_date)
 ORDER BY DATEPART(YEAR, hire_date);
```

EXTRACT 函数和 DATEPART 函数用于提取入职日期中的年份信息。

#### 空值分组 {#-5}

对于分组操作而言，同样需要注意空值问题。对于 GROUP BY，如果分组字段中存在多个 NULL 值，它们将被分为一个组

。以下示例按照奖金统计员工的数量：

```sql
SELECT bonus, COUNT(*)
  FROM employee
 GROUP BY bonus;

bonus   |COUNT(*)|
--------|--------|
10000.00|       3|
 8000.00|       1|
  [NULL]|      16|
 5000.00|       2|
 6000.00|       1|
 2000.00|       1|
 1500.00|       1|
```

从查询结果可以看出，16 个员工没有奖金；但是他们都被分组同一个组中，而不是多个不同的组

#### 常见错误 {#-6}

在使用分组汇总时，初学者常见的一个错误就是在 SELECT 列表中使用了既不是聚合函数，也不属于分组字段的字段。例如：

```sql
-- GROUP BY 错误示例
SELECT dept_id, emp_name, AVG(salary)
  FROM employee
 GROUP BY dept_id;
```

以上语句会返回一个错误：字段 emp\_name 没有出现在 GROUP BY 子句或者聚合函数中。原因在于该查询按照部门进行分组，但是每个部门包含多个员工；因此无法确定需要显示哪个员工的姓名。这是一个逻辑上的错误，而不是数据库实现的问题

