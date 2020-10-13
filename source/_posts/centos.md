---
title: Centos
tags: Centos
categories:
  - Linux
top: 100
abbrlink: cab4360e
date: 2020-06-01 00:00:00
---

### 网络配置

查看网络配置

```shell
ip addr 	

ifup ens33 #启用网络

ifconfig     
cd /sbin   可以用    ls | grep "if" 查看是否存在ifconfig命令
sudo yum install net-tools
```

配置文件位置

```shell
cat /etc/sysconfig/network-scripts/ifcfg-eth33
```

```shell
DEVICE=eth0 #描述网卡对应的设备别名
BOOTPROTO=static #设置网卡获得ip地址的方式，选项可以为为static，dhcp或bootp
ONBOOT=yes #系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备

IPADDR=12.168.1.80 #只有网卡设置成static时，固定IP
NETMASK=255.255.255.0 #子网掩码
GATEWAY=192.168.174.2 #网关
DNS1=192.168.174.2 #DNS

NETWORK=192.168.1.0 #网卡对应的网络地址，也就是所属的网段
```

```shell
service network restart #重启网络配置
```



### SSH远程登录

```shell
yum list installed | grep openssh-server #查看是否安装了openssh-server
yum install openssh-server   #安装ssh
```

```shell
cat /etc/ssh/sshd_config    #ssh端口配置

Prot 22						#监听端口
ListenAddress 0.0.0.0		#监听地址
ListenAddress ：： 			#监听地址

PermitRootLogin yes			#允许远程登录
PasswordAuthentication yes   #开启使用用户名密码来作为连接验证
```

```shell
sudo systemctl start ssh.service     #启动sshd
ps -e | grep sshd  					# 服务是否已经开启
netstat -an | grep 22 				#22 号端口是否开启监听
sudo systemctl enable sshd				#添加自启动列表
```



### 防火墙端口

##### 开放端口

```shell
# 开放8080端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent  		

#关闭5672端口
firewall-cmd --zone=public --remove-port=5672/tcp --permanent 		

# 配置立即生效
firewall-cmd --reload					  							
```

 

##### 查看防火墙所有开放的端口

```shell
firewall-cmd --zone=public --list-ports
```

 

##### 关闭防火墙

如果要开放的端口太多，嫌麻烦，可以关闭防火墙，安全性自行评估

```shell
systemctl stop firewalld.service
```

 

##### 查看防火墙状态

```shell
firewall-cmd --state
```

 

##### 查看监听的端口

```shell
netstat -lnpt
```

![img](https://s1.ax1x.com/2020/09/30/0mOssJ.png)

###### PS:centos7默认没有 netstat 命令，需要安装 net-tools 工具

```shell
yum install -y net-tools
```

 

 

##### 检查端口被哪个进程占用

```shell
netstat -lnpt |grep 5672
```

![img](https://s1.ax1x.com/2020/09/30/0mOHeA.png)

 

##### 查看进程的详细信息

```shell
ps 6832
```

![img](https://s1.ax1x.com/2020/09/30/0mOqot.png)

 

##### 中止进程

```shell
kill -9 6832
```





























