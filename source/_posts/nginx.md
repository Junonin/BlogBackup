---
title: Nginx
tags: Nginx
categories:
  - Nginx
top: 
description: Nginx学习记录
date: 2020-06-27 00:00:00
abbrlink: d60c4ae5
---


# Nginx


## 前言

> Nginx学习记录

## 启动 

```bash
start nginx
```

## 停止

```bash
nginx.exe -s stop
```

**完整有序停止**

```bash
nginx -s quit
```

## 重新载入Nginx(修改nginx.conf文件后)

```bash
nginx.exe -s reload
```

## 重新打开日志文件

```bash
nginx.exe -s reopen
```

## 强制关闭Nginx.exe

```bash
taskkill /f /t /im nginx.exe
```

## [Nginx开机自启动服务](https://www.cnblogs.com/liabin/p/11736113.html)

```bash
nginx-service.exe install 命令可注册对应的系统服务
nginx-service.exe uninstall 命令可删除对应的系统服务
nginx-service.exe stop 命令可停止对应的系统服务
nginx-service.exe start 命令可启动对应的系统服务
```
---

## Nginx常见场景代理转发配置

* 指向到公司官网或其他产品网（一级域名）
每个域名单独配置一个server即可，如HTTPS的配置如下
```xml
server {
    	client_max_body_size 10000M;  #上传大文件最大值
        listen          443 ssl;
        server_name     www.yourdomain.com; #修改为需要的一级域名即可
 
        ssl_certificate      cert.crt;
        ssl_certificate_key  cert.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        #ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

        location / {
                index  index.html index.htm index.php;
                index  proxy_set_header Host $host;
                index  proxy_set_header X-Real-IP $remote_addr;
                index  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://server_cluster; #后端服务器，具体配置upstream部分即可
            }
 
        }
```

* nginx location proxy_pass 后面的url 加与不加/的区别

> 首先是location进行的是模糊匹配
> * 没有“/”时，location /abc/def可以匹配/abc/defghi请求，也可以匹配/abc/def/ghi等
> * 而有“/”时，location /abc/def/不能匹配/abc/defghi请求，只能匹配/abc/def/anything这样的请求

以下用http://xxx.xxx.com/xxx/test 进行访问

###### 带**/**

```xml

location  /xxx/ {

proxy_pass http://127.0.0.1:5000/;

}

```

会被代理到http://127.0.0.1:5000/test 这个url



###### 不带**/**

```xml
location  /xxx/ {

proxy_pass http://127.0.0.1:5000;

}

```

会被代理到http://127.0.0.1:5000/xxx/test这个url



###### 二级带**/**

```xml
location  /xxx/ {

proxy_pass http://127.0.0.1:5000/xxx/;

}

```

会被代理到http://127.0.0.1:5000/xxx/test这个url。



###### 二级不带**/**

```xml
location  /xxx/ {

proxy_pass http://127.0.0.1:5000/xxx;

}

```

会被代理到http://127.0.0.1:5000/xxxtest这个url。



> 注意：如果需要映射二级以下代理，需要把二级也一起映射，不然会找不到对应的层级



## 强制http转https访问

* 80端口部分server配置：

```xml
server {
        listen  80;
        server_name     api.yourdomain.com;
        location / {
                rewrite ^/(.*) https://api.yourdomain.com/$1 permanent ;
                break;
        }
        error_page 497 https://$host:$server_port$request_uri;
    }
```
当用户通过HTTP 80访问时，nginx将强制转换为HTTPS 443访问。

* 443端口部分server配置：

```xml
server {
        listen       443 ssl;
        server_name  api.yourdomain.com;

        ssl_certificate      cert.crt;
        ssl_certificate_key  cert.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;
 
        #ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

        location / {
                proxy_pass http://tomcat_servers/$2;
                proxy_set_header Host $host:443;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
 
         location / {
			proxy_pass   https://192.168.1.132:444;
            root   html;
            index  index.html index.htm;
        }

        location /v1.0/ {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://tomcat_servers/v1.0/;
            proxy_cookie_path /v1.0/ /;
            proxy_redirect off;
        }

        location  /yd/ {
			proxy_pass   http://192.168.1.135:35086;
            root   html;
            index  index.html index.htm;
        }		
 
}
```

---

## Nginx证书

###  other

> server.crt 服务器证书
> ca.crt 中级证书

### nginx

> server.crt 服务器证书和中级证书的合并文件,适用于Nginx以及负载均衡、CDN、WAF等需要合并证书的情况

### apache

> server.crt 服务器证书
>
> ca.crt 中级证书