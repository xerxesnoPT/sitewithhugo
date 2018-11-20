---
title: "常用快捷键"
date: 2018-01-12T13:06:01+08:00
draft: false
categories: ['tools']
tags: ['skill']

---

## Tmux

<!--more-->
- ? : 列出所有帮助信息
- [ : 复制模式, vim操作模式，按下回车确定
- ] : 粘贴
- { : 切换pane
- , : 改变window的名字。
- . : 改变window的编号
- t : 显示时间
- alt+方向键: 调整pane大小.
- q : 显示面板编号


## spacevim 

- [ b: 跳至前一个buffer
- ] b: 后一个
- [ l: 跳至前一个错误。
- Tab: 窗口跳转
- <leader> + l p: 查看所有安装的插件
- spc + c + l: 快速注释


## vim命令

- `:[rang] copy {address}` : 可用`:co` 或者`:t`缩写
- `:[rang] move {address}` : 可用`:m`
- `:w !sudo tee%`: readonly文件修改
- `'<,'>`代表高亮区
- 按`@:` 重复上次使用的`:`命令.
- `:[rang] normal 「命令」` : 对指定行执行命令 exp: `:%normal A;` 每行结尾添加；

