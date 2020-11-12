---
title: JDK环境变量
tags: JDK
categories:
  - JAVA
  - JDK
abbrlink: bab53ddd
date: 2020-11-12 18:55:42
top:
---

## 前言

> JDK环境变量设置(JDK脚本bat)

## 安装JDK
1. 官网下载JDK安装 [JDK8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
2. JDK默认安装  *C:\Program Files\Java\JDK* 

## 配置环境变量
``` bash
@echo off

::获取管理员权限方法一：
%1 mshta vbscript:CreateObject("Shell.Application").ShellExecute("cmd.exe","/c %~s0 ::","","runas",1)(window.close)&&exit
cd /d "%~dp0"


::后面是jdk安装路径，更改成自己的jdk安装路径
setx /M JAVA_HOME "C:\Program Files\Java\JDK"

setx /M CLASSPATH ".;%%JAVA_HOME%%\lib\dt.jar;%%JAVA_HOME%%\lib\tools.jar"

setx /M PATH "%PATH%;%%JAVA_HOME%%\bin"


echo 设置成功

java -version

pause
``` 

{% note red 'fas fa-fan' simple%}
附录：[bat脚本下载](https://juno.lanzous.com/iHWYFibvtnc)
{% endnote %}