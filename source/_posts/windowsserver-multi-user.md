---
title: windows server 服务器配置多用户登录
abbrlink: f6bc6e38
date: 2020-12-07 16:06:14
description: windows server 服务器配置多用户登录
tags: Windows Server
categories:
#  - Tools
  - Windows Server
top:
---

## 前言

> 因为防止多个用户同时需要访问服务器导致服务器导致另一个用户正在使用的服务器被挤掉退出
> 参考：[Windows 10中的多个RDP（远程桌面）会话](https://www.mysysadmintips.com/windows/clients/545-multiple-rdp-remote-desktop-sessions-in-windows-10)
> 参考：[windows server 2012 r2服务器配置多用户登录](https://blog.csdn.net/zl570932980/article/details/77671597)
> 参考：[Windows Server 2012 实现多个用户同时远程桌面登陆](https://lighti.me/2858.html)
> 参考：[RDP Wrapper Library by Stas'M (Github)](https://github.com/stascorp/rdpwrap)

## 正文

> 因为Windows系统可以通过修改本地组策略来改变RDP的登录属性来使多用户同时远程服务器

- 1.在 **【运行】** 输入 **gpedit.msc** 然后进行回车进入本地组策略
![DxQOqs.png](https://s3.ax1x.com/2020/12/07/DxQOqs.png)
![DxlflF.png](https://s3.ax1x.com/2020/12/07/DxlflF.png)

- 2.在左侧栏选择 **【计算机配置】** -> **【管理模板】** -> **【Windows组件】** 然后再右栏里选择 **【运程桌面服务】**
![Dx1VXQ.png](https://s3.ax1x.com/2020/12/07/Dx1VXQ.png)

- 3.选择 **【远程桌面会话主机】** 点击打开
![Dx3Pb9.png](https://s3.ax1x.com/2020/12/07/Dx3Pb9.png)

- 4.选择 **【连接】** 点击打开
![Dx3GPP.png](https://s3.ax1x.com/2020/12/07/Dx3GPP.png)

- 5.选择 **【将远程桌面服务用户限制到单独的远程桌面服务会话】** 点击打开，修改为 **【已禁用】** 操作完后选择 **【应用】** 最后 **【确定】**
![Dx34aR.png](https://s3.ax1x.com/2020/12/07/Dx34aR.png)

- 6.我这里修改的最大连接数是3，根据自己的需要修改连接数吧！
选择 **【连接】** 点击打开再选择 **【限制连接的数量】** 点击打开默认是已禁用的，选择 **【已启用】** ，然后选择 **【应用】** 最后 **【确定】**
![Dx8pJP.png](https://s3.ax1x.com/2020/12/07/Dx8pJP.png)

这样就可以同时登录多用户登录了，如下：
![Dx8swd.png](https://s3.ax1x.com/2020/12/07/Dx8swd.png)