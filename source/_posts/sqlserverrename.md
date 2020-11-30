---
title: SqlServer 更改数据库名称
abbrlink: ab2a9e10
date: 2020-11-30 10:04:02
description: SqlServer 更改数据库名称
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 数据库因为一开始命名不规范，导致后来需要修改数据库名称
> 参考：[SqlServer 更改数据库名称](https://www.cnblogs.com/xuyufeng/p/9197137.html)

## 操作

- 1.首先选中需要更改数据库右击属性找到文件，可直接修改数据库逻辑名称

![](https://s3.ax1x.com/2020/11/30/D2Cq8e.png)

- 2.选中数据库右击选择重命名修改数据库名称。 (必要时候需要改成单用户模式)

- 3.将数据库进行分离，找到数据库文件mdf与ldf文件，直接更改文件名称

![](https://s3.ax1x.com/2020/11/30/D2PcZt.png)

- 4.附加数据库回来（此处注意需要重新选择物理文件路径，默认的为原先老的文件名）

![](https://s3.ax1x.com/2020/11/30/D2PfJS.png)




