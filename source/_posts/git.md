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

```sh
 git config --list
 git config --global user.name "JUNO"
 git config --global user.email "JUNOYAPIPI@gmail.com"
 ssh-keygen -t rsa -C "JUNOYAPIPI@gmail.com"
```
> * 然后在GitHub导入SSH密匙（C:\Users\JUNO\.ssh\id_rsa.pub）
<!--more-->

## git绑定远程仓库
```sh
git remote add origin https://github.com/junonin/junonin.github.io.git
```
然后查看是否绑定成功
```sh
git remote -v
```
注:删除远程仓库github(github是仓库名称)
```sh
git remote rm github
```

## git pull
**`git pull`命令的作用是：取回远程主机某个分支的更新，再与本地的指定分支合并。**
```sh
git pull = git fetch + git merge
```
基本用法：
```sh
git pull <远程主机名> <远程分支名>:<本地分支名>
```
例如：
```sh
git pull origin master:local
```
将远程主机origin的master分支抓到本地，并和本地local分支合并
```sh
git pull origin master
```
将远程主机origin的master分支抓到本地，并和本地当前分支合并
与pull相比fetch用法为：
```sh
git fetch origin master:local
git merge local
```
**相比起来`git fetch`更安全一些，因为再merge前，我们可以查看更新的情况，决定是否合并分支**

## git push
**`git push`命令的作用是：将本地版本库的分支推送到远程服务器上对应的分支。**
基本用法：
```sh
git push <远程主机名> <本地分支名>:<远程分支名>
```
例如：
```sh
git push
```
如果当前分支只有一个远程分支，那么主机名都可以省略，形如 git push，可以使用`git branch -r` ，查看远程的分支名
```sh
git push origin
```
如果当前分支与远程分支存在追踪关系，则本地分支和远程分支都可以省略，将当前分支推送到origin主机的对应分支
```sh
git push origin master
```
如果远程分支被省略，如上则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建
```sh
git push origin ：refs/for/master
```
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，等同于 
```sh
git push origin –delete master
```

## git status

*`git status`* 命令用于显示当前Git仓库的状态
工作树和仓库在被操作的过程中，状态会不断发生变化。在Git操作过程中时常用`git status`命令查看当前状态

```sh
git status -s
```
> 文件前面会有相关符号，符号说明如下：
> ??：新添加文件，还未跟踪
> A：新添加到暂存区中的文件
> 出现在右边的M：修改过的文件，还没有添加到暂存区中
> 出现在左边的M：修改过的文件，已经放入暂存区了
> MM：被放入暂存区之后又被修改了

## git add

- `git add` 命令后面可以跟：
  - 单个或多个文件名
  - 如果是目录，git add将递归地跟踪该目录下的所有文件
  - .代表所有文件
  - 或者其他通配符

git add可以再次更新暂存区中的内容

## git diff

- `git diff` 命令可以查看工作树、暂存区、最新提交(版本库)之间的差别：
  - `git diff` :没添加其它选项，默认查看工作树与暂存区之间的差别
  - `git diff --cached/--staged` ：对比暂存区和版本库之间的差别
  - `git diff HEAD` ：HEAD是指当前分支中最新一次提交的指针，因此是用来查看工作树和版本库之间的差别

## git commit

```sh
git commit -m "提交信息"
```

- Git提供了一个跳过使用暂存区域的方式，只要在提交的时候，给 `git commit` 加上`-a`选项，Git就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过`git add`步骤。

## git rm

- `git rm`支持如下的选项：
  - `-n`，`--dry-run` ：演习
  - `-q`，`--quiet` ：不列出删除的文件
  - `--cached`：只从索引区删除，本地的文件还存在
  - `-f`，`--force` ：忽略文件更新状态检查
  - `-r` ：允许递归删除
  - `--ignore-unmatch` ：即使没有匹配，也以零状态退出