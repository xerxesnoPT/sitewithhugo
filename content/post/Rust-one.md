---
title: "Rust学习-1 配置环境"
date: 2019-01-21T13:06:01+08:00
draft: false
categories: ['Rust']
tags: ['Language']

---

## Rust 下载
- 直接参照官网
- 讲下Cargo。感觉跟pipenv 相似.会创建一个`Cargo.toml`文件用于描述项目的依赖

``` toml
[package]
name = "guessing_game"
version = "0.1.0"
authors = ["xerxes"]
edition = "2018"
[dependencies]
rand = "0.3.14"
```

``` shell
cargo new [project] : 创建project目录
cargo build :编译
cargo run : 编译并运行
cargo check : 检查语法
cargo release: 进行版本发布
```
- rustup component: 添加工具链,添加代码格式化插件rustfmt

``` shell
rustup component add rust-src --toolchain nightly
rustup update nightly
rustup component add rustfmt
rustup component add rls --toolchain nightly
rustup component add rust-analysis --toolchain nightly
rustup component add rust-src --toolchain nightly
rustup default nightly
```

## vim 主要配置
- 首先安装好racer,fmt
- 进入YCM插件目录进行自动补全的编译

``` shell
./install.py --racer-completer
```

- 添加vim高亮插件等设置。详见.vimrc配置

