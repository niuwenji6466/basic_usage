### 数据分组

SQL 中的 GROUP BY 子句可以将数据按照某种规则进行分组。以下示例使用 GROUP BY 将员工按照性别进行分组：

```
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



