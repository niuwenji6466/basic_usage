### 连接类型 {#-1}

#### 内连接 {#-2}

内连接（Inner Join）返回两个表中满足连接条件的数据；使用关键字 INNER JOIN 表示，也可以简写成 JOIN。内连接的原理如下图所示（基于两个表的 id 进行等值连接）：

![](/assets/20190729200430638.png)其中，id = 1 和 id = 3 是两个表中匹配的数据，因此内连接返回了这 2 行记录。上一节已经给出了内连接的示例，不再重复。

#### 左外连接 {#-3}

左外连接（Left Outer Join）首先返回左表中所有的数据；对于右表，返回满足连接条件的数据；如果没有相应的数据就返回空值。左外连接使用关键字 LEFT OUTER JOIN 表示，也可以简写成 LEFT JOIN。左外连接的原理如下图所示（基于两个表的 id 进行连接）：

![](/assets/20190729214935586.png)其中，id = 2 的数据在 table1 中存在，在 table2 中不存在；左外连接仍然会返回左表中的该记录，而对于 table2 中的价格（price），返回的是空值。

假设我们想要查看所有的部门及其员工信息。考虑到某些部门可能还没有入职员工，如果使用内连接，则无法显示这些部门；因此使用左外连接：

```sql
SELECT d.dept_id, e.dept_id, d.dept_name, e.emp_name
  FROM department d
  LEFT JOIN employee e ON (e.dept_id = d.dept_id)
 ORDER BY d.dept_id DESC;
```

#### 右外连接 {#-4}

右外连接（Right Outer Join）首先返回右表中所有的数据；对于左表，返回满足连接条件的数据，如果没有相应的数据就返回空值。右外连接使用关键字 RIGHT OUTER JOIN 表示，也可以简写成 RIGHT JOIN。右外连接的原理如下图所示（基于两个表的 id 进行连接）：

![](/assets/20190729220026920.png)其中，id = 5 的数据在 table2 中存在，在 table1 中不存在；右外连接仍然会返回右表中的该记录，而对于 table1 中的名称（name），返回的是空值。简而言之：

table1 RIGHT JOIN table2        等价于： table2 LEFT JOIN table1

#### 全外连接 {#-5}

全外连接（Full Outer Join）等价于左外连接加上右外连接，同时返回左表和右表中所有的数据；对于两个表中不满足连接条件的数据返回空值。全外连接使用关键字 FULL OUTER JOIN 表示，也可以简写成 FULL JOIN 。全外连接的原理如下图所示（基于两个表的 id 进行连接）：

![](/assets/20190729220507741.png)

