---
title: 数据库：删除重复记录（去重）
categories:
  - SQL
description: 删除重复记录，删除第一条重复记录
abbrlink: a73a10a8
date: 2020-10-22 11:50:37
tags: SQL
top:
---

## 前言

> 因为之前数据库的字段添加了索引，但是没有添加唯一索引约束，导致数据库的日月年数据出现的重复。

### 查询

* distinct去重

```sql
 select distinct * from table 表名 where (条件)
```

* 存在部分字段相同的纪录（有主键id即唯一键）

```sql
 select * from table where id in (select max(id) from table group by [去除重复的字段名列表,....])
```

* 存在部分字段相同的纪录（没有唯一键id）

> 可以利用row_number()建立一个值区分去重

```sql
 select * from
 (select *, row_number() over(partition by 分组字段 order by 排序 desc) rn from 表名) A
 where rn = 1
```



### 删除

* top

```sql
DELETE TOP (20) from table 表名 where (条件)
```

* 存在部分字段相同的纪录（有主键id即唯一键）

```sql
delete from 表名 where id in (
select min(id) from 表名 group by [去除重复的字段名列表,....]
having  count(1)>1 )
```

* 存在部分字段相同的纪录（没有唯一键id）

```sql
delete A from
(select *, row_number() over(partition by 分组字段 order by 排序 desc) rn from 表名) A
where rn = 2
```

{% note danger simple %}
为了避免导致出现重复数据，还是要建议唯一索引比较好
{% endnote %}
