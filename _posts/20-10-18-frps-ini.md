---
title: frps.ini 注释
date: 20-10-18 18:28 +08
tags: 文档
---

```ini
# [common] 是必须的部分
[common]
# A literal address or host name for IPv6 must be enclosed
# in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80"
bind_addr = 0.0.0.0
bind_port = 7000

# udp 端口，帮助 udp 穿透 nat
bind_udp_port = 7001

# udp 端口，用于 kcp 协议，可与 bind_port 相同
# 若未设置，则在 frps 中禁用 kcp
kcp_bind_port = 7000

# 指定代理将侦听哪个地址，默认与 bind_addr 相同
# proxy_bind_addr = 127.0.0.1

# if you want to support virtual host, you must set the http port for listening (optional)
# Note: http port and https port can be same with bind_port
vhost_http_port = 80
vhost_https_port = 443

# response header timeout(seconds) for vhost http server, default is 60s
# vhost_http_timeout = 60

# TcpMuxHttpConnectPort specifies the port that the server listens for TCP
# HTTP CONNECT requests. If the value is 0, the server will not multiplex TCP
# requests on one single port. If it's not - it will listen on this value for
# HTTP CONNECT requests. By default, this value is 0.
# tcpmux_httpconnect_port = 1337

# set dashboard_addr and dashboard_port to view dashboard of frps
# dashboard_addr's default value is same with bind_addr
# dashboard is available only if dashboard_port is set
dashboard_addr = 0.0.0.0
dashboard_port = 7500

# dashboard user and passwd for basic auth protect, if not set, both default value is admin
dashboard_user = admin
dashboard_pwd = admin

# enable_prometheus will export prometheus metrics on {dashboard_addr}:{dashboard_port} in /metrics api.
enable_prometheus = true

# dashboard assets directory(only for debug mode)
# assets_dir = ./static
# console or real logFile path like ./frps.log
log_file = ./frps.log

# trace, debug, info, warn, error
log_level = info

log_max_days = 3

# disable log colors when log_file is console, default is false
disable_log_color = false

# DetailedErrorsToClient defines whether to send the specific error (with debug info) to frpc. By default, this value is true.
detailed_errors_to_client = true

# 指定使用哪个身份验证方法对frpc和frp进行身份验证
# 若指定 “token”，则 token 将被读入登录消息中
# 若指定 “oidc”，则使用 OIDC (Open ID Connect)的设置。默认值为“token”
authentication_method = token

# 指定是否在发送到 frps 的心跳包中含 token，默认 false
authenticate_heartbeats = false

# 指定是否在发送到 frps 的新工作连接中含 token，默认 false
authenticate_new_work_conns = false

# auth token
token = 12345678

# 指定用于 OIDC 验证的客户端 ID，默认是 ""
oidc_client_id =

# OIDC 密钥，默认 ""
oidc_client_secret =

# OIDC 观众，默认 ""
oidc_audience =

# 指定实现 OIDC 的 URL，用于获取 oidc token，默认 ""
oidc_token_endpoint_url =

# 心跳配置，不建议修改默认值，默认 90
# heartbeat_timeout = 90

# 只允许 frpc 绑定你列出的端口，若不设置则无限制使用任何端口
allow_ports = 2000-3000,3001,3003,4000-50000

# 如果超过最大值，每个代理中的 pool_count 将更改为 max_pool_count
max_pool_count = 5

# 每个客户端可使用的最大端口数，默认0表示无限制
max_ports_per_client = 0

# 是否只接受 TLS 加密的连接，默认 false
tls_only = false

# 若 subdomain_host 不为空，则可在 frpc 的配置文件中设置 http(s) 的子域
# 当子域为 test，主机为 test.frps.com
subdomain_host = frps.com

# 如果使用 tcp 流多路复用，则默认值为 true
tcp_mux = true

# 自定义 404 页面
# custom_404_page = /path/to/404.html

[plugin.user-manager]
addr = 127.0.0.1:9000
path = /handler
ops = Login

[plugin.port-manager]
addr = 127.0.0.1:9001
path = /handler
ops = NewProxy
```