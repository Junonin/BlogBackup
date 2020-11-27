---
title: 数据库服务器情况
abbrlink: 9b7af42
date: 2020-11-27 10:12:12
description: 数据库服务器情况，日志文件占用情况，连接池情况
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 想了了解数据库目前的连接池情况和日志文件占用情况

## 连接池情况

```sql

select * from master.dbo.sysprocesses
where dbid = DB_ID('database')

select @@MAX_CONNECTIONS

SELECT @@CONNECTIONS

sp_who 'sa'

select spid,ecid,status,loginame,hostname,cmd,request_id
from sys.sysprocesses where loginame='sa' and hostname='JUNO'

```

## 日志文件占用情况

```sql

DBCC SQLPERF(LOGSPACE)

DBCC showfilestats

Exec sp_spaceused

SELECT a.name [文件名称]
	,cast(a.[size]*1.0/128 as decimal(12,1)) AS [文件设置大小(MB)]
	,CAST( fileproperty(s.name,'SpaceUsed')/(8*16.0) AS DECIMAL(12,1)) AS [文件所占空间(MB)]
	,CAST( (fileproperty(s.name,'SpaceUsed')/(8*16.0))/(s.size/(8*16.0))*100.0  AS DECIMAL(12,1)) AS [所占空间率%]
	,CASE WHEN A.growth =0 THEN '文件大小固定，不会增长' ELSE '文件将自动增长' end [增长模式]
	,CASE WHEN A.growth > 0 AND is_percent_growth = 0 THEN '增量为固定大小'
		WHEN A.growth > 0 AND is_percent_growth = 1 THEN '增量将用整数百分比表示'
		ELSE '文件大小固定，不会增长' END AS [增量模式]
	,CASE WHEN A.growth > 0 AND is_percent_growth = 0 THEN cast(cast(a.growth*1.0/128as decimal(12,0)) AS VARCHAR)+'MB'
		WHEN A.growth > 0 AND is_percent_growth = 1 THEN cast(cast(a.growth AS decimal(12,0)) AS VARCHAR)+'%'
		ELSE '文件大小固定，不会增长' end AS [增长值(%或MB)]
	,a.physical_name AS [文件所在目录]
	,a.type_desc AS [文件类型]
FROM sys.database_files  a
INNER JOIN sys.sysfiles AS s ON a.[file_id]=s.fileid
LEFT JOIN sys.dm_db_file_space_usage b ON a.[file_id]=b.[file_id]
ORDER BY a.[type]
```

{% note red 'fas fa-fan' simple%}
附录：[SQL下载](https://juno.lanzous.com/i2l1zitgpib)
{% endnote %}
