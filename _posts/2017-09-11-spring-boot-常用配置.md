---
layout: post
title: "Spring boot 常用配置"
description: ""
category: 
tags: [java, spring, spring-boot]
---

# Spring 常用配置

## @Bean的`scope`

### 概念

> 描述Spring 容器如何新建Bean实例

### 实现 通过@Scope实现

- Singleton     一个spring容器只有一个bean实例,默认为单例
- Prototype     每次调用新建一个Bean
- Request       Web项目给每一个 Http Request新建一个实例
- Session       Web项目给每一个 Http Session新建一个实例
- GlobalSession 只在portal应用中有用,给每一个Global Http Session 创建一个






{% include JB/setup %}
