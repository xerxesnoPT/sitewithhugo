---
title: "asm-call,ret跟子程序 "
date: 2017-12-24T13:06:01+08:00
draft: false
categories: ['二进制']
tags: ['asm']
---

## ret

two steps

1. (ip)=((ss)*16+(sp))
2. (sp)=(sp)+2
like: pop ip

## retf

four steps

1. (ip)=((ss)*16+(sp))
2. (sp)=(sp)+2
3. (cs)=((ss)*16+(sp))
4. (sp)=(sp)+2

like: pop ip
      pop cs

#### 这两条指令主要是在函数调用后返回时候，获取栈中保存的ip。进行跳转回去。

## call
two steps

1. (sp)=(sp)-2
   ((ss)*16+(sp))=(ip)
2. (ip)=(ip) + 16 位位移

相当于：
 push ip
 jmp near ptr 标号

## call far ptr 标号
four steps

1. (sp)=(sp)-2
2. ((ss)*16+(sp))=(cs)
3. (sp)=(sp)-2
4. ((ss)*16+(sp))=(ip)

相当于：
    push cs
    push ip
    jmp far ptr 标号

## 转移地址在寄存器中的call指令

call 16位 reg
与普通call相同。

## 转移地址在内存中的call指令

两种格式

- call word ptr 内存单元地址，与普通call一样，push ip 即可
- call dword ptr 内存单元地址，相当于call far ptr

## call和ret的配合使用

call 是把call之后的ip地址保存在栈中，通过ret来从栈中取出。然后继续运行

## mul指令

乘法指令- 要么都是8位，要么都是16位

- 8位，一个默认放在AL中，另一个放在8位reg或者内存字节单位中，默认结果放在AX中
- 16位，一个默认在AX中，另一个放在16位reg或者内存字单元中,结果高位默认在DX中，低位在AX中。

## 模块化程序设计。
字程序标准框架

```
子程序开始：子程序中使用的寄存器入栈
            子程序内容
            子程序中使用的寄存器出站
            返回（ret，retf）
```











