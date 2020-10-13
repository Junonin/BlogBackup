---
title: WinSCP同步脚本
tags: WinSCP
categories:
  - Tools
top: 100
abbrlink: e9a5b8d2
date: 2020-08-29 00:00:00
---

创建xxx.bat脚本调用run.txt

```shell

@echo on
rem echo是off 不打印注释rem
title  同步文件
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
::     同步文件
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
cd "C:\Program Files (x86)\WinSCP\"
::以命令行方式同步数据     
set data=%date:~0,4%%date:~5,2%%date:~8,2%
"C:\Program Files (x86)\WinSCP\WinSCP.exe" /console /script=C:\Users\JUNO\Desktop\run.txt /log=C:\log\%data%-log.txt

pause
```



创建run.txt

```xml
# winscp.exe /console /script=sample.txt 

# Automatically answer all prompts negatively not to stall

# the script on errors

# option echo  on|off 

option echo off

# option batch on|off|abort|continue

option batch on

# option confirm  on|off 

#无需确认直接操作
option confirm off

# option transfer  binary|ascii|automatic 

#服务端如果没有该文件，则将本地文件删除

# option synchdelete  on|off

# option exclude clear | <mask>[;<mask2>...]

# option include clear | <mask>[;<mask2>...]

option synchdelete off

# open [ sftp|ftp|scp:// ][  [ :password ] @ ] <host> [ :<port> ]

# open user:password@example.com

# Connect   FTP地址

open ftp://yd-video:ABCabc123@183.236.23.5:33035

# Change remote directory

# 如果同步到远程FTP时,可用此命令转到远程某个目录下

cd /backup/	   

# Change local directory

# set to Self's working dir  设置需要同步到远程FTP的本地文件目录

lcd C:\Users\JUNO\Desktop\Learn

# Force binary mode transfer

# 使用二进制格式传送 

option transfer binary

# Download file to the local directory d:\

# 拉取文件到本地

# get examplefile.txt d:\

# synchronize local|remote|both [ <local directory> [ <remote directory> ] ]  

# 从远程同步到本地用Local;从本地同步到远程用Remote

synchronize remote  

# Disconnect

close

# Exit WinSCP

exit
```
