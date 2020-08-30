---
title: frp 内网穿透尝试
date: 20-8-30 15:35 +08
---

服务端 CentOS

```sh
# 下载压缩包
wget https://github.com/fatedier/frp/releases/download/v0.33.0/frp_0.33.0_linux_amd64.tar.gz

# 解压 .tar.gz
tar -xzvf frp_0.33.0_linux_amd64.tar.gz

# 重命名，进入
mv frp_0.33.0_linux_amd64 frp
cd frp

# 修改 frps.ini 文件，监听 HTTP 8081 端口
vi frps.ini
# '[common]' 下添加一行 'vhost_http_port = 8081'

# 启动，服务为 7000 端口
./frps -c ./frps.ini

# 防火墙开放 7000 端口，同时去云服务控制台的安全组规则添加 7000 端口
firewall-cmd --zone=public --permanent --add-port=7000/tcp; firewall-cmd --reload
```

客户端 Win

下载解压后，修改 frpc.ini 文件

```ini
[common]
; 你的服务器ip地址 x.x.x.x
server_addr = x.x.x.x
server_port = 7000

[web]
type = http
local_port = 8088
; 你设置的域名 my-domain.com
custom_domains = my-domain.com
```

```sh
# 运行
frpc.exe -c frpc.init
```

用浏览器访问 `http://my-domain.com/test.html` 时报错如下：

```
The page you visit not found.
Sorry, the page you are looking for is currently unavailable.
Please try again later.
The server is powered by frp.
```

在服务器 nginx 设代理时要设置 `proxy_set_header`

```
server {
  listen         80;
  server_name    my-domain.com;
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

完成 ✅

参考资料：

- [frp 项目](https://github.com/fatedier/frp)
- [issues: 通过 frp 穿透内网 web 服务失败](https://github.com/fatedier/frp/issues/782)
