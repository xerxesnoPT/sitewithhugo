---
title: "use Heimdall"
date: 2018-03-05T13:06:01+08:00
draft: false
categories: ['phone']
tags: ['phone']
---

## 下载heimdall。  

网页提供的版本为1.4.0，download pit的时候会出错。浪费了半天时间。
解决方法直接去git拉取最新的1.4.2版本，自己编译下。  
1. 需要进入github直接source安装。[地址](https://github.com/Benjamin-Dobell/Heimdall.git)  
2. 在debian上安装需要使用cmake编译。安装依赖包    
```shell  
sudo apt-get install cmake, qt5-default, libusb-1.0.0-dev  
cd "heimdall's path"
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
```
3. 编译完成后会在build目录下生成bin目录。包含`Heimdall`, `Heimdall-frontend` 两个可执行文件。 
注意：此处需要把当前的heimdall文件复制到`/usr/bin/heimdall`下，否则`Heimdall-frontend`无法执行。
4. 把手机关机后进入download模式。插上usb，首先需要download pit。保存后直接可以进行recovery的刷入。
5. 这里说下root。root的话找到对应的cf-root包。解压后刷入recovery跟cache两个文件。然后进入recovery模式。会自动安装superSu。

## 资源

[root工具](http://download.chainfire.eu/397/CF-Root1/CF-Auto-Root-klte-kltexx-smg900f.zip?retrieve_file=1)   
[twrp](https://dl.twrp.me/klteduos/)   
[参考文档] (http://www.all-things-android.com/content/how-root-samsung-mobile-device)
