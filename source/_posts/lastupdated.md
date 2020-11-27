---
title: 删除Maven下无效的lastUpdated文件
abbrlink: b5aff3ae
date: 2020-11-27 11:06:53
description: 删除Maven下无效的lastUpdated文件
tags: Maven
categories:
#  - Tools
  - Maven
top:
---

## 前言

> 删除maven仓库中大量未下载成功的.lastUpdated文件。
> 参考：[删除maven仓库中的LastUpdated文件](https://www.cnblogs.com/cxzdy/p/5662161.html)


## bat脚本

```shell

@echo off
rem REPOSITORY_PATH是Maven的路径
set REPOSITORY_PATH=D:\M2
rem 正在搜索...
for /f "delims=" %%i in ('dir /b /s "%REPOSITORY_PATH%\*lastUpdated*"') do (
    del /s /q %%i
)
rem 搜索完毕
::pause

```


{% note red 'fas fa-fan' simple%}
附录：[bat脚本下载](https://juno.lanzous.com/iyn3Dithizc)
{% endnote %}

