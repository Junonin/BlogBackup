---
title: Hello World
tags: Texts
categories: Hexo
top: 
abbrlink: 4a17b156
date: 2019-06-01 00:00:00
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).
<!--more-->
## Quick Start

### 创建一个新发布 Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 启动服务 Run server

``` bash
$ hexo server
$ hexo s && hexo g
```

More info: [Server](https://hexo.io/docs/server.html)

### 生成静态文件 Generate static files

``` bash
$ hexo generate
$ hexo s && hexo g
```

More info: [Generating](https://hexo.io/docs/generating.html)

### 部署到远程仓库 Deploy to remote sites

``` bash
$ hexo deploy
$ hexo g -d 
```



## 重新部署恢复 Hexo

```
npm install hexo-cli -g
```

> * 保留_config.yml，theme/，source/，scaffolds/，package.json，.gitignore，gulpfile.js 这些项目在目录下`Git bash`

```
npm install
npm install hexo-deployer-git --save
hexo clean && hexo g && hexo s
```

查看是否本地可以运行[Hexo](http://localhost:4000/)，网站正常后可以发布到远程仓库

```
hexo d
```


More info: [Deployment](https://hexo.io/docs/deployment.html)

