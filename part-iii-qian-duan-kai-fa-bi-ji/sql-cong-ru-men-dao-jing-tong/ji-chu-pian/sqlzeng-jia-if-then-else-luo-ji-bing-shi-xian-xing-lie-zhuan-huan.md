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



