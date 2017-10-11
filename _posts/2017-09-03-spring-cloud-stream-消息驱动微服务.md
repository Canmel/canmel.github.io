---
layout: post
title: "spring cloud stream 消息驱动微服务"
description: ""
category: 
tags: [Spring-cloud, Spring, Java]
---
{% include JB/setup %}


## 依赖

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-stream-rabbit</artifactId>
</dependency>
```

---

## 接收消息

### 代码
> SinkReceiver.class

```java
/**
* @EnableBinding 指定一个或多个定义了@Input或@Output注解的接口
* 以此实现对消息通道(Channel)的绑定
* Sink.class 是Spring Cloud Stream 默认实现对输入消息绑定的定义
* @Input或@Output注释的会自动加入spring容器，可以直接注入
*/
@EnableBinding(value = {Sink.class})
public class SinkReceiver {
	
	private static Logger logger =                           LoggerFactory.getLogger(MeedStreamApplication.class);
     
     /**
     * @StreamListener被注释的方法注册为消息中心中间件上数据流的时间监听器
     * 注解中的属性值对应监听的消息通道名
     */
	@StreamListener(Sink.INPUT)
	public void receiver(Object payload ) {
		logger.info("Received : " + payload);
	}
}
```

### 配置

```
＃＃ 对应`RabbitMQ` 上`exchanges(消息交换机)`的name, 这里将设置为`test`　
spring.cloud.stream.bindings.sendoutput.destination=test

```

---

## 发送消息

### 代码

```java
@EnableBinding(value = {Sender.class})
public class SinkSender {
	private static Logger logger = LoggerFactory.getLogger(SinkSender.class);

    @Bean
	@InboundChannelAdapter(value = Sender.SENDER, poller = @Poller(fixedDelay = "2000"))
	public MessageSource<String> timerMessageSource(){
		return () -> new GenericMessage<>("xxxxxxx2秒调用一次");
	}
}	

```

---

### 配置

```
＃＃ 对应`RabbitMQ` 上`exchanges(消息交换机)`的name, 这里将设置为`test`
＃＃ 可以不进行设置，但要使得消息交换机名称和之前的`test`一致才能接收消息，
＃＃ 两种方式:
＃＃ １．@StreamListener `参数` 与 前面的`test`一致
＃＃ ２．设置如下参数改变当前要接入的消息交换机`name`
spring.cloud.stream.bindings.receiverinput.destination=test

```

**@EnableBinding : 绑定消息通道（channel）**

**@StreamListener : 注册监听**
