---
title: Mysql
abbrlink: 9520183a
date: 2021-01-18 16:16:28
description: Mysql
categories:
  - SQL
  - Mysql
top: 
tags: Mysql
---

## 前言

> Mysql学习记录

## explain

- Type常用的类型有： ALL/全表、index/索引、range/索引范围、 ref/非唯一索引扫描或唯一索引前缀扫描、eq_ref/唯一索引，主键、const/system/主键或唯一索引的单记录、NULL/不访问任何表或索引直接返回结果（从左到右，性能从差到好）

## 表设计

 1. 用0代替NULL。

 2. INT，TINYINT，SMALL。

 3. 枚举，整数，代替字符串类型。

 4. TimeSamp代替Datatime。datetime 占用8个字节，timestamp 占用4个字节。而 timestamp 表示的范围是 1970—2037 适合做更新时间。

 5. 单表字段<20个。

 6. 整型存IP。

## 索引设计

 1. 值分布很稀少的字段不适合建索引，例如"性别"这种只有两三个值的字段。

 2. 字符字段只建前缀索引。

 3. 字符字段最好不要做主键

 4. 不用外键，由程序保证约束

 5. 尽量不用UNIQUE，由程序保证约束

 6. 使用多列索引时主意顺序和查询条件保持一致，同时删除不必要的单列索引

## 分区设计

 1. 一个表最多只能有1024个分区。

 2. 如果分区字段中有主键或者唯一索引的列，那么所有主键列和唯一索引列都必须包含进来。

 3. 分区表无法使用外键约束。

 4. NULL值会使分区过滤无效。

 5. 所有分区必须使用相同的存储引擎。

 >Rang分区
 >List分区
 >Hash分区
 >Key分区

## 编写注意

 1. 使用limit对查询结果的记录进行限定。

 2. 避免select *，将需要查找的字段列出来。

 3. 使用连接（join）来代替子查询。

 4. 拆分大的delete或insert语句。

 5. 可通过开启慢查询日志来找出较慢的SQL。

 6. 不做列运算：SELECT id WHERE age + 1 = 10，任何对列的操作都将导致表扫描，它包括数据库教程函数、计算表达式等等，查询时要尽可能将操作移至等号右边。

 7. sql语句尽可能简单：一条sql只能在一个cpu运算；大语句拆小语句，减少锁时间；一条大sql可以堵死整个库。

 8. OR改写成IN：OR的效率是n级别，IN的效率是log(n)级别，in的个数建议控制在200以内。

 9. IN 和 NOT IN 也要慎用，否则会导致全表扫描。对于连续的数值，能用 BETWEEN 就不要用 IN：select id from t where num between 1 and 3。

 10. 不用函数和触发器，在应用程序实现。

 11. 避免%xxx式查询。

 12. 少用JOIN。

 13. 使用同类型进行比较，比如用'123'和'123'比，123和123比。

 14. 尽量避免在WHERE子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。

 15. 对于连续数值，使用BETWEEN不用IN：SELECT id FROM t WHERE num BETWEEN 1 AND 5。

 16. 列表数据不要拿全表，要使用LIMIT来分页，每页数量也不要太大。
