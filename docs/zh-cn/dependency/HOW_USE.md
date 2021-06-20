1. ###### 前提项目需使用maven管理，在当前项目根pom下加入如下代码：

```xml
<parent>
  <groupId>org.geek</groupId>
  <artifactId>geek-dependency-management</artifactId>
  <version>1.0-SNAPSHOT</version>
</parent>
```



2. ###### 如果不想使用父pom方式引入，也可以使用import方式

```xml
<dependency>
  <groupId>org.geek</groupId>
  <artifactId>geek-dependency-management</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <type>pom</type>
  <scope>import</scope>
</dependency
```



3. ###### 在具体业务逻辑开发模块，可直接引用实际jar组件，如下

```xml
<!-- 图形验证码 -->
<dependency>
  <groupId>com.github.whvcse</groupId>
  <artifactId>easy-captcha</artifactId>
</dependency>
```

