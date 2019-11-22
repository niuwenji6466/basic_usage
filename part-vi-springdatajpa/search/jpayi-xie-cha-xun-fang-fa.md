1.普通查询，根据字段查询结果集

```java
/**
 * 根据订单序列号查询订单
 *
 * @param serialNumber 序列号
 * @return 订单
 */
Orders findBySerialNumber(String serialNumber);
```

2.带参数hql语句查询

```java
/**
 * 根据 publicOpenid 查询 serialNumber集合 
 * @param publicOpenid 
 * @return 
 */
@Query("select o.serialNumber  from Orders o where o.publicOpenid = ?1 order by o.createTime desc ")
List<String> findSerialNumberByPublicOpenId(String publicOpenid);
```

3.聚合函数

```java
/**
 *查询驾照某一时间段的扣分总数 
 * @param date1 开始日期 
 * @param date2 截止日期 
 * @return 扣分总数 
 */
@Query("select sum(dockPoints) from Orders o where o.createTime between ?1 and ?2  and o.number=?3 and o.type=?4")
Integer findDockPointsByTime(Date date1,Date date2,String number,String type);
```

4.原生sql查询部分字段

```java
/**
 *驾照某一时间段处理的车辆信息 
 * 
 * @param date1 开始时间 
 * @param date2 结束时间 
 * @param number 证件编号 
 * @param type 证件类型 
 * @return  处理过的车辆信息 */
@Query(value="select distinct(o.plate_number),(o.plate_number_type) from winstar_orders o where o.create_time between ?1 and ?2  and o.number=?3 and o.type=?4", nativeQuery = true)
List<Object[]> findPlateNumberAndPlateNumberTypeByCertificate(Date date1, Date date2, String number, String type);
```



