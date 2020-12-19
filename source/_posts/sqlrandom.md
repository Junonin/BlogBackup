---
title: Sql随机查询数据语句(NewID(),Rnd,Rand(),Random())
abbrlink: a4322757
date: 2020-12-18 11:43:57
description: Sql随机查询数据语句
tags: SQL
categories: 
#  - Tools
  - SQL
top:
---

## 前言

> 想从数据库随机抽取数据，想到使用Rand()函数，记录下。
> 参考：[SQL随机查询数据语句](https://www.cnblogs.com/alibai/p/4104564.html)

## SQL Server
```sql
Select TOP N * From TABLE Order By NewID()

NewID()函数将创建一个 uniqueidentifier 类型的唯一值。上面的语句实现效果是从Table中随机读取N条记录。
Uniqueidentifier用来存储一个全局唯一标识符，即GUID。
```

## Access
```sql
Select TOP N * From TABLE Order By Rnd(ID)

Rnd(ID) 其中的ID是自动编号字段，可以利用其他任何数值来完成，比如用姓名字段(UserName)

Select TOP N * From TABLE Order BY Rnd(Len(UserName))
```

## MySql
```sql
Select * From TABLE Order By Rand() Limit 10
```
- 在查询分析器中执行：select rand()，可以看到结果会是类似于这样的随机小数：0.36361513486289558，像这样的小数在实际应用中用得不多，一般要取随机数都会取随机整数。那就看下面的两种随机取整数的方法：
```sql
A：select  floor(rand()*N)  ---生成的数是这样的：12.0
B：select cast( floor(rand()*N) as int)  ---生成的数是这样的：12

A：select ceiling(rand() * N)  ---生成的数是这样的：12.0
B：select cast(ceiling(rand() * N) as int)  ---生成的数是这样的：12
```
- 比较 CEILING 和 FLOOR
CEILING 函数返回大于或等于所给数字表达式的最小整数。FLOOR 函数返回小于或等于所给数字表达式的最大整数。例如，对于数字表达式 12.9273，CEILING 将返回 13，FLOOR 将返回 12。FLOOR 和 CEILING 返回值的数据类型都与输入的数字表达式的数据类型相同。

## PostgreSQL
```sql
select * from glxt022 order by random() limit 5
```
