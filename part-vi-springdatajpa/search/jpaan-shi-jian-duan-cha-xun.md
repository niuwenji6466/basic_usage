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

这是项目中的示例：

```java
 @Override
    public List<AdviceEntity> serach(String serach, String stime, String etime) {
        List<AdviceEntity> resultList = null;
        Specification<AdviceEntity> querySpecifi = new Specification<AdviceEntity>() {
            @Override
            public Predicate toPredicate(Root<AdviceEntity> root, CriteriaQuery<?> criteriaQuery, CriteriaBuilder cb) {
                List<Predicate> predicates = new ArrayList<>();
                if (StringUtils.isNotBlank(stime)) {
                    //大于或等于传入时间
                    predicates.add(cb.greaterThanOrEqualTo(root.get("commitTime").as(String.class), stime));
                }
                if (StringUtils.isNotBlank(etime)) {
                    //小于或等于传入时间
                    predicates.add(cb.lessThanOrEqualTo(root.get("commitTime").as(String.class), etime));
                }
                if (StringUtils.isNotBlank(serach)) {
                    //模糊查找
                    predicates.add(cb.like(root.get("name").as(String.class), "%" + serach + "%"));
                }
                // and到一起的话所有条件就是且关系，or就是或关系
                return cb.and(predicates.toArray(new Predicate[predicates.size()]));
            }
        };
        resultList = this.adviceDao.findAll(querySpecifi);
        return resultList;
    }
```



