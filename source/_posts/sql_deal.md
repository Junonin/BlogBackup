---
title: 数据库的特殊情况处理笔记
categories:
  - SQL
description: 游标,备份，账户锁定
abbrlink: a73a1064
date: 2020-10-26 11:50:25
tags: SQL
top:
---
## 前言

> SQL的特殊情况处理笔记

## Sql 游标

1. 游标demo：

```mssql
DECLARE @username varchar(20)
DECLARE cursor_name CURSOR FAST_FORWARD FOR --定义游标
    SELECT TOP 10 Username FROM S_user_tb
OPEN cursor_name --打开游标
FETCH NEXT FROM cursor_name INTO @username  --抓取下一行游标数据
WHILE @@FETCH_STATUS = 0
    BEGIN
        PRINT '用户名：'+@username
        FETCH NEXT FROM cursor_name INTO @username
    END
CLOSE cursor_name --关闭游标
DEALLOCATE cursor_name --释放游标
```

## Convert(Decimal(18,2),0.00) 显示 .00 前面的0不显示问题

> 控制面版 -> 区域和语言选项 -> 区域选项-->其它设置 -> 数字 -> 显示前导零= “0.7”



* 设置标识列自增开关，然后手动插入指定标识的行。

```sql
set identity_insert 表名 on --关闭标识列的自增`
```


* 其次：插入你要插入的行并指定标识最后：

```sql
set identity_insert 表名 off --打开标识列的自增`
```


*  重置自增长的起始标号

```sql
dbcc checkident(表名,reseed,数字)
```



## SQL SERVER 默认备份路径

```sql
--当前的值是什么：
DECLARE @BackupDirectory VARCHAR(100)
EXEC master..xp_regread @rootkey='HKEY_LOCAL_MACHINE',
  @key='SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQLServer',
  @value_name='BackupDirectory',
  @BackupDirectory=@BackupDirectory OUTPUT
SELECT @BackupDirectory
--下面的代码是更改值：
EXEC master..xp_regwrite
     @rootkey='HKEY_LOCAL_MACHINE',
     @key='SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQLServer',
     @value_name='BackupDirectory',
     @type='REG_SZ',
     @value='D:\BACKUP'   --默认备份路径


```



## 查询数据库表行数

```sql
SELECT A.NAME ,B.ROWS  FROM sysobjects  A JOIN sysindexes B ON A.id = B.id WHERE A.xtype = 'U' AND B.indid IN(0,1) ORDER BY B.ROWS DESC
```



## 当前帐户被锁定

> 帐户当前被锁定,所以用户 'sa' 登录失败。系统管理员无法将该帐户解锁’解决方法
>
> 如果短时间内不停连接，就会被SQL SERVER误认为是这是攻击，会将此账号锁定。
>
> 要用windows身份验证登录，在查询分析器里输入：

```sql
ALTER LOGIN sa ENABLE ;
GO
ALTER LOGIN sa WITH PASSWORD = 'password' unlock, check_policy = off,
check_expiration = off ;
GO
```


## 查某个表的所有字段

```sql

SELECT GROUP_CONCAT(col.COLUMN_NAME)
FROM information_schema.COLUMNS col
WHERE col.TABLE_SCHEMA = 'mysql'
 AND col.TABLE_NAME = 'user'
 AND COLUMN_NAME NOT IN ('created_at', 'updated_at')
INTO @cols;

-- select @cols

SET @s = CONCAT('SELECT ', @cols, ' FROM mysql.user');

select @s

```

## SQLServer 中实现类似MySQL中的group_concat函数的功能

```sql
SQLServer中没有MySQL中的group_concat函数，可以把分组的数据连接在一起。
先对a列进行分组，对分组中的b以Xml形式输出，再使用stuff将开关多出的，删掉。
SELECT
    a,
    stuff((SELECT ',' + b FROM #tb WHERE a = t.a FOR xml path('')),1,1,''
    )AS b from  # tb AS t
GROUP BY
    a;

```