---
title: "frp内网穿透"
date: 2018-04-09T13:06:01+08:00
draft: false
categories: ['网络']
tags: ['内网穿透']
---

## frp使用
- 上篇文章中讲了使用ssh搭建隧道。git上面有一个go语言的开源项目frp。用来进行专业的内网穿透。
- [git地址](https://github.com/fatedier/frp). 支持tcp，udp，http，https协议。

## 具体操作

- 首先在我的公网主机A(debian7 64位)上下载frp的release版本。
```
wget https://github.com/fatedier/frp/releases/download/v0.16.1/frp_0.16.1_linux_amd64.tar.gz
```

- 编写配置文件。这边注意到frp分为frps跟frpc。看名字就知道。A上使用server端.

vim frps.ini
```
[common]
bind_port = 7000 /这里表示绑定的端口
dashboard_port = 7500 /这里的是frp网页服务的端口。
dashboard_user = xxxx /frp网页服务的账号
dashboard_pwd = xxxx  /..密码
```
之后开启服务
```
./frps -c frps.ini
```

- 客户端（需要内网穿透）的主机B的设置。使用的是树莓派3b。

- 下载arm版本的frp首先

- 编写配置文件。此时使用的是frpc

vim frpc.ini
```
[common]
server_addr = x.x.x.x /公网ip地址
server_port = 7000  /公网主机A服务端绑定的端口7000

[ssh]
type = tcp /使用tcp。可以看下stcp。更加安全
local_ip = 127.0.0.1  /本地ip映射
local_port = 22 /ssh端口
remote_port = 6000 /绑定到公网ip的端口
use_compression = true
```
开启服务
```
./frpc -c frpc.ini
```

- 全部配置完成后，内网树莓派的22端口已经通过公网ip:6000转发出来了。进行ssh连接时只需要

```
ssh pi@公网ip -p 6000
```

## 后续操作

- A跟树莓派B再进行开机自启frp服务。编写下脚本就可以时刻控制内网中的树莓派了。
- 可以通过访问公网ip:7500来进行网页浏览链接情况
- 顺便记下服务器改时区。  

```
rm /etc/localtime

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```


