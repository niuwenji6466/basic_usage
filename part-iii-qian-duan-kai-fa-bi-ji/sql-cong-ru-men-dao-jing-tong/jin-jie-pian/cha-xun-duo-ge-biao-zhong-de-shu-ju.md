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

![](/assets/20190729220507741.png)结果中包含了所有的 id，然后对于两个表中不满足连接条件的数据（id = 2 和 id = 5），分别在相应的字段中返回了空值。

假如我们想要查看所有的部门和员工信息。同时考虑到某些部门可能还没有员工，而且某些员工可能还没有分配部门，可以使用全外连接：

```sql
-- Oracle、SQL Server 以及 PostgreSQL 实现
SELECT d.dept_id, e.dept_id, d.dept_name, e.emp_name
  FROM department d
  FULL JOIN employee e ON (e.dept_id = d.dept_id);
```

MySQL不支持全外连接。 ANSI SQL/86 标准语法不支持全外连接。

#### 自然连接 {#-7}

对于连接查询，如果满足以下条件，可以使用 USING 替代 ON 简化连接条件的输入：

* 连接条件是等值连接

* 两个表中的连接字段必须名称相同，类型也相同

针对上文中的内连接查询示例，可以使用 USING 简化如下：

```sql
-- Oracle、MySQL 以及 PostgreSQL 实现
SELECT dept_id, d.dept_name, e.emp_name
  FROM employee e
  JOIN department d USING (dept_id);
```

其中，USING 表示使用两个表中的公共字段（dept\_id）进行等值连接。查询语句中的公共字段不需要添加表名限定。该语句的结果与上文中的内连接查询示例相同。

#### 自连接 {#-8}

自连接（Self Join）是指连接操作符的两边都是同一个表，也就是将一个表和它自己进行连接。自连接本质上并没有什么特殊之处，主要用于处理那些对自己进行了外键引用的表。

例如，员工表中的经理字段（manager）是一个外键列，引用了员工表自身的编号字段（emp\_id）。如果要显示员工姓名以及他们经理的姓名，可以通过自连接实现：

```sql
SELECT e.emp_name AS "员工姓名",
       m.emp_name AS "经理姓名"
  FROM employee e
  LEFT JOIN employee m ON (m.emp_id = e.manager)
 WHERE e.dept_id = 1
 ORDER BY e.emp_id;

员工姓名|经理姓名|
-------|--------|
刘备   |[NULL]  |
关羽   |刘备    |
张飞   |刘备    |
```

该查询使用自连接关联了 2 次员工表，一个用于代表员工（e），另一个用于代表经理（m）；连接条件是经理的员工编号等于员工的经理编号。这种情况下，必须使用表别名才能区分两个员工表。该查询使用了左外连接，因为“刘备”是该公司的老板，他没有上级

