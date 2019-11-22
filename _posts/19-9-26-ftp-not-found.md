---
title: ftp command not found
date:  19-9-26 2:27 +08
---

**问题**

今天在 finder 使用 FTP，却只能下载单个文件，不能下文件夹，也不能压缩。使用终端时报错
`-bash: ftp: command not found`

然后发现 telnet 也不能用
`-bash: telnet: command not found`


**原因**

系统版本：`macOS v10.14.3`
Mac 从 v10.13 版本开始取消内置 FTP 和 telnet 命令


**解决**

使用 brew 安装

```sh
brew install telnet
brew install inetutils
brew link --overwrite inetutils
```

会生成以下命令
- dnsdomainname
- ftp
- rcp
- rexec
- rlogin
- rsh
- telnet


# telnet 使用

查看正在运行的端口
`netstat -AaLlnW`

连接端口
`telnet 127.0.0.1 8085`


# ftp 使用

```sh
# 方式一
$ ftp server-ip

# 方式二
$ ftp
ftp> open server-ip
```


