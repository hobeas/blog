---
title: Mac 修改终端用户名
date:  20-4-4 22:53 +08
---

打开终端，每行都有很长的 主机名+计算机名+文件夹名+用户名，而且还不是自己的，如：

```sh
xiaoheMacBook-Pro:Desktop xiaohe$ 
```

那就把这些乱七八糟的名称改一改

1. 修改主机名

```sh
sudo scutil --set HostName hobeas
```

2. 修改计算机名（共享名称）

```sh
sudo scutil --set ComputerName hobeas
# 或
sudo scutil --set LocalHostName hobeas
```

3. 修改用户名

打开 bashrc 文件

```sh
sudo vi /etc/bashrc
```

内容如下：

```sh
# System-wide .bashrc file for interactive bash(1) shells.
if [ -z "$PS1" ]; then
   return
fi
# \h 主机名，\W 当前目录名，\u 用户名
PS1='\h:\W \u\$ '
```

把最后一行 `\u` 改为自己想要的名称，如 `PS1='\h:\W hobeas\$ '`，甚至可以把它移除，如 `PS1='\h:\W \$ '`

`shift+:` 输入 `wq!` 保存

现在再打开终端，就只有 用户名+文件夹名 了，如下：

```sh
hobeas:Desktop $ 
```
