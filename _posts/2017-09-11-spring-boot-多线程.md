---
layout: post
title: "spring boot 多线程"
description: ""
category: 
tags: [java, spring-boot, spring]
---

> Spring 通过任务执行器（TaskExecutor）实现多线程和并发编程，使用`ThreadPoolTaskExecutor`可实现一个基于线程池`TaskExecutor`
> 通过`＠EnableAsync`开启对异步任务的支持，并通过在实际执行的`Bean`的方法中使用`＠Async`注解其是一个`异步任务`

## 代码

### 配置类

```java
@Configuration
@ComponentScan("com.meedesidy.test_spring4.executor")
@EnableAsync
// 实现AsyncConfigurer 重写getAsyncExecutor, getAsyncUncaughtExceptionHandler
public class TaskExecutorConfig implements AsyncConfigurer{
    
    //配置一个基于线程池的Executor
	@Override
	public Executor getAsyncExecutor() {
		ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
		executor.setCorePoolSize(5);
		executor.setMaxPoolSize(10);
		executor.setQueueCapacity(25);
		executor.initialize();
		return executor;
	}

	@Override
	public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
		return null;
	}
}
```

### 任务执行类

```java
@Service
public class AsyncTaskService {

	@Async // @Async 定义在方法上,该方法是个异步方法,若在类上,该类下所有的方法都是异步方法
	public void executeAsyncTask(Integer i) {
		System.out.println("执行异步任务：" + i);
	}
	
	@Async
	public void executeAsyncTaskPlus(Integer i) {
		System.out.println("执行异步任务 + 1: " + (i + 1));
	}
}

```

{% include JB/setup %}
