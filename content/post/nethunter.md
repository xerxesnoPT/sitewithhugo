---
title: "nethunter "
date: 2018-03-01T13:06:01+08:00
draft: false
categories: ['phone']
tags: ['phone']
---

# TODO:刷入三星S5 nethunter

## 准备步骤：

- 下载工具及所需rom：  
    1. [Linux安装`adb`](https://wiki.lineageos.org/adb_fastboot_guide.html) 
    2. 下载s5的cm固件，使用kltechn版本的。[下载链接](https://download.lineageos.org/kltechn)
    3. 下载[Heimdall套件](http://glassechidna.com.au/heimdall/#downloads)来对三星设备进行recovery的刷入。
    4. [TWRP](https://dl.twrp.me/kltechn/)
    5. [nethunter roma](https://build.nethunter.com/nightly/3.15.4-20170101-0951/) 注意，这里内核使用kltechn的

## 操作步骤：

- 刷入recovery操作:
    1. 关机，进入download模式。 按住[音量—] 跟[home]跟[电源]键盘
    2. usb 连接到电脑
    3. 开启一个终端  
    ```
    heimdall print-pit
    ```
    4. 使用heimdall刷入TWRP。重复1，2步骤。开启终端  
    ```
    heimdall flash --RECOVERY twrp-xxx.img --no-reboot
    ```
    5. 亮蓝灯表示recovery已经刷入
    6. 断开usb
    7. 手动重启下进入recovery模式.关机，按住[音量+] 跟[home]跟[电源]键盘

- 刷入cm rom：
    1. 把rom包放在`/sdcard`目录下   
    ```
    adb push filename.zip /sdcard/
    ```
    2. 使用Wipe->Advanced Wipe。
    3. Swipe to Wipe。
    4. Install
    5. navigate to `/sdcard` . 选择rom包
    6. 确定安装

- 刷入的kali nethunter:
    1. 需要选择需要刷入的 kali nethunter 安装包：arm64 是 64位，armhf32位，找和你 cpu 结构一致的，full 是完整版，minimal 是简版。
    2. 刷入手机对应的nethuer内核。kernel-nethunter 表示是内核，klte 是型号, touchwiz 是基于三星原厂 rom,marshmallow 是 安卓 6.0,lollipop 是 5.x 这里我这边是 cm 是通刷的。所以内核也是通刷三星的。
