---
layout: post
title: "spring-boot 坑坑洼洼的小路"
description: ""
category: 
tags: [Java, Spring-boot, Exp]
---

服务器打jar包部署问题解决

---

## 问题描述：
### 最近在看spring-boot的东西，POM.xml基本的内容包括：

```
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>
	
		<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-aop</artifactId>
		</dependency>
    <dependencies>

```

### 出现的问题：
- 本地eclipse上 `Run As Application`正常访问，正常使用
- 本地使用 `mvn spring-boot:run `，正常启动，正常使用
- 服务器使用`jar`包启动，`java -jar xxxxxx.jar`, 访问的各页面找不到，404

```
x.x.x.x ERROR template might not exist or might not be accessible by any of the configured Template Resolvers
```


## 可用的解决方法

1. 可能是静态资源没有打进`jar`包，在`mapper`中加入`<build>`

    ```
    <build>
    <resources>
    		<resource>
    			<directory>src/main/resources</directory>
    			<includes>
    				<include>**/*.*</include>
    			</includes>
    			<filtering>false</filtering>
    		</resource>
    		<resource>
    			<directory>src/main/java</directory>
    			<includes>
    				<include>**/*.*</include>
    			</includes>
    			<excludes>
    				<exclude>**/*.java</exclude>
    			</excludes>
    			<filtering>false</filtering>
    		</resource>
    	</resources>
    </build>
    ```

2. `theamleaf`支持强格式的H5语法，可以严格将`html`标签全部对应结束标签，或者让其支持不严格的`html`标签

    ```
    <dependency>
    	<groupId>net.sourceforge.nekohtml</groupId>
    	<artifactId>nekohtml</artifactId>
    </dependency>
    ```

   然后在品配置文件中加入:

    ```
    spring:
      aop:
        auto: true
      thymeleaf:
        suffix: .html
        prefix: classpath:/templates/
        encoding: UTF-8
        mode: LEGACYHTML5   ## 支持不严格的Html格式
        content-type: text/html
        cache: false
    
    ```

3. 可能是启用了`theamleaf`的默认配置,`theamleaf`默认前缀`/templates/`

4. （我的错误就是这个造成的）在`controller`中`return`了的`viewName`前面有`/`,造成在`jar`包里面没有走相对的路径，删除返回页面的前置的斜杠`/`即可。 


{% include JB/setup %}
