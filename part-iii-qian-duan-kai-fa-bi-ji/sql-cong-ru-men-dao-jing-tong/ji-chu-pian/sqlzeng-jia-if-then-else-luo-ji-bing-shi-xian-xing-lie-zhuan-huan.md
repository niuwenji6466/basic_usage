### 简单 CASE 表达式 {#case}

简单 CASE 表达式的语法如下：

```sql
CASE expression
  WHEN value1 THEN result1
  WHEN value2 THEN result2
  ...
  [ELSE default_result]
END
```

首先计算 expression 的值；然后依次与 WHEN 列表中的值（value1，value2，…）进行比较，找到第一个相等的值并返回对应的结果（result1，result2，…）；如果没有找到相等的值，返回 ELSE 中的默认结果；如果此时没有指定 ELSE，返回 NULL 值。

以下示例使用简单 CASE 表达式将员工的部门编号显示为相应的名称：

```sql
SELECT emp_name,
       CASE dept_id
         WHEN 1 THEN '行政管理部'
         WHEN 2 THEN '人力资源部'
         WHEN 3 THEN '财务部'
         WHEN 4 THEN '研发部'
         WHEN 5 THEN '销售部'
         WHEN 6 THEN '保卫部'
         ELSE '其他部门'
       END AS department
  FROM employee;
```

首先判断部门编号是否等于 1，等于就显示为“行政管理部”；否则，如果部门编号等于 2， 显示为“人力资源部”；依次类推；如果部门编号不等于 1 到 6 中的任何值，显示为“其他部门”。

**CASE 表达式的一个常见应用就是实现表的行列转换**

假设存在以下学生成绩表：

```sql
-- 创建成绩表 t_case，sname 为学生姓名，cname 为课程名称，score 为考试成绩
CREATE TABLE t_case(sname varchar(10), cname varchar(10), score int);

-- 插入测试数据
INSERT INTO t_case(sname, cname, score) VALUES ('张三', '语文', 80);
INSERT INTO t_case(sname, cname, score) VALUES ('李四', '语文', 77);
INSERT INTO t_case(sname, cname, score) VALUES ('王五', '语文', 91);
INSERT INTO t_case(sname, cname, score) VALUES ('张三', '数学', 85);
INSERT INTO t_case(sname, cname, score) VALUES ('李四', '数学', 90);
INSERT INTO t_case(sname, cname, score) VALUES ('王五', '数学', 60);
INSERT INTO t_case(sname, cname, score) VALUES ('张三', '英语', 81);
INSERT INTO t_case(sname, cname, score) VALUES ('李四', '英语', 69);
INSERT INTO t_case(sname, cname, score) VALUES ('王五', '英语', 82);
```

执行以上语句创建 t\_case 表并且插入数据。该表中的数据如下：

![](/assets/20190724193652245.png)

每个学生的每科成绩都是一行数据。我们利用 CASE 表达式将其转换为按列显示的形式，最终的结果如下：

![](/assets/20190724193935532.png)

目前的结果还是 9 条记录。然后将每个学生的成绩合并成一条记录，此时需要使用到分组汇总的操作：

```sql
SELECT sname,
       SUM(CASE cname WHEN '语文' THEN score ELSE 0 END) AS "语文",
       SUM(CASE cname WHEN '数学' THEN score ELSE 0 END) AS "数学",
       SUM(CASE cname WHEN '英语' THEN score ELSE 0 END) AS "英语"
  FROM t_case
 GROUP BY sname;
```

在这里我们使用了 SUM 汇总函数和 GROUP BY 分组操作，这些内容是接下来两篇的主题。在此简单说明一下，GROUP BY 按照学生进行分组，这样每个学生最终只有一条记录；同时将学生每门课程的成绩使用 SUM 函数进行求和（每科成绩加上两个 0），还是得到每科的成绩。这样就实现了数据的行转列。

### 搜索 CASE 表达式 {#case-1}

搜索 CASE 表达式的语法如下：

```sql
CASE
  WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ...
  [ELSE default_result]
END
```

按照顺序依次计算每个分支中的条件（condition1，condition2，…），找到第一个结果为真的分支并返回相应的结果（result1，result2，…）；如果没有任何条件为真，返回 ELSE 中的默认结果；如果此时没有指定 ELSE，返回 NULL 值。

**所有的简单 CASE 表达式都可以替换为等价的搜索 CASE 表达式**

我们可以将上一节的示例改写如下：

```sql
SELECT emp_name,
       CASE 
         WHEN dept_id = 1 THEN '行政管理部'
         WHEN dept_id = 2 THEN '人力资源部'
         WHEN dept_id = 3 THEN '财务部'
         WHEN dept_id = 4 THEN '研发部'
         WHEN dept_id = 5 THEN '销售部'
         WHEN dept_id = 6 THEN '保卫部'
         ELSE '其他部门'
       END AS department
  FROM employee;
```

首先判断部门编号等于 1 是否成立（为真），成立就显示为“行政管理部”；否则，判断部门编号等于 2 是否成立， 成立就显示为“人力资源部”；依次类推；如果部门编号不等于 1 到 6 中的任何值，显示为“其他部门”。查询结果参见上文中的简单 CASE 表达式示例。



