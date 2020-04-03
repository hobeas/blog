---
title: mongodb 命令小记
date:  20-4-1 23:26 +08
---

# 常用

```sh
# 查看路径
whereis mongodb

# 查看进程是否开启
ps aux |grep mongodb
# 关闭进程（13461是查到的pid）
kill -9 13461

# 查看端口是否开启
netstat -anpt|grep 27017

# 以配置文件启动
mongod -f mongodb.conf
# 关闭
mongod -f mongodb.conf --shutdown

# brew 启动 mongo
brew services start mongodb

# 查看库
show dbs
# 当前库
db
# 创建或切换库
use webtest1
# 删除库
db.dropDatabase()

# 查看表
show tables
# 查询 users 表的记录
db.users.find()
```



# 操作用户

```sh
# 切换到 admin 库
use admin
# 认证
db.auth('root', 'root123456')
# 查看全局所有账户
db.system.users.find().pretty()
# 查看当前库下的账户
show users

# 创建用户 centos
db.createUser({ user: 'centos', pwd: 'centos123456', roles: ['dbAdminAnyDatabase'] })
# 修改密码
db.changeUserPassword('centos', '123456')
# 修改角色
db.updateUser('centos', { roles: ['root'] })
# 授予角色（将在admin中创建的用户授予操作其他库的权限）
db.grantRolesToUser('<userName>', [{ role: '<role>', db: '<database>' }])
# 取消角色
db.revokeRolesFromUser('<userName>', [{ role: '<role>', db: '<database>' }])

# 切换到 test 库
use test
# 创建普通用户 test1
db.createUser({ user: "test1", pwd: "test123456", roles: [{ role: "readWrite", db: "securitydata" }] })
# 删除用户（需先切换到用户管理的数据库）
use test
db.dropUser('test1')
```

内置角色

- 数据库用户角色：read、readWrite
- 数据库管理角色：dbOwner、dbAdmin、userAdmin
- 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager
- 备份恢复角色：backup、restore
- 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
- 超级用户角色：root
- 内部角色：__system



# mongoose

- 条件操作符

  `$lt`、`$lte`、`$gt`、`$gte`、`$ne`

  < 、 <= 、 > 、 >=、!=


- 原子操作符

  `$and`、`$or`、`$nor`、`$in`、`$nin`、`$or`、`$not`、`$exists`

  `$elemMatch`(数组过滤)

  limit、skip、sort


- 数组修改器
  - $push 已有的数组末尾加入一个元素
  - $addtoset 往数组里加数据，若数组里已存在，则不会加入（避免重复）
  - $pop 删除数组元素，只能从头部（-1）或尾部（1）删除一个元素
  - $pull 删除数组元素，将所有匹配的元素删除


- 访问量+1，用 `$inc`

  db.findOneAndUpdate({ _id }, { $inc: { views: 1 } }, { new: true })


- 值为 null 不检查唯一性

  { unique: true, sparse: true }
