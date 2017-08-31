---
layout: post
title: "spring cloud config 分布式配置中心"
description: ""
category: 
tags: [java, spring]
---
{% include JB/setup %}

# spring cloud config 分布式配置中心

之前跟着`spring cloud 微服务实战`做分布式配置中心，一直报`环境仓库`, 注入失败的问题,记录下现在的成功项目现状

[TOC]


### 1. 文件结构
- [X] **config**
    - [X]  **src/main/java**
        - [X] **com/meedesidy/config**
            - [ ] ConfigApplication.class   
    - [X]  **src/main/resources**
        - [ ] application.properties
        - [ ] bootstrap.properties
    - [X]  **src/test/java**
        - [X] **com/meedesidy/cinfig**
            - [ ] ConfigApplicationTest.class
    - [ ] pom.xml
    

### 2. 依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.skio</groupId>
	<artifactId>config</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>config</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<spring-cloud.version>Dalston.SR3</spring-cloud.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-config-server</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>${spring-cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```

### 3. 添加注释

```java
@SpringBootApplication
@EnableConfigServer
public class ConfigApplication {
	public static void main(String[] args) {
		SpringApplication.run(ConfigApplication.class, args);
	}
}
```
添加`@SpringBootApplication` 开启`Spring Cloud Config 服务端功能`


---

### 4. 添加配置
1. `application.properties`
```properties
server.port=8888

spring.cloud.config.label=master
spring.cloud.config.server.git.uri=https://github.com/Canmel/spring-cloud-config/
spring.cloud.config.server.git.uri.username=892379244@qq.com
spring.cloud.config.server.git.uri.password=xxxxxx
spring.cloud.config.server.git.uri.search-paths=xxxxxx
```
#### 配置说明
1. `spring.cloud.config.server.git.uri`: Git仓库位置
2. `spring.cloud.config.server.git.uri.username`: Git用户名
3. `spring.cloud.config.server.git.uri.password`: Git密码
4. `spring.cloud.config.server.git.uri.search-paths`: 仓库下相对位置，多个时以逗号分隔

---

### 5. 填写仓库中实际配置信息
- 在git上的`search-paths`路径下添加文件
    - meedesidy.properties
    - meedesidy-test.properties
    - meedesidy-dev.properties
    - meedesidy-prod.properties
- `search-paths`路径下的文件添加对应内容
    - meedesidy.properties => `from=meedesidy-default-1.0`
    - meedesidy-test.properties => `from=meedesidy-test-1.0`
    - meedesidy-dev.properties => `from=meedesidy-dev-1.0`
    - meedesidy-prod.properties => `from=meedesidy-prod-1.0`
- 切换分支`test-branch`,修改`search-paths`路径下的文件
    - meedesidy.properties => `from=meedesidy-default-2.0`
    - meedesidy-test.properties => `from=meedesidy-test-2.0`
    - meedesidy-dev.properties => `from=meedesidy-dev-2.0`
    - meedesidy-prod.properties => `from=meedesidy-prod-2.0`

---

### 6. 测试使用
#### ①　创建服务，在`pom.xml`添加下面的依赖
```
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```
#### ② 　创建`bootstrap.properties`
    
```text
spring.application.name=meedesidy
spring.cloud.config.profile=dev
spring.cloud.config.label=master
spring.cloud.config.uri=http://localhost:8888

server.port=3001
```
> 注：客户端获得外部配置文件后加载到客户端ApplicationContext实例，该配置内容优先级高于客户端jar包内部的配置内容，所以jar包内重复的内容不在重复加载
> 如：　若`application.properties`与`bootstrap.properties`中都定义了项目名称，则以前者为准．

#### ③ 　属性绑定@Value("${from}")
```java
@RefreshScope
@RestController
Public class TestController{
	@Value("${from}")
	private String from;
	
	@RequestMapping(value = "/from", method = RequestMethod.GET)
	public String from(){
	    return this.from;
	}
}
```

#### ④　注入@Autowired
```java
@RefreshScope
@RestController
Public class TestController{
    @Autowired
	private Environment env;
	
    @RequestMapping(value = "environment", method = RequestMethod.GET)
	public String environment() {
		return env.getProperty("from", "undefined");
	}
}
```
> 注：　`Environment`在 `org.springframework.core.env.Environment`,并不在cloud下

#### ⑤　启动访问
- 启动 `注册中心`　
- 启动 `配置中心`
- 启动 `meedesidy`应用
- 访问　`localhost:3001/environment`可以根据配置环境(profile)输出对应的`from`属性值
- 访问　`localhost:3001/from`可以根据配置环境(profile)输出对应的`from`属性值
- 修改`git`上的文件数据**`不可以`**实时更改

---

### 占位符配置URI
> 如：　spring.cloud.config.server.git.uri=https://github.com/Canmel/{application}

- **{application}**
spring cloud 会根据`spring.application.name`填充占位符,从而动态获取配置
- **{profile}**
根据`profile`填充
- **{lebel}**
分支，如果分支名中出现'/',在HTTP的URL中使用＂(_)＂来替代，避免改变URI含义

---

### 安全保护
> 敏感信息存放在未加密的微服务应用上是不安全的
> 配置中心加密方式有很多，由于spring cloud基于`spring boot`与`Spring Security` 结合使用会更加方便

#### 配置`Spring Security`
- 依赖
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
- 启动默认会在日志给出一个随机密码，一般不适用随机密码，由配置文件指定
```
sucurity.user.name=meeedesidy
security.user.password=mypassword
```
- 客户端使用，在配置文件中添加配置（存在bootstrap.properties时，似乎在application.propeties中配置无效）
```
#-- bootstrap.properties 
spring.cloud.config.username=meedesidy
spring.cloud.config.password=mypassword
```
---

### 服务化配置中心
#### 配置中心

- 依赖
```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
```
- 主类添加注释`@EnableDiscoveryClient`,以获取被注册中心发现的能力．
- 配置文件,指明注册中心地址
```xml
 eureka.client.serviceUrl.defaultZone=http://peer1:1111/eureka/
```
#### 客户端
- 配置(添加对配置中心的需求及安全信息)
```yml
spring:
  cloud:
    config:
      username: meedesidy
      password: lidejian111
      discovery:
        enable: true
        serviceId: config-server
      profile: dev
```
- 主类添加注释`@EnableDiscoveryClient`,以获取被注册中心发现的能力．
#### 使用
```java
@Value("${from}")
private String from;

@RequestMapping("/from")
public String from(){
    return from;
}
```
















