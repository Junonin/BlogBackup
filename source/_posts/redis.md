---
title: Redis
abbrlink: 7b25d017
date: 2020-11-25 18:02:03
description: Redis，Redis服务，Redis-windows
tags: Redis
categories:
  - Redis
top:
---

## 前言

> Redis学习记录

## Redis下载

> **[Windows下载地址3.x.x](https://github.com/MicrosoftArchive/redis/releases)**
>
> **[Windows下载地址5.x.x](https://github.com/tporadowski/redis/releases)**

## Windows下配置

> ##### **redis.windows.conf**文件配置密码
>
> ##### requirepass password(你的密码)

Windows下启动

```bash
@echo off
cd /d D:\Redis-x64-3.2.100[替换为你的redis路径]

start redis-server.exe redis.windows.conf

echo redis start...
::pause
```

在Redis目录下运行

```bash
redis-cli.exe -h 127.0.0.1 -p 6379
```

```bash
set  key  abc

get  key 
```


## Redis服务安装
- 进入Redis安装包目录，cmd安装服务

```cmd
redis-server --service-install redis.windows-service.conf --loglevel verbose
```

提示 Redistribution successfully installed as a service.即安装成功

![](https://s2.ax1x.com/2019/11/21/MIOn29.png)