#### 短路运算 {#-4}

对于逻辑运算符 AND 和 OR，SQL 使用短路运算（short-circuit evaluation）。也就是说，只要前面的表达式能够决定最终的结果，不执行后面的计算。这样能够提高运算效率。因此，以下语句不会产生除零错误

```sql
SELECT 'AND'
  FROM employee
 WHERE 1 = 0 AND 1/0 = 1;

SELECT 'OR'
  FROM employee
 WHERE 1 = 1 OR 1/0 = 1;
```

第一个查询由于 AND 左边的结果为假，肯定不会返回任何结果，因此也就不会计算 1/0；第二个查询由于 OR 左边的结果为真，一定会返回结果，同样不会产生除零错误

#### 运算符优先级 {#-5}

以下是 SQL 中各种条件运算符按照优先级从高到低进行的排列；必要时可以使用圆括号进行调整

* =、!=、&lt;&gt;、&lt;、&lt;=、&gt;、&gt;=
* IS \[NOT\] NULL、\[NOT\] LIKE、\[NOT\] BETWEEN、\[NOT\] IN、\[NOT\] EXISTS
* NOT
* AND
* OR

### 去除重复值 {#-6}





