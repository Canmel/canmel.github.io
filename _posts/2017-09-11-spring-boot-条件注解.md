---
layout: post
title: "Spring boot 条件注解"
description: ""
category: 
tags: [Java, Spring, Spring-boot]
---

## 定义

> 通过活动的profile可以获得不同的bean, Spring 4提供了一个通用的基于条件的Bean 的创建, 即使用@Conditional注解

## 代码

### 判定条件定义

```java
public class LinuxCondition implements Condition {
	@Override
	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
		return context.getEnvironment().getProperty("os.name").contains("Linux");
	}
}
```

```java
public class WindowsCondition implements Condition {
	@Override
	public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
		return context.getEnvironment().getProperty("os.name").contains("Windows");
	}
}
```

### 不同条件下的Bean

```java
public interface ListService {
	public String showListCmd();
}
```

```java
public class LinuxListService implements ListService {

	@Override
	public String showListCmd() {
		return "ls";
	}
}
```

```java
public class WindowsListService implements ListService{

	@Override
	public String showListCmd() {
		return "dir";
	}
}
```

### 配置

```java
@Configuration
public class ConditionConfig {

	@Bean
	@Conditional(WindowsCondition.class) // WindowsCondition 中 matches 返回true 则使用这个bean 
	public ListService windowsListService() {
		return new WindowsListService();
	}
	
	@Bean
	@Conditional(LinuxCondition.class) // WindowsCondition 中 matches 返回true 则使用这个bean
	public ListService linuxListService() {
		return new LinuxListService();
	}
}
```

## 运行

```java
public static void main( String[] args ) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConditionConfig.class);
        ListService listService = context.getBean(ListService.class);
        System.out.println(context.getEnvironment().getProperty("os.name") + "系统下的列表命令:　" + listService.showListCmd());
}
```

{% include JB/setup %}
