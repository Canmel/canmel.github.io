---
layout: post
title: "ubuntu 安装jdk"
description: ""
category: 
tags: [java]
---
{% include JB/setup %}


### 添加ppa,更新

> ~ sudo add-apt-repository ppa:webupd8team/java

> ~ sudo apt-get update

###　安装oracle-java-installer

- jdk7
> ~ sudo apt-get install oracle-java7-installer
- jdk8
> ~ sudo apt-get install oracle-java8-installer

```
安装器会提示你同意 oracle 的服务条款,选择 ok,然后选择yes 即可
```
### 设置系统默认jdk
- JDk7
> ~ sudo update-java-alternatives -s java-7-oracle
- JDk8
> ~ sudo update-java-alternatives -s java-8-oracle

### 切换jdk版本
- jdk8 切换到jdk7
> ~ sudo update-java-alternatives -s java-7-oracle
- jdk7 切换到jdk8
> ~ sudo update-java-alternatives -s java-8-oracle

### 测试jdk 是是否安装成功
> ~ java -version
> ~ javac -version