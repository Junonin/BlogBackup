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










## 视频站搭建

- 苹果 CMS + 海螺模板
---



