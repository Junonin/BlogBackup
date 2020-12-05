---
title: MySQL索引原则
abbrlink: ca3480e9
date: 2020-12-05 16:21:12
description: MySQL索引原则
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 有关索引的知识，记录下，防止遗忘
```sql
ALTER TABLE 表名 ADD [UNIQUE | FULLTEXT | SPATIAL]  INDEX | KEY  [索引名] (字段名1 [(长度)] [ASC | DESC]) [USING 索引方法]；
或
CREATE  [UNIQUE | FULLTEXT | SPATIAL]  INDEX  索引名 ON  表名(字段名) [USING 索引方法]；
```

## 正文

### 最左前缀匹配原则

MySQL 会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配。所以要尽量把这些条件放在最后，把“=”条件放在前面。举例如下：

    反例，不会用到 b 列的索引：
    ```sql
    select * from xttblog where a=1 and c>0 and b=2
    ```
    正例，会用到 b 列的索引：
    ```sql
    select * from xttblog where a=1 and b=2 and c>0
    ```
在索引选择上，尽量选择区分度高的列作为索引，区分度的公式是 count(distinct col)/count(*)，表示字段不重复的比例，比例越大我们扫描的记录数越少。
{% note red 'fas fa-fan' simple%}
默认情况下，当取出的数据超过全表数据的 20% 时，不会使用索引。
{% endnote %}

### like 使用原则
    反例，不使用索引：
    ```sql
    select * from codedq where name like '%JUNO%'
    ```
    正例，使用索引：
    ```sql
    select * from codedq where name like 'JUNO%'
    ```

### or 和 union all
or 和 union all 的使用选择。推荐尽量将 or 转换为 union all。

    反例，不使用索引：
    ```sql
    select * from xttblog where name='JUNO' or age='20'
    ```
    正例，使用索引：
    ```sql
    select * from xttblog where name ='JUNO' 
    union all 
    select * from xttblog where age ='20'
    ```

### 索引+函数
字段加上大多数函数都不会使用索引。所以尽量把函数放在数值上。

    反例，不使用索引：
    ```sql
    select * from xttblog where truncate(price) = 888
    ```
    正例，使用索引：
    ```sql
   select * from xttblog where price > 887 and price < 889
    ```

### 索引列数据类型转换
如果索引列使用的类型不对，发生类型转换。那么就需要注意，很可能不会按照你的期望走索引。
    
    反例，不使用索引：
    ```sql
    select * from xttblog where mobile = 1314520
    ```
    正例，使用索引：
    ```sql
   select * from xttblog where mobile = '1314520'
    ```

### 列上计算
字段加运算符不会使用索引。所以尽量把运算放在数值上。

    反例，不使用索引：
    ```sql
    SELECT account, amount
    FROM codedq
    WHERE amount + 3000 > 5000;
    ```
    正例，使用索引：
    ```sql
    SELECT account, amount
    FROM codedq
    WHERE amount > 2000 ;
    ```

### 组合索引
使用组合索引时，必须要包括第一个列。

    例如
    ```sql
    alter table xttblog add index(a,b,c)：
    ```
    反例，不使用索引：
    ```sql
    select * from xttblog where b=1 and c=2;

    select * from xttblog where b=1;

    select * from xttblog where c=2;
    ```
    正例，使用索引： 
    ```sql
    select * from xttblog where a=1 AND b=1 AND c=2;

    select * from xttblog where a=1 AND b=1;

    select * from xttblog where a=1 AND c=2;
    ```

### is null或is not null
尽量避免使用 is null 或 is not null。

    反例，不使用索引：
    ```sql
    SELECT …

    FROM codedq

    WHERE age IS NOT NULL;
    ```
    正例，使用索引： 
    ```sql
    SELECT …

    FROM codedq

    WHERE age >0;
    ```

### 不等于（!=）
不等于（!=）不会使用索引。

    反例，不使用索引：
    ```sql
    SELECT name

    FROM xttblog

    WHERE age !=0;
    ```
    正例，使用索引： 
    ```sql
    SELECT name

    FROM xttblog

    WHERE age >0;
    ```

### ORDER BY 子句
ORDER BY 子句只在以下的条件下使用索引：

1. ORDER BY 中所有的列必须包含在相同的索引中并保持在索引中的排列顺序

2. ORDER BY 中不能既有 ASC 也有 DESC

        举例：
        ```sql
        alter table xttblog add index(a,b);

        alter table xttblog add index(c);
        ```
        反例，不使用索引：
        ```sql
        ## 不在一个索引中
        select * from xttblog order by a,c; 
        ## 没有出现组合索引的第一列
        select * from xttblog order by b; 
        ## 混合 ASC 和 DESC
        select * from xttblog order by a asc, b desc; 
        ## where和order by用的不是同一个索引，where使用索引，order by不使用。
        select * from xttblog where a=1 order by c; 
        ```
        正例，使用索引： 
        ```sql
        select * from xttblog order by a,b;

        select * from xttblog order where a=1 order by b;

        select * from xttblog order where a=1 order by a,b;

        select * from xttblog order by a desc, b desc;

        select * from xttblog where c=1 order by c;
        ```