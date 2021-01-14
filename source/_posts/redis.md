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


## Redis-cli命令行远程连接Redis服务

```cmd
redis-cli -h host -p port -a password
redis-cli -h 192.168.1.131 -p 6379 -a ''
```

### 查看redis客户端连接

```cmd
redis-cli info clients

# Clients
connected_clients:6000
client_longest_output_list:0
client_biggest_input_buf:5792
blocked_clients:0
```

### 查看redis客户端状态

```cmd
redis-cli client list
# list
addr=127.0.0.1:52555 fd=5 name= age=855 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=client
addr=127.0.0.1:52787 fd=6 name= age=6 idle=5 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=ping
```
age:表示连接存在的时间，单位秒
idle:表示连接空闲时间，单位秒

### 查看redis客户端超时设置

```cmd
redis-cli config get timeout
"timeout"
"0" #0表示不开启空闲清除
```

### 设置空闲清理时间

```cmd
redis-cli config set timeout 600 #单位秒
```