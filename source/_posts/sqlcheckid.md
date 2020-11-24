---
title: 如何用SQL来核查序号是否连续
abbrlink: 2b21e7e
date: 2020-11-24 15:01:15
description: 如何用SQL来核查序号是否连续
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 设置了自增id的数据，想查询哪个id被删除了，导致不连续

## 查询

```sql
select id+1 from TableName
where id+1 not in(select id from TableName)
and id+1<>(select max(id)+1 from TableName)
```


