---
title: "ssh 隧道搭建"
date: 2018-03-29T13:06:01+08:00
draft: false
categories: ['网络']
tags: ['内网穿透']
---

## 如何连接内网

1. 使用公网ip 创建ssh隧道。

- 远程端口转发（意思就是把内网A主机的ssh服务端22端口转发到公网主机上）  
```
ssh -CfNg -R 2222(公网ip主机上开启的端口): 127.0.0.1:22(内网A主机的22端口) 162.219.xxx.xxx(公网主机的ip地址)
```

- 本地端口转发 （意思就是把公网主机上刚才的2222端口转发到内网主机B的本地端口）
```
ssh -CfNg -L 4444(内网b主机开启的接受端口): 127.0.0.1:2222(公网主机的端口) 162.219.xxx.xxx(公网主机的ip地址)
```

完整的链接即是： 内网B:4444->公网主机:2222 -> 内网A：22
在主机B上

```
ssh usename@localhost -p 4444
```

- 参数解释  

```
-C：压缩数据传输。
-f ：后台认证用户/密码，通常和-N连用，不用登录到远程主机。
-N ：不执行脚本或命令，通常与-f连用。
-g ：在-L/-R/-D参数中，允许远程主机连接到建立的转发的端口，如果不加这个参数，只允许本地主机建立连接。
-L 本地端口:目标IP:目标端口
-T 不分配 TTY 只做代理用
-q 安静模式，不输出 错误/警告 信息
```
