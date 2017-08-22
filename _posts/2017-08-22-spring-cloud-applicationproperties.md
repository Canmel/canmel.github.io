---
layout: post
title: "Spring cloud application.properties"
description: "Spring cloud application.properties常用的配配置"
category: 
tags: []
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
        - register-with-eureka: `默认true, 启动时想注册机注册自己`
        - fetch-registry:   `检索服务　默认true`
        - registry-fetch-interval-seconds: `获取只读服务列表的时间间隔默认３０秒, 前提是fetch-registry　不为false`
    - instance:
        - hostname:         `主机名`    
        - lease-renewal-interval-in-seconds: `服务续约任务的调用时间　默认30秒`
        - lease-expiration-duration-in-seconds: `定义服务失效时间　默认90秒`
    - server:
        - enable-self-preservation: `关闭保护机制　默认true`
            
        
        
        
        
        
        