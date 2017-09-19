---
layout: post
title: "Spring boot 计划任务"
description: ""
category: 
tags: [Java, Spring, Spring-boot]
---

## 用法

> 配置类注解@EnableScheduling 开启对计划任务的支持,然后在要执行的计划任务方法上添加注解@Scheduled,声明这是一个计划任务

**`@Scheduled`支持多种类型的计划任务,包含`cron` `fixDelay` `fixRate`等**

## 代码

### 计划任务执行类

```java
@Service
public class ScheduledTaskService {

	private static final SimpleDateFormat dateFormat = new SimpleDateFormat("HH:mm:ss");
	
	// @Scheduled 设置计划任务执行方式
	@Scheduled(fixedRate = 1000)
	public void reportCurrentTime() {
		System.out.println("每隔1秒执行一次 " + dateFormat.format(new Date()));
	}
	
	@Scheduled(cron = "0 0 13 ? * *")
	public void fixTimeExecution() {
		System.out.println("在指定时间 " + dateFormat.format(new Date()) + " 执行");

	}
}
```

### 配置类

>  @EnableScheduling 开启对注解类的支持

```java
@Configuration
@ComponentScan("com.meedesidy.test_spring4.schedu")
@EnableScheduling
public class TaskSchedulerConfig {
	
}
```

{% include JB/setup %}
