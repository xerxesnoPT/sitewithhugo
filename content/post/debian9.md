---
title: "debian9-日常"
date: 2018-05-11T13:06:01+08:00
draft: false
categories: ['linux']
tags: ['linux','system']
---

## Debian9的日常：

- 开启crontab日志
```
sudo vim /etc/rsyslog.conf
# 去掉cron.* 前面的注释
sudo systemctl restart rsyslog.conf
