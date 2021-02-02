### 重新部署恢复 Hexo

```shell
$ npm install hexo-cli -g
```

### 安装git依赖，方便部署

> 保留_config.yml，theme/，source/，scaffolds/，package.json，.gitignore，gulpfile.js 这些项目在目录下Git bash

```shell
$ npm install
$ npm install hexo-deployer-git --save
$ hexo clean && hexo g && hexo s
```

### 查看是否本地可以运行[Hexo](http://localhost:4000/)发布

```shell
$ hexo new "文件名"  # 新建文章
$ hexo g  # 生成静态网页
$ hexo s  # 本地预览
$ hexo d  # 部署上传到git
```
