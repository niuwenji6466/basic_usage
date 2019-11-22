项目中有需求要按照名称模糊查找和时间段查找数据，可能只有名称没有时间，也可能只有时间没有名称，也可能这几个参数同时匹配，所以要多条件动态查询

JpaSpecificationExecutor 接口提供很多条件查询方法:

```java
public interface JpaSpecificationExecutor<T> {
    T findOne(Specification<T> var1);

    List<T> findAll(Specification<T> var1);

    Page<T> findAll(Specification<T> var1, Pageable var2);

    List<T> findAll(Specification<T> var1, Sort var2);

    long count(Specification<T> var1);

}
```

比如方法:

```java
List<T> findAll(Specification<T> var1);
```



