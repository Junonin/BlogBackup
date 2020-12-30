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

到 *https://github.com/winsw/winsw/releases* 下载文件  **WinSW-x64.exe** 放在Nginx的安装目录下。
[【WinSW】工具介绍](https://cloud.tencent.com/developer/news/246538)
并且将其重命名为 **nginx-service.exe**
然后分别创建**nginx-service.exe.config**，**nginx-service.xml**文件,把这两个文件放在Nginx安装目录下。
如图：
![rLkVVf.png](https://s3.ax1x.com/2020/12/30/rLkVVf.png)

**nginx-service.exe.config**
```conf
<configuration>
   <startup>
     <supportedRuntime version="v2.0.50727" />    
     <supportedRuntime version="v4.0" />  
  </startup>
  <runtime>
     <generatePublisherEvidence enabled="false"/>   
  </runtime>
</configuration>
```

**nginx-service.xml**
```xml
<!-- nginx-service.xml -->
<service>
	<id>Nginx</id>
	<name>Nginx</name>
    <description>High Performance Nginx Service</description>
	<logpath>D:\Nginx\logs</logpath>
	<log mode="roll-by-size">
        <sizeThreshold>10240</sizeThreshold>
        <keepFiles>8</keepFiles>
    </log>
	<executable>D:\Nginx\nginx.exe</executable>
    <startarguments>-p D:\Nginx\</startarguments>
    <stopexecutable>D:\Nginx\nginx.exe</stopexecutable>
    <stoparguments>-p D:\Nginx -s stop</stoparguments>
</service>
```

用管理员权限在cmd执行命令安装Windows服务
```bash
nginx-service.exe install 命令可注册对应的系统服务
nginx-service.exe uninstall 命令可删除对应的系统服务
nginx-service.exe stop 命令可停止对应的系统服务
nginx-service.exe start 命令可启动对应的系统服务
```

- [Nginx安装成Windows服务配置下载](https://juno.lanzous.com/iLyYEjut4pc)

---


## Nginx常见场景代理转发配置

* 指向到公司官网或其他产品网（一级域名）
每个域名单独配置一个server即可，如HTTPS的配置如下
```conf
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

```conf

location  /xxx/ {

proxy_pass http://127.0.0.1:5000/;

}

```

会被代理到http://127.0.0.1:5000/test 这个url



###### 不带**/**

```conf
location  /xxx/ {

proxy_pass http://127.0.0.1:5000;

}

```

会被代理到http://127.0.0.1:5000/xxx/test这个url



###### 二级带**/**

```conf
location  /xxx/ {

proxy_pass http://127.0.0.1:5000/xxx/;

}

```

会被代理到http://127.0.0.1:5000/xxx/test这个url。



###### 二级不带**/**

```conf
location  /xxx/ {

proxy_pass http://127.0.0.1:5000/xxx;

}

```

会被代理到http://127.0.0.1:5000/xxxtest这个url。



> 注意：如果需要映射二级以下代理，需要把二级也一起映射，不然会找不到对应的层级



## 强制http转https访问

* 80端口部分server配置：

```conf
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

```conf
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

## Nginx负载均衡

```conf
server {
        #监听端口
        listen       33020;
        #listen       somename:8080;
        server_name  localhost;

       location / {
           #表示将所有请求转发到upstream_name的负载服务器组中配置的某一台服务器上。
           proxy_pass http://upstream_name;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #    root   html;
        #    index  index.html index.htm;
       }
    }

    #upstream配置负载服务器
    upstream upstream_name{
        server 192.168.1.136:33020;
        server 192.168.1.136:33022;
    }

```

|        轮询        |    默认方式     |
| :----------------: | :-------------: |
|       weight       |    权重方式     |
|       ip_has       |   依据ip分配    |
|     least conn     |  最少连接方式   |
|   fair（第三方）   |  响应时间方式   |
| url_hash（第三方） | 依据URL分配方式 |



### 内置负载策略

#### **轮询**

| fail_timeout | 与max_fails结合使用。                                        |
| ------------ | ------------------------------------------------------------ |
| max_fails    | 设置在fail_timeout参数设置的时间内最大失败次数，如果在这个时间内，所有针对该服务器的请求都失败了，那么认为该服务器会被认为是停机了， |
| fail_time    | 服务器会被认为停机的时间长度,默认为10s。                     |
| backup       | 标记该服务器为备用服务器。当主服务器停止时，请求会被发送到它这里。 |
| down         | 标记服务器永久停机了。                                       |

    注意：
    - 在轮询中，如果服务器down掉了，会自动剔除该服务器。
    - 缺省配置就是轮询策略。
    - 此策略适合服务器配置相当，无状态且短平快的服务使用

#### **weight**
权重方式，在轮询策略的基础上指定轮询的几率。例如：
```conf
#动态服务器组
upstream upstream_name {
  server localhost:8080  weight=2; #tomcat 7.0
  server localhost:8081; #tomcat 8.0
  server localhost:8082  backup; #tomcat 8.5
  server localhost:8083  max_fails=3 fail_timeout=20s; #tomcat 9.0
}
```
weight参数用于指定轮询几率，weight的默认值为1,；weight的数值与访问比率成正比，比如Tomcat 7.0被访问的几率为其他服务器的两倍。

    注意：
    - 权重越高分配到需要处理的请求越多。
    - 此策略可以与least_conn和ip_hash结合使用。
    - 此策略比较适合服务器的硬件配置差别比较大的情况。

#### **ip_has**
指定负载均衡器按照基于客户端IP的分配方式，这个方法确保了相同的客户端的请求一直发送到相同的服务器，以保证session会话。
这样每个访客都固定访问一个后端服务器，可以解决session不能跨服务器的问题。
```conf
#动态服务器组
  upstream upstream_name {
    ip_hash;  #保证每个访客固定访问一个后端服务器
    server localhost:8080  weight=2; #tomcat 7.0
    server localhost:8081; #tomcat 8.0
    server localhost:8082; #tomcat 8.5
    server localhost:8083  max_fails=3 fail_timeout=20s; #tomcat 9.0
  }
```
    注意：
    - 在nginx版本1.3.1之前，不能在ip_hash中使用权重（weight）。
    - ip_hash不能与backup同时使用。
    - 此策略适合有状态服务，比如session。
    - 当有服务器需要剔除，必须手动down掉。

#### **least_conn**
把请求转发给连接数较少的后端服务器。轮询算法是把请求平均的转发给各个后端，使它们的负载大致相同；
但是，有些请求占用的时间很长，会导致其所在的后端负载较高。这种情况下，least_conn这种方式就可以达到更好的负载均衡效果。
```conf
#动态服务器组
upstream upstream_name {
  least_conn;  #把请求转发给连接数较少的后端服务器
  server localhost:8080  weight=2; #tomcat 7.0
  server localhost:8081; #tomcat 8.0
  server localhost:8082 backup; #tomcat 8.5
  server localhost:8083  max_fails=3 fail_timeout=20s; #tomcat 9.0
}
```
    注意：

    - 此负载均衡策略适合请求处理时间长短不一造成服务器过载的情况。



### 第三方负载策略
第三方的负载均衡策略的实现需要安装第三方插件。

#### **fair**
按照服务器端的响应时间来分配请求，响应时间短的优先分配。
```conf
#动态服务器组
upstream upstream_name {
  server localhost:8080; #tomcat 7.0
  server localhost:8081; #tomcat 8.0
  server localhost:8082; #tomcat 8.5
  server localhost:8083; #tomcat 9.0
  fair;  #实现响应时间短的优先分配
}
```

#### **url_hash**
按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，要配合缓存命中来使用。
同一个资源多次请求，可能会到达不同的服务器上，导致不必要的多次下载，缓存命中率不高，以及一些资源时间的浪费。
而使用url_hash，可以使得同一个url（也就是同一个资源请求）会到达同一台服务器，一旦缓存住了资源，再此收到请求，就可以从缓存中读取。　
```conf
#动态服务器组
upstream upstream_name {
  hash $request_uri;  #实现每个url定向到同一个后端服务器
  server localhost:8080; #tomcat 7.0
  server localhost:8081; #tomcat 8.0
  server localhost:8082; #tomcat 8.5
  server localhost:8083; #tomcat 9.0
}
```

## 快速切换 Nginx 的 upstream

1. [treafik](https://containo.us/traefik/)
2. [ngx_http_upstream_conf_module](http://nginx.org/en/docs/http/ngx_http_upstream_conf_module.html)
3. [ngx_http_dyups_module](https://github.com/yzprofile/ngx_http_dyups_module)

```conf
lua 变量替代 proxy_pass 里写死的 upstream
set $backend "default";
rewrite_by_lua_block {
 ngx.var.backend="foo"
}
proxy_pass http://backend;
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


### Java环境导入509cer证书
```bash
keytool -import -v -trustcacerts -alias maven -file C:\Users\JUNO\Desktop\IrrServer.cer -storepass changeit -keystore C:\Program Files\Java\JDK\jre\lib\security\cacerts
```


> - Cloudflare开启后能临时禁用缓存 3 小时
> [Cloudflare Development Mode](https://support.cloudflare.com/hc/en-us/articles/200168246-Understanding-Cloudflare-Development-Mode)