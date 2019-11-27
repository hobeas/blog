---
title: 一些常用 dos 命令小记
date:  19-8-23 0:25 +08
---

[TOC]

# CentOS
```sh
# 查看当前路径
pwd
# 查看命令手册
man pwd

# 查看防火墙
systemctl status firewalld
# 开启
systemctl start firewalld
# 关闭
systemctl stop firewall

# 查看开放的端口
firewall-cmd --list-ports
# 开放 27017 端口
firewall-cmd --zone=public --permanent --add-port=27017/tcp; firewall-cmd --reload

# mac 上传文件到 Linux 服务器
# scp 文件名 用户名@服务器ip:目标路径
# scp -r 文件夹 用户名@服务器ip:目标路径
scp a.txt root@192.168.0.100:/var/www
# 下载整个 test 文件夹
scp -r root@192.168.0.100:/var/www/test /Users/hobeas/Desktop

# 查看时间、文件大小
ls -lht
# 查看修改时间，mtime，更改文件内容时
ls -l
# 查看状态时间，更改权限或属性时
ls -l --time=ctime
# 查看存取时间，如使用cat读取时
ls -l --time=atime

# 创建一个相对路径的超链接
ln -s ../archive

# 解压中文不乱码
unzip -O CP936 html5-200.zip

# 将SSH公钥复制到主机，开启无密码登录
ssh-copy-id root@192.168.0.100

# 断开ssh，不关闭终端。按 Ctrl+D 或
logout

修改 .bashrc 后使其生效
source ~/.bashrc
```

# MongoDB

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

# 查看所有数据库
show dbs
# 显示当前数据库
db
# 创建或切换数据库
use webtest1
# 删除数据库
db.dropDatabase()

# 创建用户管理员
use admin
db.createUser({user:"root",pwd:"root123456",roles:["userAdminAnyDatabase"]})
# 认证
db.auth('root','root123456')

# 创建普通用户
use test
db.createUser({user:"test1",pwd:"test123456",roles:[{role:"readWrite",db:"securitydata"}]})
```

访问量+1，用 `$inc`
findOneAndUpdate({ _id },{ $inc: { views: 1 } },{ new: true })

mongodb 值为null不检查唯一性
{unique: true, sparse: true}

条件操作符
$lt、$lte、$gt、$gte、$ne
< 、 <= 、 > 、 >=、!=

原子操作符：`$and`, `$or`, `$nor`

$in/$nin/$or/$not
$exists
数组过滤：$elemMatch

limit, skip, sort

数组修改器
$push 已有的数组末尾加入一个元素
$addtoset 往数组里加数据，若数组里已存在，则不会加入（避免重复）
$pop 删除数组元素，只能从头部（-1）或尾部（1）删除一个元素
$pull 删除数组元素，将所有匹配的元素删除


# Mac

```sh
# 查看端口占用的进程
lsof -i:8082
# 关闭进程
kill 8082

# 查看所有pid命令
top
# 查看svn pid
ps aux | grep svn

# 测试端口
telnet 192.168.0.100 27017

# 设置 LaunchPad 图标每行 10 个
defaults write com.apple.dock springboard-columns -int 10
# 重启 LaunchPad
defaults write com.apple.dock ResetLaunchPad -bool TRUE;killall Dock

```
mac 禁止应用更新提示：
进入应用程序，选中该app，右键选“显示包内容”，进入“Contents”文件夹，把里面的“_MASReceipt”文件夹删了，OK！



# Nginx

```sh
# CentOS7 安装 nginx
yum install nginx

# 启动
nginx
# 关闭
nginx -s stop
# 退出
nginx -s quit
# 重载
nginx -s reload

# 测试 nginx.conf 配置
nginx -t

# 查询主进程号
ps -ef|grep nginx

# 解决启动 nginx.pid 错误
nginx -c /etc/nginx/nginx.conf

```

在 Mac 上
安装路径：`/usr/local/Cellar/nginx/1.8.0`
配置文件路径：`/usr/local/etc/nginx/nginx.conf`
服务器默认路径：`/usr/local/var/www`

在 CentOS 上
配置文件路径：`/etc/nginx/nginx.conf`
服务器默认路径：`/usr/share/nginx/html`


# ffmpeg

```sh
# 安装
brew install ffmpeg

# 下载 ts 视频（找到m3u8文件）
ffmpeg -i http://www.xxx.com/xxx.m3u8 name.mp4
```


# git

```sh
# 配置账户
git config --global user.name "hobeas"
git config --global user.email "hobeas@qq.com"

# github 拉取私有库
git clone https://<username>:<password>@github.com/<username>/<project-name>.git

```

# ssh

```sh
# 1.校验本机是否已经生成SSH密钥
ls -al ~/.ssh
# 2.生成SSH密钥，注意修改最后的E-mail地址
ssh-keygen -t rsa -C hobeas@qq.com
# 3.复制SSH公钥
pbcopy < ~/.ssh/id_rsa.pub
```


# Liquid

or, and, contains, 
真值: 除了 nil 和 false 之外的所有值都是真值
数据类型: String, Number, Boolean, Nil, Array
剔除空白符: `{ {- -} }`、`{ %- -% }`（中间不能有空格，为了编译...）

注释: comment-endcomment
控制流: if-elsif-else-endif, case-when-else-endcase, unless-endunless
迭代: for-in-endfor(break,continue,limit,offset), reversed, cycle, tablerow(cols,limit,offset,range)
原始内容: raw
变量: assign, capture, increment, decrement

abs 绝对值
append 拼接追加
at_least 将数字限制在最小值
at_most 将数字限制在最大值
capitalize 首字母大写
ceil 向上取整
compact 删除数组中的所有 nil 值
concat 拼接数组, split 
date strftime语法 (如%F %T)
default 指定一个默认值
divided_by 将两个数相除
downcase 转为小写
escape 字符串转义
escape_once 转义(不修改已转义过的实体)
first 数组第一项
floor 向下取整
join 数组转字符串
last 数组最后一项
lstrip (rstrip) 删除左(右,两)侧所有空白符（制表符、空格和换行符）
map 从对象提取指定属性值，返回数组
minus 从一个数中减去另一个数
modulo 返回除法运算的余数
newline_to_br 换行符(\n)替换为<br>标签
plus 相加
prepend 之前附加
remove (remove_first) 删除
replace (replace_first) 替换
reverse 反序
round 就近舍入
size 数量,支持点标记
slice 切片
sort (sort_natural) 排序(不区分大小写)
split 字符串转数组
strip 删除两侧空白符
strip_html 删除HTML标签
strip_newlines 删除换行符
times 相乘
truncate (truncatewords) 截短省略(单词个数)
uniq 删除数组冗余项
upcase 大写
url_decode URL解码
url_encode URL编码


# Chrome

## 保存整个网页为图片

打开需要保存为图片的网页
然后按F12，接着按Ctrl+Shift+P
在红框内输入full
点击下面的“Capture full size screenshot”就可以保存整个网页为图片了

> 方法来自 [二小怪](https://www.cnblogs.com/ChouXiaoShou/p/ChromeScreenshot.html)

