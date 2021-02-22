---
title: 组装电脑，安装 win10 后一些记录
date: 21-2-21 22:16 +08
tags: 系统
---

[TOC]

# 新电脑配置

组装了台电脑，配置如下

```
CPU i7-10700 2216
主板 B460-PRO WiFi ATX 1033
硬盘 西数SSD500G+HDD4T 989
内存 威刚XPG-Z1 16G 491
电源 鑫谷金牌600W 309
机箱 Aigo炫影黑 179
散热 九州风神大霜塔 179
合：5396 元
```

在 [msdn](https://msdn.itellyou.cn/) 下载最新版系统，使用 EasyU 制作 PE，安装 win10，数字激活（略）

# 新机装软件清单

```
chrome
Surfshark
git
vscode
node
MongoDB
Robo3T
qq
potPlayer
7-Zip
```

# 问题记录

## 去除快捷方式箭头

1. 按 win+R，输入 "regedit"，打开注册表编辑器，找到 HKEY_CLASSES_ROOT\lnkfile

2. 在 lnkfile 中找到 IsShortcut，右击删除该键值，重启电脑

## 点击固定到任务栏的快捷图标报错

报错内容：该文件没有与之关联的应用来执行该操作。请安装应用，若已经安装应用，请在“默认应用设置”页面中创建关联。

好像是上述的 "去除快捷方式箭头" 导致的，参考 [Dexter Lien 的解决方法](https://answers.microsoft.com/zh-hans/windows/forum/all/%E8%AF%A5%E6%96%87%E4%BB%B6%E6%B2%A1%E6%9C%89/457027ff-7e4f-4d90-9313-308e1122ddc9)

快捷箭头去除了个寂寞

## 克隆私有库

1. 首次使用 git，配置一下用户名和邮箱

> 注：xxx 指代用户名、计算机名等

```sh
git config --global user.name xxx
git config --global user.email xxx@qq.com
```

> 可在 `C:\Users\xxx\.gitconfig` 查看配置结果

2. 生成 RSA 公私钥；配置公钥到 github（略）

```sh
ssh-keygen -t rsa -C xxx@qq.com
```

> 省略 -C 及值则自动使用计算机名；可在 `C:\Users\xxx\.ssh` 目录下看到生成了 "id_rsa.pub" 和 "id_rsa" 公钥私钥文件

3. 现在可克隆自己的私有库了

```sh
git clone git@github.com:xxx/test.git
```

> RSA 公钥后续可用于 SSH 免密登录等
