---
title: 安装 OpenCC
date: 21-10-11 01:42 +08
---

[TOC]

安装 [OpenCC](https://github.com/BYVoid/OpenCC) 时报错

```sh
$ npm i opencc
```

### 报错 1 python

```sh
gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable
```

看了系统环境变量，有设置 python 执行目录。查看 npm 配置时没有，尝试 npm 也配置

```sh
# 查看
$ npm config ls
# 设置
$ npm config set python "C:\Users\hobeas\AppData\Local\Microsoft\WindowsApps\python.exe"
```

后来发现，虽然有 `python.exe` 这个文件，但大小是 0KB。win10 没有默认支持 python，还是老老实实自己安装。先把设置项删除

```sh
$ npm config delete python
```

到 [python 官网](https://www.python.org/downloads/windows/) 下载安装略

### 报错 2 Visual Studio

再次执行 `npm i opencc` 时报错

```sh
gyp ERR! stack Error: Could not find any Visual Studio installation to use
```

好吧，缺少 `VS`，新系统就这样，见怪不怪~~，到 [Visual Studio 官网](https://visualstudio.microsoft.com/zh-hans/) 下载安装略

这有个插曲，点下载或重试时没反应，打开控制台，发现是微软页面代码出 bug 了，用打点调试获取的下载地址

### 报错 3 C++

安装完 `VS` 再执行 `npm i opencc` 时报错

```sh
gyp ERR! find VS checking VS2019 (16.11.31727.386) found at:
gyp ERR! find VS "D:\app\Microsoft\VS"
gyp ERR! find VS - "Visual Studio C++ core features" missing
```

`VS` 是找到了，缺少 `C++`

打开安装器 `Visual Studio Installer`，点修改，勾选 `使用 C++ 的桌面开发`，安装后重启

解决了，完！
