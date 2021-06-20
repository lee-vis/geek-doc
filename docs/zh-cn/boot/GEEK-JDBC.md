###### 1. 引入

​	对应pom文件，添加如下

```xml
<dependency>
  	<groupId>org.geek</groupId>
  	<artifactId>geek-jdbc</artifactId>
</dependency>
```

###### 2. 使用说明

​	2.1 spring boot启动类Application配置

```java
//开启数据源和事务管理
@EnableTransaction
//配置数据源包路径
@SpringBootApplication(scanBasePackages = {"org.geek.jdbc"}
```

同时配置文件如application-dev.xml添加

```yaml
db:
    default: base
base:
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://81.70.6.18:3306/geek-admin?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
    username: root
    password: rO0ABXciABB2rz6aDft1s74d9VlSE2y9KOVU0AWUgs41zS/zHk9yEA==
    initialSize: 10
test:
    driverClassName: com.mysql.jdbc.Driver
    url: jdbc:mysql://81.70.6.18:3306/geek-webpage?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true
    username: root
    password: rO0ABXciABB2rz6aDft1s74d9VlSE2y9KOVU0AWUgs41zS/zHk9yEA==
    initialSize: 10
```

​	参数说明：db.default为启动默认加载数据库，后续查询默认查询此库

​						数据源base和test配置为两个数据库配置信息，具体参数不做详解

​	2.2 切换数据源

```java
//直接method内动态切换数据源，base为配置文件的数据源名字
DataSourceLookupFactory.setDataSourceLookupKey("test");
//或者method上添加
SelectDataSource("test")
```

​	2.3 事务处理

​		@EnableTransaction会开启全局事务处理，拦截insert,add,save,modify,update,delete,del,get,count,find,list,select开头的方法进行事务管理，隔离和传播级别均为默认，也可以通过@Transactional配置

