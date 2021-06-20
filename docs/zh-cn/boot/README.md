###### 1. 简介

​	类似于javaweb开发脚手架，根据多年开发经验提取出的平时开发项目常用到的技术组件，提供封装的公共api和公共技术组件可以直接使用，减少非业务代码开发，缩短项目开发周期

###### 2. 引入

​	项目父pom引入geek-boot依赖，parent或者import均可

​	

```xml
<parent>
    <groupId>org.geek</groupId>
    <artifactId>geek-boot</artifactId>
    <version>1.0-SNAPSHOT</version>
    <relativePath>../geek-boot</relativePath>
</parent>
<!-- 或者 -->
<dependency>
    <groupId>org.geek</groupId>
    <artifactId>geek-boot</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <type>pom</type>
    <scope>import</scope>
</dependency
```

