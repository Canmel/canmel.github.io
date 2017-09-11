---
layout: post
title: "spring boot 事件"
description: ""
category: 
tags: [java spring-boot spring]
---

## 概念

>　Spring Event为bean和bean之间通信提供了支持,当一个bean处理完一个,另一个bean做出相应的处理

## 流程

1. 自定义事件, 继承 ApplicationEvent
2. 定义事件监听,实现 ApplicationListener
3. 使用容器发布

## 代码

### 定义事件

```java
public class DemoEvent extends ApplicationContextEvent {

	private static final long serialVersionUID = 1L;
	
	private String msg;
	
	public DemoEvent(ApplicationContext source, String msg) {
		super(source);
		this.msg = msg;
	}

	public String getMsg() {
		return msg;
	}

	public void setMsg(String msg) {
		this.msg = msg;
	}
}
```

### 定义监听

```java
@Component
public class DemoListener implements ApplicationListener<DemoEvent>{

	@Override
	public void onApplicationEvent(DemoEvent event) {
		String msg = event.getMsg();
		System.out.println("我(bean-demo-listener)接收到了 bean-demoPublisher 发布的消息:　" + msg);
	}
}
```

### 使用容器发布

```java
@Component
public class DemoPublisher {
	@Autowired
	ApplicationContext applicationContext;
	
	public void publish(String msg) {
		applicationContext.publishEvent(new DemoEvent(applicationContext, msg));
	}
}
```

### 运行

```java
public static void main( String[] args ) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(EnventConfig.class);
        DemoPublisher demoPublisher = context.getBean(DemoPublisher.class);
        demoPublisher.publish("我在App-class发送一条消息");
        context.close();
    }
```

### 解释

1. `使用容器发布`注入容器;
2. 容器中发布新建的事件`DemoEvent`;
3. `事件监听`,实现`ApplicationListener`,重写`onApplicationEvent(DemoEvent event)`;
4. 事件监听业务逻辑就在`onApplicationEvent`中


{% include JB/setup %}