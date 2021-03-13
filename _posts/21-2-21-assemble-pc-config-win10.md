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
主板 B460-PRO WiFi6 ATX 1033
硬盘 西数SSD500G+HDD4T 989
内存 威刚XPG-Z1 16G 491
电源 鑫谷金牌600W 309
机箱 Aigo炫影黑 179
散热 九州风神大霜塔 179
合：5396 元
```

在 [msdn](https://msdn.itellyou.cn/) 下载最新版系统，使用 EasyU 制作 PE，安装 win10，数字激活（略）

# 软件清单

此表暂为新机清单，以后装的软件也会陆续加入

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

## 截图工具 Snipaste

QQ 截图还可以，但不想每次都打开 QQ，就在 微软商店 下了个 Snipaste

它有个优点，支持自定义文件名设置，如我设为时间格式 `yyMdd_HHmms`，若用 QQ 截图文件名为 `QQ截图_2021-03-01_23-19-07-254.png`，现在用 Snipaste 默认保存为 `21301_23197.png`

## 任务栏透明 TaskbarTools

微软商店有个 TranslucentTB，看名字也知道功能是"半透明任务栏"，而 TaskbarTools 吸引我的是它还提供把"开始"屏幕设为半透明

我用的绿色版免安装，就一个 exe 文件，这使我立即卸载了 TranslucentTB，当然，它也有绿色版，但我不想找了（问就是因为懒）

TaskbarTools 还提供把自己的图标从任务栏移除，这也给我的下一个工具提供想法

## 隐藏托盘图标 PSTrayFactory

像 Snipaste、TaskbarTools，甚至 Rainmeter 这些工具，我们使用它的功能，但极少或几乎不操作它，没必要时时待在任务栏

TaskbarTools 提供自隐藏，别的没提供，于是使用 [PS Tray Factory](http://www.pssoftlab.com/pstf_info.phtml) 这么一个提供隐藏托盘图标的工具

## 窗口多标签 WindowTabs

需要激活一下，否则只能合并三个标签。

## 快速查找文件 Wox + Everything

[Wox](https://github.com/Wox-launcher/Wox/releases) 可快速调出搜索框，快捷键 `Alt+空格键`

[Everything](https://www.voidtools.com/) 快速查找电脑上的文件

# 问题记录

## 去除快捷方式箭头[不建议]

1. 按 win+R，输入 "regedit"，打开注册表编辑器，找到 HKEY_CLASSES_ROOT\lnkfile

2. 在 lnkfile 中找到 IsShortcut，右击删除该键值，重启电脑

> 此方法可能导致下一个问题，还是老老实实用 360 去除

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

## 删除此电脑中的视频图片文档等文件夹

1. Win+R，输入 regedit 回车，找到 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace`

2. 找到对应键值，右键删除

- 3D 对象: {0DB7E03F-FC29-4DC6-9020-FF41B59E513A}
- 视频: {f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}
- 图片: {24ad3ad4-a569-4530-98e1-ab02f9417aa8}
- 文档: {d3162b92-9365-467a-956b-92703aca08af}
- 下载: {088e3905-0323-4b02-9826-5d99428e115f}
- 音乐: {3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}
- 桌面: {B4BFCC3A-DB2C-424C-B029-7FE99A87C641}

3. 右键底部任务栏 - 任务管理器 - 重启 “Windows 资源管理器”
