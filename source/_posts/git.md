---
title: Git
tags: Git
categories:
  - Tools
top: 
abbrlink: 69c3279c
description: GIT学习记录
date: 2019-09-23 00:00:00
---

## 前言

> GIT学习记录


## GIT config
> * 配置config
配置git个人信息和生成ssh密钥
打开`git bash`，输入

```
 git config --list
 git config --global user.name "JUNO"
 git config --global user.email "JUNOYAPIPI@gmail.com"
 ssh-keygen -t rsa -C "JUNOYAPIPI@gmail.com"
```
> * 然后在GitHub导入SSH密匙（C:\Users\JUNO\.ssh\id_rsa.pub）
<!--more-->

## git绑定远程仓库
```
git remote add origin https://github.com/junonin/junonin.github.io.git
```
然后查看是否绑定成功
```
git remote -v
```
注:删除远程仓库github(github是仓库名称)
```
git remote rm github
```

## git pull
**git pull命令的作用是：取回远程主机某个分支的更新，再与本地的指定分支合并。**
```
git pull = git fetch + git merge
```
基本用法：
```
git pull <远程主机名> <远程分支名>:<本地分支名>
```
例如：
```
git pull origin master:local
```
将远程主机origin的master分支抓到本地，并和本地local分支合并
```
git pull origin master
```
将远程主机origin的master分支抓到本地，并和本地当前分支合并
与pull相比fetch用法为：
```
git fetch origin master:local
git merge local
```
**相比起来git fetch更安全一些，因为再merge前，我们可以查看更新的情况，决定是否合并分支**

## git push
**git push命令的作用是：将本地版本库的分支推送到远程服务器上对应的分支。**
基本用法：
```
git push <远程主机名> <本地分支名>:<远程分支名>
```
例如：
```
git push
```
如果当前分支只有一个远程分支，那么主机名都可以省略，形如 git push，可以使用`git branch -r` ，查看远程的分支名
```
git push origin
```
如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支
```
git push origin master
```
如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建
```
git push origin ：refs/for/master
```
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，等同于 
```
git push origin –delete master
```








