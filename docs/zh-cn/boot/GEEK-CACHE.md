###### 1. 引入

​	对应pom文件，添加如下

```xml
<dependency>
  	<groupId>org.geek</groupId>
  	<artifactId>geek-cache</artifactId>
</dependency>
```

###### 2. 使用说明

​	2.1 spring boot启动类Application，配置@EnableCache注解启⽤缓存，通过ActiveCacheType控制缓存类型切换，目前支持redis和ehcache

```java
//启用redis
@EnableCache(mode = ActiveCacheType.REDIS)
//启用ehcache
@EnableCache(mode = ActiveCacheType.EHCACHE)
//两者均启用
@EnableCache(mode = ActiveCacheType.ALL)
//配置缓存包路径
@SpringBootApplication(scanBasePackages = {"org.geek.cache"}
```

​	2.2 启用redis（适合多实例）

```java
//class里面引用
@Autowired
private ICacheService cacheService;
//method内直接使用
//String
cacheService.vSet(key, value, time);
//Hash
cacheService.hmSet(key,hashKey,hashValue);
cacheService.hmSet(key,map);
//List
cacheService.lLeftPushAll(key,list);
cacheService.lRightPop(key);
//无序Set
cacheService.sAdd(key,value);
cacheService.sMembers(key);
//有序set
cacheService.zAdd(key,value,score);
cacheService.rangeByScore(key,score1,score2);
//key模糊匹配 一般禁止使用
cacheService.keys(pattern);
//游标匹配
cacheService.scan(key,count);
cacheService.hscan(key,count);
cacheService.sscan(key,count);
cacheService.zscan(key,count);
//支持发布/订阅
cacheService.convertAndSend(channel,message);
//分布式主键生成
cacheService.generate(key,increment,expireTime);
```

2.3 启用ehcache（适合单实例）

```java
//method上面直接引入下面注解
//添加
@Cacheable(value = "cacheName", key = "#obj.id")
//更新
@CachePut(value = "cacheName", key = "#obj.id")
//删除
@CacheEvict(value = "cacheName", key = "#id")
//查询
@Cacheable(value = "cacheName", key = "#obj.id")
//flush缓存
@CacheEvict(value = "cacheName", allEntries = true)
```

