---
title: Tomcat服务
abbrlink: 9240bb8c
date: 2020-11-24 16:41:27
tags: Tomcat
categories:
  - JAVA
  - JDK
top:
description: Tomcat服务，端口占用，部署webapp
---

## 前言

> Tomcat服务，异常处理笔记


## Tomcat安装卸载服务

安装 cmd 根目录	```service.bat install Tomcat9```

卸载cmd根目录	```service.bat remove Tomcat9```



## Nt Kernel & System 占用80端口

`net stop http 按Y 确定`

`Sc config http start= disabled`

重启Apache



## Tomcat8080端口无法启动

查看端口占用情况
```netstat -ano|findstr 8080```

删除占用端口的PID
```taskkill /pid 18352 /f```

- 强制关闭8080端口

```bash
@echo off
setlocal enabledelayedexpansion
rem 8080端口
for /f "delims=  tokens=1" %%i in ('netstat -aon ^| findstr "8080"') do (
set a=%%i
goto js
)
:js
taskkill /f /pid "!a:~71,5!"
pause>nul
```


## Tomcat部署WAR包访问不带项目名的方式

1. 将项目打成WAR包放在Tomcat的webapps目录下

2. 在Tomcat的安装目录的conf下找到server.xml的文件，如：D:\apache-tomcat-9.0.8\conf\server.xml

3. 在Host标签里边添加

```xml
<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">

 <Context path="" docBase="myproject" reloadable="true" />
    
 </Host>
```


- Context标签内容，注意path填空，docBase为项目名称

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```
- 在此可以修改port的默认端口名8080

4. 再次访问即可携带项目名称或不带都可以访问到项目。如localhost:8080/或localhost:8080/myproject

##  webapps

> ***webapps*是用来存放运行工程的目录**

1. 在服务器上部署web项目时，直接将项目war包放入tomcat中的webapps文件下后重启tomcat后，war包会自动解压，这时访问项目的地址是ip+端口+**项目名称**就可以正常访问项目。

注：webapps目录和放在ROOT目录的区别是webapps不需要解压，ROOT需要解压；webapps访问项目需要加项目名，ROOT不需要加项目名。



## Tomcat的加载运行机制

1. Tomcat_Home/lib目录下的jar包

2. 然后加载 Tomcat_Home/ webapps/项目名/WEB-NF/lib的jar包

3. 最后加载的是 Tomcat_HOME/webapps/项目名/WEB-INF/ classes下的类文件

{% note danger modern %}
- 注：本机的 Tomcat_HOME为你本机的Tomcat路径
- 关键是： Tomcat 按上述顺序依次加载资源，当后加载的资源与之前加载的资源相重时，后加载的资源会继续加载并将之前的资源覆盖
{% endnote %}


{% note red 'fas fa-fan' simple%}
附录：[Kill8080.bat下载](https://juno.lanzous.com/igmH6iq3vra)
{% endnote %}