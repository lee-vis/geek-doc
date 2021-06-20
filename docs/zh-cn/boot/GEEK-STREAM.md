###### 1. 简介

​	基于spring cloud stream，构建⾼度可扩展的事件驱动微服务的框架；可以⽆缝对接kafka和rabbitmq等

​	框架给出rocketmq示例

###### 2. 引入

​	对应pom文件，添加如下

```xml
<dependency>
  	<groupId>org.geek</groupId>
  	<artifactId>geek-stream</artifactId>
</dependency>
```

###### 3. 使用说明

​	生产者示例

​		3.1 Application启动类添加

```java
//启用redis
@EnableBinding(Source.class)
```

​		3.2 application-dev.yml添加

```yaml
spring:
  cloud:
    stream:
      # 绑定mq信息，这里我们绑定的是rabbitmq
      bindings:
        # 定义output，因为我们是消息生产者，需要将消息写到channel中
        output:
          # 使用消息队列名字，在kafka就是topic的名字，然后rabbitmq的话就是Exchange 的名字
          destination: GEEK-STREAM-TOPIC
          # 传输内容的格式，也就是消息的格式，如果是json的话 application/json
          content-type: application/json
      rocketmq:
      	binder:
      		# RocketMQ Namesrv 地址
      		name-server: http://xx.xx.xx.xx:9876
      		# RocketMQ 自定义 Binding 配置项
          bindings:
            output:
              # RocketMQ Producer 配置项，对应 RocketMQProducerProperties 类
              producer:
                group: test # 生产者分组
                sync: true # 是否同步发送消息，默认为 false 异步。

```

​		3.3 发送消息

```java
// <2>创建 Message
GeekMessage message = new GeekMessage().setId(new Random().nextInt());
// <3>创建 Spring Message 对象
Message<GeekMessage> springMessage = MessageBuilder.withPayload(message)
  .build();
// <4>发送消息
return source.output().send(springMessage);
```

​	消费者示例

​		3.4 Application启动类添加

```java
@EnableBinding(Silk.class)
```

​		3.5 application-dev.yml添加

```yaml
spring:
  cloud:
    stream:
      # Binding 配置项
      bindings:
        input:
          destination: GEEK-STREAM-TOPIC # 目的地。这里使用 RocketMQ Topic
          content-type: application/json # 内容格式。这里使用 JSON
          group: geek-consumer-group-GEEK-STREAM-TOPIC # 消费者分组,命名规则：组名+topic名
      # Spring Cloud Stream RocketMQ 配置项
      rocketmq:
        # RocketMQ Binder 配置项
        binder:
          name-server: http://xx.xx.xx.xx:9876 # RocketMQ Namesrv 地址
        # RocketMQ 自定义 Binding 配置项
        bindings:
        	input:
            # RocketMQ Consumer 配置项
            consumer:
            	# 是否开启消费，默认为 true
              enabled: true
              # 是否使用广播消费，默认为 false 使用集群消费，如果要使用广播消费值设成true。
              broadcasting: false 
```

​			3.6 接收消息

```java
@Component
@Slf4j
public class StreamListener{
	@StreamListener(Sink.INPUT)
    public void onMessage(@Payload GeekMessage message) {
        logger.info("[onMessage][线程编号:{} 消息内容：{}]", Thread.currentThread().getId(), message);
    }
}
```



Tips: 

​		name-server为rocketmq服务地址， binder绑定器可以绑定多个input， destination⽬的地相当 于rocketmq的topic主题， sink中intput对应channel消息通道去向， 且 通过 @EnableBinding注解声明
