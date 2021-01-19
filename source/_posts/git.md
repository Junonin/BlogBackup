---
title: Git
tags: Git
categories:
  - Tools
  - Git
top: 
abbrlink: 69c3279c
description: GIT学习记录
date: 2019-09-23 00:00:00
---

## 前言

> Git学习记录

## Git 工具更新
```sh
git --version
git update-git-for-windows
```

## Git config
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

- 文件名过长错误
```sh
git config --system core.longpaths true
```
- 禁止git自动将lf转换成crlf
```sh
git config --global core.autocrlf false
```
- 提交时转换为LF，检出时不转换
```sh
git config --global core.autocrlf input
```
- 拒绝提交包含混合换行符的文件
```sh
git config --global core.safecrlf true
```


## Git绑定远程仓库
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

## Git pull
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

## Git push
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

## Git status

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

## Git add

- `git add` 命令后面可以跟：
  1. 单个或多个文件名
  2. 如果是目录，git add将递归地跟踪该目录下的所有文件
  3. 代表所有文件
  4. 或者其他通配符

git add可以再次更新暂存区中的内容

## Git diff

- `git diff` 命令可以查看工作树、暂存区、最新提交(版本库)之间的差别：
  1. `git diff` :没添加其它选项，默认查看工作树与暂存区之间的差别
  2. `git diff --cached/--staged` ：对比暂存区和版本库之间的差别
  3. `git diff HEAD` ：HEAD是指当前分支中最新一次提交的指针，因此是用来查看工作树和版本库之间的差别

## Git commit

```sh
git commit -m "提交信息"
```

```sh
git add missed-file // missed-file 为遗漏提交文件
git commit --amend --no-edit
```
`--no-edit`表示提交消息不会更改，在 git 上仅为一次提交

- Git提供了一个跳过使用暂存区域的方式，只要在提交的时候，给 `git commit` 加上`-a`选项，Git就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过`git add`步骤。

## Git rm

- `git rm`支持如下的选项：
  1. `-n`，`--dry-run` ：演习
  2. `-q`，`--quiet` ：不列出删除的文件
  3. `--cached`：只从索引区删除，本地的文件还存在
  4. `-f`，`--force` ：忽略文件更新状态检查
  5. `-r` ：允许递归删除
  6. `--ignore-unmatch` ：即使没有匹配，也以零状态退出

## Git clone

```sh
#全克隆
git clone https://github.com/Junonin/BlogBackup.git   #HTTPS
git clone git@github.com:Junonin/BlogBackup.git        #SSH

#单一克隆
git clone -b master https://github.com/Junonin/BlogBackup.git  # -b branch_name 
git clone -b git_仓库_分支 --single-branch git_仓库_url
git clone -b master --single-branch https://github.com/Junonin/BlogBackup.git  #single-branch 拉取单一branch分支

#深度克隆
#--depth=commit_num 或者 --depth commit_num
git clone --depth=10 https://github.com/Junonin/BlogBackup.git  ##最近commit_num提交记录的代码

```
- `git clone`克隆:
  1. `git clone git_仓库_url` 获取全部branch内容，整体下载时间较长 & 所占磁盘空间较大。
  2. `git clone -b git_分支名称 git_仓库_url` 根上述 1. 结果一致
  3. `git clone -b git_分支名称 --single--branch git_仓库_url` 获取指定分支的代码
  4. `git clone --depth 10 git_仓库_url` 只会获取最近 xx（10条提交记录的）代码，默认是master分支， 如果想要指定分支，可以结合 `-b --single--branch` 使用！


## Git reset 

```sh
git reset –-soft    # 回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可；
git reset -–hard    # 彻底回退到某个版本，本地的源码也会变为上一个版本的内容，撤销的commit中所包含的更改被冲掉；

git reset HEAD <文件名>  #把暂存区的修改撤销掉（unstage），重新放回工作区。

# git版本回退，回退到特定的commit_id版本，可以通过git log查看提交历史，以便确定要回退到哪个版本(commit 之后的即为ID);
git reset --hard commit_id 
# 将版本库回退1个版本，不仅仅是将本地版本库的头指针全部重置到指定版本，也会重置暂存区，并且会将工作区代码也回退到这个版本
git reset --hard HEAD~1

# 修改版本库，保留暂存区，保留工作区
# 将版本库软回退1个版本，软回退表示将本地版本库的头指针全部重置到指定版本，且将这次提交之后的所有变更都移动到暂存区。
git reset --soft HEAD~1

```

## Git stash
stash命令可用于临时保存和回复修改，可跨分支。

> 注：在未add之前才能执行stash！！！！

```sh
#保存，save为可选项，message为本次保存的注释
git stash [save message]     
#所有保存的记录列表
git stash list
#恢复，num是可选项，通过git stash list可查看具体值。只能恢复一次
git stash pop stash@{num}
#恢复，num是可选项，通过git stash list可查看具体值。可回复多次
git stash apply stash@{num}
#删除某个保存，num是可选项，通过git stash list可查看具体值
git stash drop stash@{num}
#删除所有保存
git stash clear
```
