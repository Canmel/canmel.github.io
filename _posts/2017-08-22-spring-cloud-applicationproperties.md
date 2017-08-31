---
layout: post
title: "Spring cloud application.properties"
description: "Spring cloud application.properties常用的配配置"
category: 
tags: [spring-cloud]
---
{% include JB/setup %}

- server:
    - port:                 `服务端口`
- spring: 
    - application:  
        - name:             `服务名称，`      
- eureka:
    - client:
        - serviceUrl:
            - defaultZone:  `服务注册中心地址`
            为了安全很多时候都会为服务注册中心加上安全校验配置serviceUrl加上Url加上安全校验信息，比如`http://<username>:<password>@localhost:1111/eureka/`
            其中`username`是安全校验信息用户名，`password`为该用户的密码
        - register-with-eureka: `默认true, 启动时想注册机注册自己`
        - fetch-registry:   `检索服务　默认true`
        - registry-fetch-interval-seconds: `获取只读服务列表的时间间隔默认３０秒, 前提是fetch-registry　不为false`
    - instance:
        - hostname:         `主机名`    
        - lease-renewal-interval-in-seconds: `服务续约任务的调用时间　默认30秒`
        - lease-expiration-duration-in-seconds: `定义服务失效时间　默认90秒`
    - server:
        - enable-self-preservation: `保护机制(检测实例心跳链接: 15分组低于85%)　默认true`
            
        
        
        
        
        
