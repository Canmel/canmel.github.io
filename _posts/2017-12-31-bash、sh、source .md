---
layout: post
title: "ubuntu中bash、sh、source "
description: ""
category: 
tags: []
---

## 在Ubuntu 当中 执行脚本默认的使用的是dash，而非bash，执行能力较弱

### 例如1.sh

> !/bin/sh
> source /etc/profile 

输出：`source: not found`
所以不能使用在`sh`中使用source

### 2.sh

> !/bin/bash
> source /etc/profile 

正常执行
所以可以在`bash`中使用source



### 3.source等价于 .

所以使用`. /etc/profile`都会执行正常 （注意点号后面空格）


{% include JB/setup %}
