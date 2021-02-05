---
title: Todoist
date: 2020-12-25 00:00:00
comments: false
description: todo list
---

---

## mysql进阶讲解书籍

- 《 MySQL 技术内幕:InnoDB 存储引擎(第 2 版) 》
- 《 高性能mysql(第3版) 》
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

## 万能模板 dev 合并到 master 有冲突

解决把 patch-1 合到 patch-2 时的冲突一般是把 patch-2 反过来合到 patch-1，确保 patch-1 包含 patch-2 所有的提交，github 解决 pr 冲突也是这样的思路

```sh
- git reset --soft                #回退到某个版本，只回退了commit的信息
- git stash                        #保存当前工作进度
- git fetch & merge master       #更新并合并master分支
- git stash pop                   #恢复工作进度到工作区
- fix conflict                    #手动解决冲突
- git commit & merge             #提交合并
```

## Node.js 后端作为一个服务运行在 Windows 服务器

- [nssm](https://nssm.cc/)
- [docker](https://www.docker.com/)

## Windows Terminal Preview设置

- [settings.json](https://juno.lanzous.com/iJ4M5ka372h)

## 抓包工具

- [网关：wireshark](https://www.wireshark.org/)
- [代理：fiddler](https://www.telerik.com/fiddler)
- [安卓：NetKeeper](xxx)

## 路由器

- MikroTik
- UBNT

## 硬盘

- HUH721212ALN604

## 国产数据库

达梦，神通，金仓，南大通用。。。

## 简易博客系统

1. [简易博客系统](https://github.com/wsydxiangwang/mood)
2. [Pagic 是一个由 Deno + React 驱动的静态网站生成器](https://pagic.org/zh-CN/docs/introduction.html)
3. [ISSUE BLOG](https://github.com/ttop5/issue-blog)

## 视频站搭建

- 苹果 CMS + 海螺模板

---

二次元随机图: <http://www.dmoe.cc/random.php>

## sql转置

```sql

 select * from (
    select MpCode,convert(varchar(13),DataTime,120) + ':00:00' as datatime , Rain ,
 right(convert(varchar(16),DataTime,120),2) as mins  from IrrBMRealRain
 where MpCode = 'Mp00001386' and convert(varchar(13),DataTime,120) = '2020-09-23 16'
 ) a PIVOT (sum(Rain) for mins in ([00],[05],[10],[15],[20],[25],[30],[35],[40],[45],[50],[55])) as xxx

```

## Vim

[Vim](https://mp.weixin.qq.com/s/Q0MVyvmAP96QG0qCMhODuQ)

## AnyDesk 使用 FRP 自建远程桌面连接

[AnyDesk 使用 FRP 自建远程桌面连接](https://mp.weixin.qq.com/s/nV19lLeOS241Bttqu1mQWQ)

## FRP代理工具详解

[FRP代理工具详解](https://mp.weixin.qq.com/s/ai8IFbSlr7gkLRj8xNaESg)

## SQL Server 常用的83条SQL语句

[SQL Server 常用的83条SQL语句](https://mp.weixin.qq.com/s/0X90yPf_w01M9IYpb11Aww)

## Jenkins在K8S中的三种部署

[Jenkins在K8S中的三种部署](https://mp.weixin.qq.com/s/HsJWg03wm9z5YY-7YBrYJQ)

## Springboot整合log4j2日志全解

[Springboot整合log4j2日志全解](https://www.cnblogs.com/keeya/p/10101547.html)

## Linux 最常用命令

[Linux 最常用命令](https://mp.weixin.qq.com/s/2WMxbvkpUrorFmQtMEp93g)

## hashMap的循环

[hashMap的循环](https://mp.weixin.qq.com/s/qSBLva5GBtrDCkIimqazDQ)

## 事务、隔离级别、锁

[事务、隔离级别、锁](https://mp.weixin.qq.com/s/kxLDum-mIDP4mFBfxoaR9Q)