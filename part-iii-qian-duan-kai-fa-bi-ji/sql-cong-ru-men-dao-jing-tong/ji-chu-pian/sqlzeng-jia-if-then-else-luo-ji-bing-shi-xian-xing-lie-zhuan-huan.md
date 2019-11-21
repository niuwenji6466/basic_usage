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

