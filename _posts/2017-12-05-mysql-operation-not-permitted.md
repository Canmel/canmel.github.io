---
layout: post
title: "mysql Operation not permitted"
description: ""
category: 
tags: [Mysql]
---

## 场景
- 安装完`mysql`,发现命令行中没有`mysql`的命令，再找到`mysql`命令在`/usr/local/mysql/bin/`下。

## 计划
本来想把`mysql/bin`下的`mysql`命令在`/usr/bin`下创建一个软链接。但是需要权限。

当我再次sudo执行，还是抛出`mysql Operation not permitted`

## 原因

这是因为苹果在OS X 10.11中引入的SIP特性使得即使加了`sudo`（也就是具有root权限）也无法修改系统级的目录，其中就包括了/usr/bin。要解决这个问题有两种做法：

一种是比较不安全的就是关闭`SIP`，也就是`rootless`特性；

另一种是将本要链接到`/usr/bin`下的改链接到`/usr/local/bin`下就好了。

## 解决

> sudo ln -s /usr/local/mysql/bin/mysql /usr/local/bin


{% include JB/setup %}
