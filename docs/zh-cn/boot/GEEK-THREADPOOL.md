###### 1. 引入

​	对应pom文件，添加如下

```xml
<dependency>
  	<groupId>org.geek</groupId>
  	<artifactId>geek-threadpool</artifactId>
</dependency>
```

###### 2. 使用说明

​	2.1 启动类配置

```java
//配置线程池参数
//corePoolSize为核⼼线程数，maximumPoolSize为最⼤线程数，阻塞队列默认2w，HANDLER拒绝策略，Caller代表池没资源的话，拒绝并抛给调⽤者处理
@PoolConfig(corePoolSize = -1,maximumPoolSize 200,HANDLER = RejectPolicyEnum.Caller)
//配置缓存包路径
@SpringBootApplication(scanBasePackages = {"org.geek.threadpool"}
```

​	2.2 线程池使用

```java
//class内引用
@Resource(name="executorService")
private ExecutorService executorService;
//创建无返回值线程
executorService.execute(() -> {
  log.info("Thread.currentThread().getName(): {}" , Thread.currentThread().getName());
});
//创建带返回结果
Future<?> result = executorService.submit(() -> "success");
log.info("thread result : {}",result.get());
```

