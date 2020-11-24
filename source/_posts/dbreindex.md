---
title: 索引重建
abbrlink: 10d7ac37
date: 2020-11-20 11:13:36
description: DBCC DBREINDEX，DBCC SHOWCONTIG，重建索引，索引重建
tags: SQL
categories:
#  - Tools
  - SQL
top:
---

## 前言

> 因为数据库插入的时候执行时间过长，通过DBCC SHOWCONTIG看到Scan Density **扫描密度** 指标过低，需要利用DBCC DBREINDEX重建索引提高SQL Server性能
>参考：[DBCC SHOWCONTIG 索引碎片查询]("https://blog.csdn.net/wangjunhe/article/details/7228208)

## 正文
- 因为需要遍历整个数据库，然后分析其中的Scan Density低于90%的表，才需要进行做DBCC DBREINDEX，我用了一张临时表，然后将低于90%的数据插入，再遍历出来做DBCC DBREINDEX，最后再删除临时表，如下：
```sql

/*注意执行的时候会导致表锁死，请谨慎操作*/
/*批量重建当前库的所有表中扫描密度低于90%*/
 IF object_id('tempdb..#temp') is not null 
 Begin
 --判断临时表是否存在
       DROP TABLE  #temp
 End

DECLARE @tablename VARCHAR (128)

--创建临时表
CREATE TABLE #temp (
		ObjectName CHAR ( 255 ),
		ObjectId INT,
		IndexName CHAR ( 255 ),
		IndexId INT,
		Lvl INT,
		CountPages INT,
		CountRows INT,
		MinRecSize INT,
		MaxRecSize INT,
		AvgRecSize INT,
		ForRecCount INT,
		Extents INT,
		ExtentSwitches INT,
		AvgFreeBytes INT,
		AvgPageDensity INT,
		ScanDensity DECIMAL,
		BestCount INT,
		ActualCount INT,
		LogicalFrag DECIMAL,
		ExtentFrag DECIMAL 
	)

--创建游标获取所有表名
DECLARE tables CURSOR FOR
   SELECT TABLE_NAME
   FROM INFORMATION_SCHEMA.TABLES
   WHERE TABLE_TYPE = 'BASE TABLE'
	 
-- 开启游标
OPEN tables
--获取光标位置
FETCH NEXT 
FROM tables INTO @tablename
	 
--FETCH 语句成功
WHILE @@FETCH_STATUS = 0

BEGIN
		--遍历执行DBCC SHOWCONTIG(@table_name)
   INSERT INTO #temp 
   EXEC ('DBCC SHOWCONTIG (''' + @tablename + ''') 
      WITH FAST, TABLERESULTS, ALL_INDEXES, NO_INFOMSGS')
   FETCH NEXT
      FROM tables
      INTO @tablename
END

--关闭游标
CLOSE tables
--删除游标
DEALLOCATE tables
--关注BestCount,ActualCount的指标
--select ObjectName,BestCount,ActualCount , cast((BestCount*1.00)/(ActualCount*1.00) as  DECIMAL(18,2)) as result  from #temp where ActualCount <> 0
--DBCC SHOWCONTIG ('dbo.yd_mp_realdata')


--DECLARE @tablename VARCHAR (128)

--创建需要重建索引的游标
DECLARE tables CURSOR FOR
--关注BestCount,ActualCount的指标
SELECT DISTINCT ObjectName FROM #temp
WHERE cast((BestCount*1.00)/(ActualCount*1.00) as  DECIMAL(18,2)) < 0.90 and ActualCount <> 0
-- 开启游标
OPEN tables
--获取光标位置
FETCH NEXT 
FROM tables INTO @tablename
	 
--FETCH 语句成功
WHILE @@FETCH_STATUS = 0
BEGIN
	--重建索引
	--输出表名
	PRINT @tablename
	--DBCC dbreindex ('' + @tablename + '','',0)
	EXEC ( 'DBCC DBREINDEX (' + @tablename + ','''',0) WITH NO_INFOMSGS ' )
	FETCH NEXT
		FROM tables
		INTO @tablename
END
--关闭游标
CLOSE tables
--删除游标
DEALLOCATE tables

DROP TABLE #temp


```

## 结语
- 操作后可以看到Scan Density **扫描密度** 指标恢复正常
[![DM5KoR.png](https://s3.ax1x.com/2020/11/20/DM5KoR.png)](https://imgchr.com/i/DM5KoR)


{% note red 'fas fa-fan' simple%}
附录：[SQL下载](https://juno.lanzous.com/irRMgikpved)
{% endnote %}