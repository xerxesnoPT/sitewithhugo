---
title: "常用快捷键"
date: 2018-11-12T13:06:01+08:00
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
- s :切换session

## spacevim 

- [ b: 跳至前一个buffer
- ] b: 后一个
- [ l: 跳至前一个错误。
- Tab: 窗口跳转
- <leader> + l p: 查看所有安装的插件
- spc + c + l: 快速注释


## vim命令
YCM使用python3 只需要把
`let g:ycm_python_binary_path = '/usr/local/bin/python3'`

- C+n,p : 补全时上下移动
- C+y :选中当前补全代码。按enter会回车
- `<leader>+;`: 自动补全
- `<leade>+jd`: 到函数定义处
- gd: 定位到rust函数定义处
- `<leader>+gd`: rust doc文档

surround 使用:

- ds": 删除 "
- cs"( : 把"换成(
- cst" a : 把"换成`<a>`标签
- ysiw" : Hello -> "Hello"

- yss" :添加整行 Hello world -> "Hello world"
- ySS" :添加整行并换行 Hello world ->

```shell
"
    Hello world
"
```
- 可视模式下S" : 添加"

- `:[rang] copy {address}` : 可用`:co` 或者`:t`缩写
- `:[rang] move {address}` : 可用`:m`
- `:w !sudo tee%`: readonly文件修改
- `'<,'>`代表高亮区
- 按`@:` 重复上次使用的`:`命令.
- `:[rang] normal 「命令」` : 对指定行执行命令 exp: `:%normal A;` 每行结尾添加；
- `:g/pattern/d`: 包含pattern的行删除
- `:v/pattern/d`: 不包含pattern的行删除
- 复制指定字符的行到寄存器a:

```shell
    :g/pattern/y A 
    :put a
```



