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

### 新问题：Windows Kits

过几天偶然看到在 D 盘根目录多了个 `Windows Kits` 文件夹，很碍眼，把它移到 `D:\app\Microsoft\`，并在注册表

`计算机\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Microsoft SDKs\Windows\v10.0`

把 `InstallationFolder` 的值设为 `D:\app\Microsoft\Windows Kits\10\`

又过几天，项目要安装新模块，发现报了个 VS 的错

```sh
npm ERR! D:\app\Microsoft\VS\VC\Tools\MSVC\14.29.30133\include\cstdio(12,10): fatal error C1083: �޷��򿪰����ļ �: ��stdio.h��: No such file or directory [F:\wks\bc\spd-s\node_modules\opencc\build\opencc.vcxproj]
```

信息有乱码，应该是 `Windows PowerShell` 显示问题，无关紧要。第一猜测是 `Windows Kits` 文件夹移动了，做个软连接

```sh
# 以管理员身份运行 cmd
mklink /d "D:\Windows Kits" "D:\app\Microsoft\Windows Kits"
```

可以了！

D 盘还是有个碍眼的 `Windows Kits` 文件夹，即便只是软链接，之后再看看能不能改动，先这样~~
