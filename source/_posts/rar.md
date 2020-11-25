---
title: 一些Bat的备份脚本
abbrlink: 6ad92fda
date: 2020-11-26 17:34:00
description: 备份后自动压缩脚本，winrar命令行压缩，bandizip命令行压缩
tags: Bat
categories: 
#  - Tools
  - Bat
top:
---

## 前言

> 因为需要主从盘，跨服务器之类的备份，记录一下常用的备份脚本
>参考：[Bandizip 命令行参数](https://www.bandisoft.com/bandizip/help/parameter/)
>参考：[Winrar 命令行参数](https://www.cnblogs.com/lidabo/archive/2012/08/10/2632219.html)


## 正文
- 一个备份到从盘的demo
```shell

@echo off
rem echo是off 不打印注释rem
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
::     备份Tomcat
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
title  备份Tomcat

set rar=E:\backup\
rem 根据时间在E:\backup\创建当天的新文件夹  例：20200512
set data=%date:~0,4%%date:~5,2%%date:~8,2%
md %rar%%data%

rem 利用Bandizip做从盘备份
Bandizip bc -aoa -o:%rar%%data%\ D:\Tomcat8-01 D:\Tomcat8-02 D:\Tomcat8-03 D:\Tomcat8-04 D:\Tomcat8-05 
rem D:\Tomcat8-01 → E:\backup\20200512\Tomcat8-01.zip
rem D:\Tomcat8-02 → E:\backup\20200512\Tomcat8-02.zip
rem D:\Tomcat8-03 → E:\backup\20200512\Tomcat8-03.zip

rem 删除备份目录下7天前的文件（目录为E:\backup）
forfiles /p "E:\backup" /s /m *.* /d -7 /c "cmd /c del @path"

rem pause


```

{% note danger modern %}
删除某个目录下系统文件修改日期七天前的
```shell
rem 删除C:\sql back目录下7天前的*.dbb *.bak文件
Forfiles /p "c:\sql back" /s /d -7 /m *.dbb /c "cmd /c del /q /f @path"
Forfiles /p "c:\sql back" /s /d -7 /m *.bak /c "cmd /c del /q /f @path"
```
{% endnote %}


{% note red 'fas fa-fan' simple%}
附录：[Winrar下载](https://juno.lanzous.com/iFH4jirht5g)
附录：[Bandizip下载](https://juno.lanzous.com/icO91irht7i)
{% endnote %}
