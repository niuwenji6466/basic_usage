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

