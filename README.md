# Test Hibernate Second Level Cache

## Enable second level cache in Spring Boot
```
spring.jpa.properties.hibernate.cache.use_second_level_cache=true
spring.jpa.properties.hibernate.cache.use_query_cache=true
spring.jpa.properties.hibernate.cache.region.factory_class=org.hibernate.cache.ehcache.EhCacheRegionFactory
```

## Test Case
Blog refer to Author as one of its property. By getting blog list, the authors referred by blogs should be loaded from cache, rather than a second database query.

### TestController.java
```Java
@GetMapping("/blogs")
public List<Blog> get(){
    return blogRepo.findAll();
}
```

### Test Results
```
Hibernate: select blog0_.id as id1_1_, blog0_.author_id as author_i4_1_, blog0_.content as content2_1_, blog0_.title as title3_1_ from blog blog0_
Hibernate: select author0_.id as id1_0_0_, author0_.gender as gender2_0_0_, author0_.name as name3_0_0_ from author author0_ where author0_.id=?
Hibernate: select blog0_.id as id1_1_, blog0_.author_id as author_i4_1_, blog0_.content as content2_1_, blog0_.title as title3_1_ from blog blog0_
Hibernate: select blog0_.id as id1_1_, blog0_.author_id as author_i4_1_, blog0_.content as content2_1_, blog0_.title as title3_1_ from blog blog0_
Hibernate: select blog0_.id as id1_1_, blog0_.author_id as author_i4_1_, blog0_.content as content2_1_, blog0_.title as title3_1_ from blog blog0_
Hibernate: select blog0_.id as id1_1_, blog0_.author_id as author_i4_1_, blog0_.content as content2_1_, blog0_.title as title3_1_ from blog blog0_
```

## Conclusions
Reduce database hits by enabling second level cache, especially in 1+N selection case.

## Use Redis as Distributed Cache Provider
