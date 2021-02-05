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

> ### **redis.windows.conf**文件配置密码
>
> ### requirepass password(你的密码)

Windows下启动

```shell
@echo off
cd /d D:\Redis-x64-3.2.100[替换为你的redis路径]

start redis-server.exe redis.windows.conf

echo redis start...
::pause
```

在Redis目录下运行

```shell
redis-cli.exe -h 127.0.0.1 -p 6379
```

```shell
set  key  abc

get  key 
```

## Redis服务安装

- 进入Redis安装包目录，cmd安装服务

```shell
redis-server --service-install redis.windows-service.conf --loglevel verbose
```

提示 Redistribution successfully installed as a service.即安装成功

![x](https://s2.ax1x.com/2019/11/21/MIOn29.png)

## Redis-cli命令行远程连接Redis服务

```shell
redis-cli -h host -p port -a password
redis-cli -h 192.168.1.131 -p 6379 -a ''
```

### 查看redis客户端连接

```shell
redis-cli info clients

# Clients
connected_clients:6000
client_longest_output_list:0
client_biggest_input_buf:5792
blocked_clients:0
```

### 查看redis客户端状态

```shell
redis-cli client list
# list
addr=127.0.0.1:52555 fd=5 name= age=855 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=client
addr=127.0.0.1:52787 fd=6 name= age=6 idle=5 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 events=r cmd=ping
```

age:表示连接存在的时间，单位秒
idle:表示连接空闲时间，单位秒

### 查看redis客户端超时设置

```shell
redis-cli config get timeout
"timeout"
"0" #0表示不开启空闲清除
```

### 设置空闲清理时间

```shell
redis-cli config set timeout 600 #单位秒
```

### redis 60 秒内的最大响应延迟

```shell
redis-cli -h 127.0.0.1 -p 6379 --intrinsic-latency 60
```

### 查看一段时间内 Redis 的最小、最大、平均访问延迟

```shell
redis-cli -h 127.0.0.1 -p 6379 --latency-history -i 1
```

## redis慢日志命令

```shell

# 命令执行耗时超过 5 毫秒，记录慢日志
CONFIG SET slowlog-log-slower-than 5000
# 只保留最近 500 条慢日志
CONFIG SET slowlog-max-len 500
# 查看最近的5条慢日志
SLOWLOG get 5

# 集中过期会导致 Redis 延迟变大，因为主动过期 key 的定时任务，是在 Redis 主线程中执行的。
# 设置 key 的过期时间时，增加一个随机时间
# 1.在过期时间点之后的 5 分钟内随机过期掉
redis.expireat(key, expire_time + random(300))
# 2.Redis 4.0 以上版本，开启 lazy-free 机制
# 释放过期 key 的内存，放到后台线程执行
lazyfree-lazy-expire yes

# 查看 Redis 进程是否使用到了 Swap，内存被换到了磁盘上会变慢
# 先找到 Redis 的进程 ID
ps -aux | grep redis-server
# 查看 Redis Swap 使用情况
cat /proc/$pid/smaps | egrep '^(Swap|Size)'

# redis内存碎片
Memory
# 内存碎片率计算：mem_fragmentation_ratio = used_memory_rss / used_memory

# 开启自动内存碎片整理（总开关）
activedefrag yes
# 内存使用 100MB 以下，不进行碎片整理
active-defrag-ignore-bytes 100mb
# 内存碎片率超过 10%，开始碎片整理
active-defrag-threshold-lower 10
# 内存碎片率超过 100%，尽最大努力碎片整理
active-defrag-threshold-upper 100
# 内存碎片整理占用 CPU 资源最小百分比
active-defrag-cycle-min 1
# 内存碎片整理占用 CPU 资源最大百分比
active-defrag-cycle-max 25
# 碎片整理期间，对于 List/Set/Hash/ZSet 类型元素一次 Scan 的数量
active-defrag-max-scan-fields 1000

```
