---
title: 批量备份数据库
abbrlink: 2e2ac8a6
date: 2020-11-26 16:21:17
description: 批量备份数据库
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 一些业务服务器在同一个服务器上很久没有做全量备份了，需要临时备份一次，就想做一个脚本全部备份一次。
>参考：[SQL Server 批量完整备份](https://www.cnblogs.com/gaizai/p/3542898.html)


## 单库备份

```sql
use datacenter

declare @SqlBackupDataBase as nvarchar(1000)
--根据时间拼接bak名称，形成备份语句
set @SqlBackupDataBase=N'BACKUP DATABASE Gd_datacenter TO DISK = ''F:\backup\Gd_datacenter-'+
CONVERT(varchar(10),GETDATE(),120)+'Full.bak''WITH FORMAT, NAME = N''Gd_datacenter_Full_' + 
CONVERT(varchar(10),GETDATE(),120)+''' 
'
--备份文件格式
print @SqlBackupDataBase --打印出来（为了方便调试，可省略）
exec sp_executesql @SqlBackupDataBase 
```


## 批量备份

```sql

DECLARE
      @FileName VARCHAR(200),
      @CurrentTime VARCHAR(50),
      @DBName VARCHAR(100),
      @SQL VARCHAR(1000)

SET @CurrentTime = CONVERT(CHAR(8),GETDATE(),112) + CAST(DATEPART(hh, GETDATE()) AS VARCHAR) + CAST(DATEPART(mi, GETDATE()) AS VARCHAR)

DECLARE CurDBName CURSOR FOR 
    SELECT NAME FROM Master..SysDatabases where dbid>4

OPEN CurDBName
FETCH NEXT FROM CurDBName INTO @DBName
WHILE @@FETCH_STATUS = 0
BEGIN
    --Execute Backup
    --保存bak的位置可以更改
    SET @FileName = 'E:\DBBackup\' + @DBName + '_' + @CurrentTime
    SET @SQL = 'BACKUP DATABASE ['+ @DBName +'] TO DISK = ''' + @FileName + '.bak' +
     ''' WITH NOINIT, NOUNLOAD, NAME = N''' + @DBName + '_backup'', NOSKIP, STATS = 10, NOFORMAT'
    EXEC(@SQL)

    --Get Next DataBase
    FETCH NEXT FROM CurDBName INTO @DBName
END

CLOSE CurDBName
DEALLOCATE CurDBName

```

{% note red 'fas fa-fan' simple%}
附录：[SQL下载](https://juno.lanzous.com/iPwKeiso2da)
{% endnote %}

