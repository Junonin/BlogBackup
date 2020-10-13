---
title: Typora图床
tags: Typora
categories:
  - Tools
top: 100
abbrlink: d67c4ae5
date: 2020-09-28 00:00:00
---

### Github

```json
{
 "picBed": {
  "github": {
   "repo": "Junonin/CloudImg",
   "token": "94ca52b56a70c00e03a80e2f215ea2ae1c4a9125",
   "path": "img/",
   "customUrl": "",
   "branch": "master"
  },
  "current": "github",
  "uploader": "github"
 },
 "picgoPlugins": {
  "picgo-plugin-smms-user": true
 }
}
```

### 七牛云

```json
{
  "picBed": {
    "uploader": "qiniu",
    "qiniu": {
      "accessKey": "p8ND0-TzhfLmILsCGOnjpdtRtv8m327vaf183Egi",
      "secretKey": "lA-4ecs6_4mTheA4I-Rdyc_Ul1SW5RuAYDD14CzH",
      "bucket": "junonin",
      "url": "http://junonin.clouddn.com",
      "area": "na0",
      "options": "",
      "path": ""
    }
  },
  "picgoPlugins": {
    "picgo-plugin-smms-user": true
  }
}
```

### SM.MS

```json
{
  "picBed": {
    "uploader": "smms", // 代表当前的默认上传图床为 SM.MS,
    "smms": {
      "token": "gFkSK6fWws1RfCZ3VYzIDZKwmxcNy1ty" //XXXX替换为你注册获得的token
    }
  },
  "picgoPlugins": {} // 为插件预留
}
```

