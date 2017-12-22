---
title: "asm-记录01 "
date: 2017-12-01T13:06:01+08:00
draft: false
categories: ['二进制']
tags: ['asm']
---

## 伪指令dd

定义双字型数据。 

```asm

data segment
    db 1
    dw 1
    dd 1
data ends
```
- 第一个数据为01h，1个字节 data:0
- 第二个数据为0001h，2个字节 data:1
- 第三个数据为0000 0001h，4个字节 data:3

## dup

用来进行数据的重复
```asm
data segment
db 3 dup (0) ;相当于定义了3个字节，db 0,0,0
db 3 dup (0,1,2) ;db 0,1,2,0,1,2,0,1,2
data ends
```

## offset

伪指令,取得标号的偏移地址
```asm
code segment
start: mov ax, offset start ;相当于mov ax,0
    s: mov ax, offset s ; 相当于mov ax ,3 因为start这段指令占用3个字节
code ends
```

## jmp指令

无条件跳转，可只修改ip，也可同时修改cs跟ip

#### 根据偏移位置的jmp（只修改ip）

`jmp short 标号`
- 实现的是段内短转移，ip修改范围为8位（-128~127）

`jmp near ptr 标号`
- 16位的ip位移(-32768~32767)

`jmp 16位reg`
- 与上一条相同

`jmp word ptr 内存单元地址（段内转移）`
- 与上相同

#### 转移的目的地址在指令中的jmp指令（同时修改ip跟cs）
`jmp far ptr 标号`
- 段间转移,远转移

`jmp dword ptr 内存单元地址（段间转移）`

- （cs）=（内存单元地址+2） ; 高位存cs
- （ip）=（内存单元地址） ; 低位存ip

## jcxz指令
jcxz指令为有条件转移指令。所有的有条件转移指令都是短转移。
ip 控制在八位（-128~127）之间。

`jcxz 标号`

- 如果(cx)=0,转移到标号处执行. (cx)!=0时,程序继续向下执行
- 相当于`if((cx)==0) jmp short 标号`

## loop 指令
所有的循环指令都是短转移。对应修改ip 8位（-128~127）
指令格式:
`loop 标号`

- (cx)=(cx)-1;
- 如果 (cx)!=0,(ip)=(ip)+8位位移.
- 如果(cx)==0,程序向下运行
- 相当于`if((cx)!=0) jmp short 标号`

	
