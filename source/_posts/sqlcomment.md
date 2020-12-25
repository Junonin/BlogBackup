---
title: Oracle、Mysql、SqlServer创建表和给表和字段加注释
abbrlink: 9770b99d
date: 2020-12-24 18:47:45
description: Oracle、Mysql、SqlServer创建表和给表和字段加注释,sql注释
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 记录下表和字段加注释
> 参考：[Oracle、Mysql、SqlServer创建表和给表和字段加注释](https://www.cnblogs.com/a735882640/p/9639904.html)


## Oracle

```sql
--创建表
create table test ( 
     id varchar2(200) primary key not null,
     sort number, 
     name varchar(200)
)
--表加注释
comment on table test is '测试表' 
--字段加注释
comment on column test.id is 'id'; 
comment on column test.sort is '序号';
--查看表字段注释
select table_name,column_name,comments from user_col_comments where table_name='test';
```

## Mysql

```sql
--创建表
create table test ( 
     id varchar(200) not null,
     sort int(11) comment '排序',
     name varchar(200) comment  '名称',
)           
--表加注释
alter table test comment ='测试表' 
--字段加注释
alter table test modify column field_name int comment '修改后的字段注释'
--查看表的注释
select table_name,table_comment from information_schema.tables where table_schema ='db' and table_name = 'tablename' ;
--查看列名注释
select column_name,column_comment from information_schema.columns where table_schema ='db' and table_name = 'tablename' ;
```

## SqlServer

```sql
--创建表
create table test ( 
     id varchar(200) primary key not null,
     sort int,
     name varchar(200),
)
--给字段添加注释
EXECUTE sp_addextendedproperty N'MS_Description',N'字段备注信息',N'SCHEMA',N'dbo',N'table',N'表名',N'column',N'添加注释的字段名';
--修改字段注释
EXECUTE sp_updateextendedproperty N'MS_Description',N'修改的注释内容',N'SCHEMA',N'dbo',N'table',N'表名',N'column',N'添加注释的字段名';
--删除字段注释 
EXECUTE sp_dropextendedproperty N'MS_Description',N'SCHEMA',N'dbo',N'table',N'表名',N'column',N'添加注释的字段名';

--添加表注释
EXECUTE sp_addextendedproperty N'MS_Description',N'注释内容',N'SCHEMA',N'dbo',N'table',N'表名',NULL, NULL;
--修改表注释
EXECUTE sp_updateextendedproperty  N'MS_Description',N'修改注释内容',N'SCHEMA',N'dbo',N'table',N'表名',NULL, NULL;
--删除表注释  
execute sp_dropextendedproperty N'MS_Description',N'SCHEMA',N'dbo',N'table',N'表名',NULL, NULL;

--获取表注释
SELECT obj.name AS [TableName],pro.value AS [Description] 
FROM sys.objects obj LEFT JOIN sys.extended_properties pro ON obj.object_id=pro.major_id
WHERE obj.type='u'

--获取字段注释
SELECT A.name AS table_name,B.name AS column_name,C.value AS column_description
FROM sys.tables A
INNER JOIN sys.columns B ON B.object_id = A.object_id
LEFT JOIN sys.extended_properties C ON C.major_id = B.object_id AND C.minor_id = B.column_id
WHERE A.name = '表名'
```