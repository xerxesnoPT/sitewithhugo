---
title: "asm-记录01 "
date: 2017-12-22T13:06:01+08:00
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








