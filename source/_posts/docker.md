---
title: Docker
tags: Docker
categories:
  - Docker
top: 
abbrlink: f5f9fa9b
description: Docker学习记录
date: 2020-08-24 00:00:00
---

## 前言

> Docker学习记录



## Dcoker依的赖环境

```sh
yum -y install yum-utils device-mapper-persistent-data lvm2

#阿里云镜像
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo			
#安装Docker
yum makecache fast
yum -y install docker-ce

```



## 设置中央仓库

```java
1.Docker官方的中央仓库：这个仓库是镜像最全的，但是下载速度较慢。
https://hub.docker.com/
2.国内的镜像网站：网易蜂巢，daoCloud等，下载速度快，但是镜像相对不全。
https://c.163yun.com/hub#/home 
http://hub.daocloud.io/ （推荐使用）
```

```sh
#需要创建 /etc/docker/daemon.json，并添加如下内容
 {
 	#这个私库的服务地址
	"registry-mirrors":["https://registry.docker-cn.com"],
	 #私库加速器
	"insecure-registries":["ip:port"]
 }
#重启两个服务
systemctl daemon-reload
systemctl restart docker
```



## Docker镜像下载

Docker镜像仓库： [Docker Hub](https://hub.docker.com/_/tomcat?tab=tags) 

```shell
#启动Docker
systemctl start docker	
#添加自启动列表
systemctl enable docker		
#测试
docker run hello-world
```

```shell
#搜索镜像
docker search tomcat		
#查看本地镜像
docker images -a	
#删除本地镜像
docker rmi 镜像标识
#拉取镜像
docker pull xxx[镜像地址]
```

---

## Docker镜像导入导出

```shell
如果因为网络原因可以通过硬盘的方式传输镜像，虽然不规范，但是有效，但是这种方式导出的镜像名称和版本都是null，需要手动修改
#将本地的镜像导出
docker save -o 导出的路径 镜像id
#加载本地的镜像文件
docker load -i 镜像文件
#修改镜像文件
docker tag 镜像id 新镜像名称：版本
```

---

## 容器的操作

```sh
#运行容器需要定制具体镜像，如果镜像不存在，会直接下载
#简单操作
docker run 镜像的标识|镜像的名称[:tag]
#常用的参数
docker run -d -p 宿主机端口:容器端口 --name 容器名称 镜像的标识|镜像名称[:tag]
#-d:代表后台运行容器
#-p 宿主机端口:容器端口：为了映射当前Linux的端口和容器的端口
#--name 容器名称:指定容器的名称

#查看全部正在运行的容器信息
docker ps [-qa]
docker ps -a
#-a 查看全部的容器，包括没有运行
#-q 只查看容器的标识



```

## 容器的启动，停止，删除等操作，后续会经常使用到

```shell
#重新启动容器
docker restart 容器id
#启动停止运行的容器
docker start 容器id
#停止指定的容器(删除容器前，需要先停止容器)
docker stop 容器id
#停止全部容器
docker stop $(docker ps -qa)
#删除指定容器
docker rm 容器id
#删除全部容器
docker rm $(docker ps -qa)
```



## 宿主机和容器的操作

```shell
#将宿主机的文件复制到容器内部的指定目录
docker cp 文件名称 容器id:容器内部路径
docker cp xxx.war 容器id:/usr/local/tomcat/webapps

#查看容器日志，以查看容器运行的信息
docker logs -f 容器id
#-f：可以滚动查看日志的最后几行

#可以进入容器的内部进行操作
docker exec -it 容器id bash
#退出容器内部
exit
```

## Docker启动mysql

```shell
docker run --restart=always --name mysql5.7.32 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -e TZ=Asia/Shanghai -d mysql:5.7.32 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-time_zone='+8:00'

--restart=always:开机启动
--name mc_mysql:自定义名称
-p 3306:3306:端口映射
-e MYSQL_ROOT_PASSWORD=root:设置密码
-e TZ=Asia/Shanghai:设置时区
-d daocloud.io/library/mysql:5.7.5:指定国内镜像
--character-set-server=utf8mb4:指定字符集
--collation-server=utf8mb4_unicode_ci:指定字符集
--default-time_zone='+8:00':默认时区
```

## 容器间的通讯方式

1. container ip：port 容器ip端口
2. master ip:port 宿主机ip端口
3. container link 容器间的链接
4. network docker的network桥接

##### 容器间的通讯--network

```shell
docker network ls

docker network create [别名]

#已经启动的镜像加入网络
docker network connect --alias [别名] [network别名] container
#断开镜像连接
docker network disconnect network别名] container

#将两个容器绑定在同一个network上，可以通过别名直接访问

```

