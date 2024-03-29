---
title: 一些常用 dos 命令小记
date: 19-8-23 0:25 +08
---

[TOC]

# CentOS7

```sh
# 查看当前路径
pwd
# 查看命令手册
man pwd

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

# 查看隐藏文件
ls -a

# 将SSH公钥复制到主机，开启无密码登录
ssh-copy-id root@192.168.0.100

# 断开ssh，不关闭终端。按 Ctrl+D 或
logout

# 重启
reboot

# 修改 .bashrc 后使其生效
source ~/.bashrc

# 查看系统版本
cat /etc/redhat-release
# 查看内核版本
uname -a
# 查看可登录用户
cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1
# 查看登录历史
last
# 登录失败
lastb
```

## 防火墙 firewall

```sh
# 查看防火墙
systemctl status firewalld
# 开启
systemctl start firewalld
# 关闭
systemctl stop firewalld

# 查看开放的端口
firewall-cmd --list-ports
# 开放 27017 端口
firewall-cmd --zone=public --permanent --add-port=27017/tcp; firewall-cmd --reload
# 关闭端口
firewall-cmd --zone=public --permanent --remove-port=27017/tcp; firewall-cmd --reload
# 查看端口状态
firewall-cmd --query-port=27017/tcp
```

## 压缩解压 tar

```sh
# 解压 .tar.gz
tar -xzvf file.tar.gz
# 压缩 .tar.gz
tar -czf file.tar.gz ./a.md ./b.txt
# 解压 .tar
tar -xvf file.tar
# 压缩 .tar
tar -czf file.tar ./a.md

# 解压中文不乱码
unzip -O CP936 html5-200.zip
```

## 配置 nginx 自动启动

1. `vi /usr/lib/systemd/system/nginx.service`

2. 填写以下文本

```ini
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

3. 启动 `systemctl start nginx`

- 关闭 `systemctl stop nginx`

## 增加 ssh 端口号

1. 执行 `vi /etc/ssh/sshd_config`
2. 在 `Port 22` 下面加一行 `Port 7011`
3. 重启 ssh `systemctl restart sshd`

## 禁 IP

使用 iptables

```sh
# 查看
iptables -L -n --line-numbers

# 禁。-I Insert，-D Delete，INPUT 入站，DROP 放弃连接
iptables -I INPUT -s 123.45.6.7 -j DROP

# 解
iptables -D INPUT -s 123.45.6.7 -j DROP
# 或。1为行号
iptables -D INPUT 1

# 禁IP段
iptables -I INPUT -s 121.0.0.0/8 -j DROP

# 清空
iptables –flush
# 或
iptables -F

```

使用 ipset，暂时不需。

参考

- [CentOS 用 iptables 封 IP 的方法](http://www.360doc.com/content/15/0323/12/14900341_457370635.shtml)
- [Centos 禁止 IP、封 IP、解除封 IP 的方法](https://www.cnblogs.com/kwang-cai/articles/5236499.html)
- [iptables 详解](http://blog.chinaunix.net/uid-26495963-id-3279216.html)

# MacOS

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

# 安装软件时禁用检查，即允许任何来源
sudo spctl --master-disable
# 关闭
sudo spctl --master-enable
```

mac 禁止应用更新提示：
进入应用程序，选中该 app，右键选“显示包内容”，进入“Contents”文件夹，把里面的“\_MASReceipt”文件夹删了，OK！

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

# 合并音频和视频
ffmpeg -i video.mp4 -i audio.wav -c:v copy -c:a aac -strict experimental output.mp4

# 合并B站音视频
ffmpeg -i video.m4s -i audio.m4s -c:v copy -c:a copy output.mp4

# 去水印。水印左上角的x坐标、y坐标，w宽度、h高度
ffmpeg -i video.mp4 -filter_complex "delogo=x=680:y=5:w=150:h=40" output.mp4

# 改尺寸去黑边。a输出宽度，b输出高度，c左移距离，d上移距离
ffmpeg -i video.mp4 -vf crop=a:b:c:d output.mp4

# 裁剪视频。从3分10s处开始剪切，持续2分16秒
ffmpeg -i video.mp4 -ss 3:10 -t 2:16 -codec copy output.mp4
```

# git

```sh
# 配置账户
git config --global user.name "hobeas"
git config --global user.email "hobeas@qq.com"

# github 拉取私有库
git clone https://<username>:<password>@github.com/<username>/<project-name>.git

# 查看两个提交版本id修改了那些文件
$ git diff commit-id1 commit-id2 --stat

# 创建独立分支 test
git checkout --orphan test

# 后退两步的提交历史
git checkout HEAD^2
# 做一些事，如打包
npm run build
# 回到分支
git checkout develop

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

# pm2

```sh
# 保存
pm2 save

# 开机运行
pm2 startup
# 取消开机运行
pm2 unstartup systemd

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
date strftime 语法 (如%F %T)
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
strip_html 删除 HTML 标签
strip_newlines 删除换行符
times 相乘
truncate (truncatewords) 截短省略(单词个数)
uniq 删除数组冗余项
upcase 大写
url_decode URL 解码
url_encode URL 编码

# Chrome

## 保存整个网页为图片

1. 打开需要保存为图片的网页
2. 按 F12，接着按 Ctrl+Shift+P
3. 在红框内输入 full
4. 点击下面的“Capture full size screenshot”就可以保存整个网页为图片了

> 方法来自 [二小怪](https://www.cnblogs.com/ChouXiaoShou/p/ChromeScreenshot.html)
