---
title: frp 内网穿透尝试
date: 20-8-30 15:35 +08
---

# 服务端 CentOS7

```sh
# 下载压缩包
wget https://github.com/fatedier/frp/releases/download/v0.33.0/frp_0.33.0_linux_amd64.tar.gz

# 解压 .tar.gz
tar -xzvf frp_0.33.0_linux_amd64.tar.gz

# 重命名
mv frp_0.33.0_linux_amd64 frp

# 进入
cd frp

# 修改 frps.ini 文件
vi frps.ini
```

填写如下

```ini
[common]
; 服务为 7010 端口
bind_port = 7010
; 监听 HTTP 8081 端口
vhost_http_port = 8081
```

```sh
# 启动
./frps -c ./frps.ini

# 防火墙开放 7010 端口
firewall-cmd --zone=public --permanent --add-port=7010/tcp; firewall-cmd --reload
```

云服务控制台的安全组规则添加 7010 端口

> 注：授权对象 为内网客户端的公网 ip，以下仅为示例，也可不限 ip 0.0.0.0/0

| 授权策略 | 优先级 | 协议类型   | 端口范围  | 授权对象 | 描述 |
| -------- | ------ | ---------- | --------- | -------- | ---- |
| 允许     | 100    | 自定义 TCP | 7010/7010 | 8.8.8.8  | frp  |

# 客户端 Win7

下载解压后，修改 frpc.ini 文件

```ini
[common]
; 你的服务器域名或ip地址
server_addr = test.com
server_port = 7010

[web]
type = http
local_ip = 127.0.0.1
local_port = 8088
; 你设置的 web 域名
custom_domains = t1.test.com
```

```sh
# 运行
frpc.exe -c frpc.init
```

浏览器访问 `http://t1.test.com` 时报错如下：

```
The page you visit not found.
Sorry, the page you are looking for is currently unavailable.
Please try again later.
The server is powered by frp.
```

在服务器 nginx 设代理时要设置 `proxy_set_header`

```conf
server {
  listen         80;
  server_name    t1.test.com;
  location / {
    proxy_pass http://127.0.0.1:8081;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Port $server_port;
  }
}
```

本地服务器完成 ✅

# MacOS ssh 访问内网

1. 服务端防火墙开放 7011 端口

```sh
firewall-cmd --zone=public --permanent --add-port=7011/tcp; firewall-cmd --reload
```

2. 云服务控制台的安全组规则添加 7011 端口（如上）

3. 客户端修改 frpc.ini 文件，添加

```ini
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 7011
```

4. 创建新账户 user2，设置密码（略）

> 因为 win 账户在使用的同时不能被远程

5. mac ssh 指定端口连接

```sh
ssh -p 7011 user2@test.com
```

参考资料：

- [frp 项目](https://github.com/fatedier/frp)
- [issues: 通过 frp 穿透内网 web 服务失败](https://github.com/fatedier/frp/issues/782)
