---
layout: post
title: "Spring boot 组合注解与元注解"
description: ""
category: 
tags: [Java, Spring, Spring-boot]
---

>　可以使用自定义的注解，使用多个注解组合成一个组合注解，这样我们只需要一个注解就可以表示两个注解

## 代码

### 组合注解

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
@ComponentScan
public @interface WiselyConfigration {

	String[] value() default {};
}
```

### 使用组合注解


```java
@WiselyConfigration("com.meedesidy.test_spring4.annotation")
public class DemoConfig {

}
```

### 运行

> 使用新的注解依然可以在容器中加入bean

```java
public static void main( String[] args ) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DemoConfig.class);
        DemoService demoService = context.getBean(DemoService.class);
        demoService.outputResult();
        context.close();
}
```



{% include JB/setup %}
