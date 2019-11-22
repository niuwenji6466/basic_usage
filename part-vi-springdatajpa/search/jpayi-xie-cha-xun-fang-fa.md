1. 普通查询，根据字段查询结果集

```java
/**
 * 根据订单序列号查询订单
 *
 * @param serialNumber 序列号
 * @return 订单
 */
Orders findBySerialNumber(String serialNumber);
```

1. 带参数hql语句查询

```java
/**
 * 根据 publicOpenid 查询 serialNumber集合 
 * @param publicOpenid  
 * @return 
 */
@Query("select o.serialNumber  from Orders o where o.publicOpenid = ?1 order by o.createTime desc ")
List<String> findSerialNumberByPublicOpenId(String publicOpenid);
```

1. 聚合函数
2. 原生sql查询部分字段
3. 参数是集合的



