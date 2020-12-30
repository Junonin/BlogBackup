---
title: WebService
abbrlink: c05121c7
date: 2020-11-27 16:30:14
description: WebService
tags: WebService
categories:
#  - Tools
  - WebService
top:
---

## 前言

> WebService学习记录
> 因为公司的WebService是用.net做的，业务端是用java来显示，需要将WebService转成jar包调用。


## Webservice

可以使用Eclipse的**Web Service Client**生成代码

默认生成的代码不带认证信息 

需要添加 -XadditionalHeaders  参数



生成jar包  D:\Doc  使用wsimport的-clientjar  不包含源码打包成jar

```bash
wsimport -keep -d D:\Doc -s D:\Doc -p com.yd.service -clientjar D:\Doc\service-1.0.3.jar -encoding utf-8 -verbose -XadditionalHeaders http://192.168.1.131:33046/YdDatacenterCloud.CloudCenter/CloudWebServicer/?wsdl
```



生成jar包  D:\Doc 包含源码打包成jar

```bash
wsimport -keep -d D:\Doc -s D:\Doc -p com.yd.service -encoding utf-8 -verbose -XadditionalHeaders http://www.xxxxxx.com:xxx/ServerService?wsdl
```

在D:\Doc目录下启动命令行将com和META-INF一起打成jar包

```bash
jar cvf service-1.0.2.jar com META-INF
```



更新MANIFEST.MF文件

```bash
jar cvf service-1.0.2.jar com
```



```bash
jar -uvfm service-V1.0.0.jar MANIFEST.MF
```

