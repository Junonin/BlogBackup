---
title: Todoist
date: 2020-12-25 00:00:00
comments: false
description: todo list
---

---
## mysql进阶讲解书籍

- 《 MySQL 技术内幕:InnoDB 存储引擎(第 2 版)》
- 高性能 mysql
- [数据库内核月报](http://mysql.taobao.org/monthly/)

## Linux下常驻后台运行的程序

- systemd 的 unit
- 短时间常驻用 screen、tmux
- nohup + crontab
- systemd 
- supervisor

## Systemd模板
```sh
改名 xxx.service，扔到 /etc/systemd/system 里，然后 systemctl start xxx 启动服务，systemctl enable xxx 开启自动启动

[Unit]
Description=
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=
User=
WorkingDirectory=
Restart=on-failure
RestartSec=3
StartLimitBurst=10

[Install]
WantedBy=multi-user.target
```

## mySql (or和in的效率问题)

in和or所在的列是否有索引或者是否是主键。如果有索引或者主键性能没啥差别，如果没有索引，or会随着记录越多的话性能下降，in的效率不会有太大的下降。


## mac下github的clone过慢

- Mac 的 ClashX 直接有复制终端代理命令的选项，复制后终端执行，然后 Clone 就走代理了，但只对当前终端起作用；要想以后打开终端都走代理，就修改配置文件.zshrc，增加

```shell
export https_proxy="http://127.0.0.1:7890"
export http_proxy="http://127.0.0.1:7890"
export all_proxy="socks5://127.0.0.1:7890"
```
设置了 https 代码却使用 git clone git@github.com 没用，因为ssh不走https。

- 配置全局 git 代理：

```shell
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy https://127.0.0.1:7890
```

## 消息队列的意义

消息队列的三大作用：解偶设计、异步任务、流量削峰

非常常见的解耦场景：一个生产方对应多个消费方(排队取号)



















## 国产数据库

达梦，神通，金仓，南大通用。。。

## 简易博客系统

1. [简易博客系统](https://github.com/wsydxiangwang/mood)
2. [Pagic 是一个由 Deno + React 驱动的静态网站生成器](https://pagic.org/zh-CN/docs/introduction.html)
3. [ISSUE BLOG](https://github.com/ttop5/issue-blog)

## 视频站搭建

- 苹果 CMS + 海螺模板
---



