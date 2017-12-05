---
layout: post
title: "centos安装部署gitlab服务器"
description: ""
category: 
tags: []
---


## 1. 安装依赖软件
> yum -y install policycoreutils openssh-server openssh-clients postfix

## 2.设置postfix开机自启，并启动，postfix支持gitlab发信功能
> systemctl enable postfix && systemctl start postfix

## 3.下载gitlab安装包，然后安装

- **查看系统版本**
> cat /etc/os-release

- **centos 6系统的下载地址:**
> https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el6

- **centos 7系统的下载地址:**
> https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7

- **安装**
> rpm -i gitlab-ce-x.x.x-ce.x.x.x.x86_64.rpm

## 4.修改gitlab配置文件指定服务器ip和自定义端口
> vim  /etc/gitlab/gitlab.rb

```apple js
## 设置访问路径，http://127.0.0.1 
## 注意这里设置的端口不能被占用，默认是8080端口，如果8080已经使用，请自定义其它端口，并在防火墙设置开放相对应得端口
external_url 'http://127.0.0.1'
```

## 5.重置并启动GitLab

- 重新使用新配置
> gitlab-ctl reconfigure

- 重启
> gitlab-ctl restart

## 6.访问 GitLab页面

- 如果没有域名，直接输入服务器ip和指定端口进行访问
- **初始账户: root 密码: 5iveL!fe**
- 第一次登录修改密码,第一次访问直接让你修改密码

## 7.局域网hostname访问
> vi /etc/hosts
```apple js
192.168.xxx.xxx code.xxx.xxx
```
**访问 code.xxx.xxx**


{% include JB/setup %}
