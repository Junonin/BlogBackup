---
title: MongoDB
tags: MongoDB
categories:
  - Tools
  - SQL
top: 
abbrlink: 8ead567e
description: MongoDB
date: 2020-08-27 00:00:00
---

## 前言

> MongoDB学习记录

## Docker环境

```shell
#进入docker里的mongodb操作界面
docker exec -it mongo mongo admin
#设置管理员账户
db.createUser({ user: 'JUNO', pwd: '123456', 
roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
#授权登录
数据库内部
db.auth("JUNO","123456");
外部shell
mongo --authenticationDatabase  -u  -p

#修改密码
db.updateUser(
   "root",
   {
      pwd: "abc"
   }
)
```



## MongoDB数据库

```plsql
#查看所有数据库
#要数据库有数据才显示数据库
show dbs

#查看所有用户
show users

#如果数据库不存在，则创建数据库，否则切换到指定数据库。
use DATABASE_NAME

db.dropDatabase("DATABASE_NAME")


#创建和删除用户都要在对应的数据库下
use test;
db.dropUser("test");

#查看已有集合
show collections 
show tables

db.COLLECTION_NAME.drop()

```



## 增加

```plsql
增加一条_id不存在的数据
db.COLLECTION_NAME.insert(document)

如果 _id 主键存在则更新数据，如果不存在就插入数据
db.COLLECTION_NAME.insertOne(document)
db.COLLECTION_NAME.replaceOne(document) 

#定义变量插入
document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库'
});
db.col.insert(document)

#插入单条记录
db.collection.insertOne({"a": 3})
#插入多条记录
db.collection.insertMany([{"b": 3}, {'c': 4}])
```



## 删除 

```plsql
先查询
db.col.find({'name':'xxx'})
删除
db.col.remove({'name':'xxx'})

只想删除第一条找到的记录可以设置 justOne 为 1
db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```



## 修改

```plsql
#修改所有匹配的数据
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
#只会修改第一条匹配的数据
#需要设置 multi 参数为 true
db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})

$inc
对某个字段进行增减操作，若键值不存在便增加
db.col.update({"uid" : "201203"},{"$inc":{"size" : 1}})

$set
指定一个键并覆盖更新键值，若键不存在并创建
db.col.update({"uid" : "20120002","type" : "3"},{"$set":{"sname":"ssk"}})

$unset
用来删除键
db.col.update({'aaa':'1111'},{'$unset':{'size':1}})
删除{'size'：2}的键值

$push
数组类型的键添加一个数组元素，不过滤重复的数据。添加时键存在，要求键值类型必须是数组；键不存在，则创建数组类型的键。
db.col.update({'aaa':'1111'},{'$push':{'name':'xxx'}})
"name" : [ "xxx", "xxx" ]

$ne/$addToSet
给数组类型键值添加一个元素时，避免在数组中产生重复数据，$ne在有些情况是不通行的。
db.col.update({'aaa':'1111'},{'$ne':{'name':'xxx'}})
db.col.update({'aaa':'1111'},{'$ne':{'addToSet':'xxx'}})


$pop
删除数据元素
从数组的头部 -1
db.col.update({'aaa':'1111'},{'$pop':{'name':-1}})
从数组的尾部删除 1
db.col.update({'aaa':'1111'},{'$pop':{'name':1}})
从数组的尾部删除 0
db.col.update({'aaa':'1111'},{'$pop':{'name':0}})

$pull
从数组中删除满足条件的元素
db.col.update({'aaa':'1111'},{'$pull':{'name':'xxx'}})


```



## 查询

```plsql
#等于			'name':'xxx'	
db.col.find({'name':'xxx'})

#大于		>		$gt		（greater  than ）
db.col.find({'name':{$gt:20}})

#小于		<		$lt		 (less  than )
db.col.find({'name':{$lt:20}})

#大于等于	  >=	$gte	(greater  than or   equal to)
db.col.find({'name':{$gte:20}})

#小于等于	  <=	$lte	(less than  or equal to )
db.col.find({'name':{$lte:20}})

#不等于
$ne  != （not equal to）  {'age': {'$ne': 20}}

#在范围内
$in    {'age': {'$in': [20, 23]}}   注意用list

#不在范围内
$nin  (not in) {'age': {'$nin': [20, 23]}} 
这个方法可以计算某个值既不等于x也不等于y

匹配以M开头的名字
$regex (正则匹配） db.collection.find({'name': {'$regex': '^M.*'}})  
        
查找name属性存在
$exists   属性是否存在  {'name': {'$exists': True}}     
        
#类型判断
$type     类型判断        {'age': {'$type': 'int'}}    age的类型为int

#text类型的属性中包含Mike字符串
$text      文本查询      {'$text': {'$search': 'Mike'}}     

#查找多种条件
$or  查找多种条件   ({'$or':[{'name':'chen'},{'name':'wang'}]})
```



## MongoDB权限

```plsql
#创建用户
db.createUser(
  {
    user: "admin",
    pwd: "123456",
    roles: [
       { role: "userAdminAnyDatabase", db: "admin" },
       { role: "dbOwner", db: "Test" }
    ]
  }
)
```

```plsql
#更改用户权限
db.updateUser('admin',{roles:[{role:'userAdminAnyDatabase',db:'admin'}]})


db.updateUser( "abc",
{
roles:[
         { role : "readWrite", db : "abc"  },
    	 {role:"read", db:"testDB"}
    
      ]
}
);


#增加权限
db.grantRolesToUser( "<username>", [ <roles> ], { <writeConcern> } )

#回收权限
db.revokeRolesFromUser( "<username>", [ <roles> ], { <writeConcern> })

--db.grantRolesToUser("usertest", [{role:"read", db:"testDB"}])

```



## 角色权限

#### 数据库用户角色（Database User Roles）：

-  read：授予User只读数据的权限
-  readWrite：授予User读写数据的权限

#### 数据库管理角色（Database Administration Roles）：

-  dbAdmin：在当前dB中执行管理操作
-  **dbOwner：在当前DB中执行任意操作**
-  userAdmin：在当前DB中管理User

#### 备份和还原角色（Backup and Restoration Roles）：

-  backup
- restore

#### 跨库角色（All-Database Roles）：

-  readAnyDatabase：授予在所有数据库上读取数据的权限
-  readWriteAnyDatabase：授予在所有数据库上读写数据的权限
-  userAdminAnyDatabase：授予在所有数据库上管理User的权限
-  dbAdminAnyDatabase：授予管理所有数据库的权限

#### 集群管理角色（Cluster Administration Roles）：

-  clusterAdmin：授予管理集群的最高权限
-  clusterManager：授予管理和监控集群的权限
-  clusterMonitor：授予监控集群的权限，对监控工具具有readonly的权限
-  hostManager：管理Server





