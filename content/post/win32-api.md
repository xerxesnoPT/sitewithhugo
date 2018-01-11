---
title: "win32汇编(环境搭建)"
date: 2018-01-02T13:06:01+08:00
draft: false
categories: ['二进制']
tags: ['asm']
---

## API 函数返回值

Win32 API 返回值类型是一个dword。永远放在eax中。如果放不下，则eax中返回一个指针，或者提供一个缓存区地址。直接把数据返回到缓冲区中

## 函数声明

格式：
函数名 proto [距离] [语言] [参数1]:数据类型,[参数2]:数据类型,......

[]中的东西都可以省略

exp:
MessageBox Proto :dword,:dword,:dword,:dword

注意：
MessageBox 在User32.dll中有两个MessageBoxA(ANSI字符串), MessageBoxW(Unicode字符串)

## 标号

2种格式

```asm
标号名:  目的指令


标号名:: 目的指令
```

win32汇编中，使用`::`来表示全局标号.可以从一个子程序直接跳转进入 

`:`表示局部标号，在一个子程序中有效

`@@`标号。
- `@F`表示本条指令后的第一个`@@`标号.
- `@B`表示本条指令前...

## 局部变量

```asm
push ebp    ;栈底ebp入栈保存
mov ebp,esp  ; 把栈底设置为栈顶(设置子程序栈底)
add esp, fffffff8 ; 栈指针往上提升8个字节（可以看做在原来的栈中往上延伸一个新栈，用来存放子程序）
mov eax, dword ptr [ebp-04] ;第一个局部变量入栈
mov ax, word ptr [ebp-06] ;第二个
mov al, byte ptr [ebp-07] ;第三个

leave ; leave 相当于2个操作 mov esp,ebp              pop ebp  此时堆栈就返回了一开始的状态。
ret
```

## Charles Simonyi

b       byte
w       word
dw      dword
h       handle
lp      long point
sz      str end with zero
lpsz    long point point to sz
f       float
st      struct

## 获取模块api

`invoke GetModuleHandle, lpModuleName`

lpmodulename is the str name of the ModuleHandle

like

```asm

szUserDll   db      'User32.dll',0
            ...
            invoke  Getmodulehandle, addr   szUserDll
            .if     eax
                    mov hUserDllHandle, eax
            .endif

            ...
```

if the lpModuleName is NULL, then, we will get the caller's ModuleHandle.

we use it in source code

```asm
invoke  Getmodulehandle, NULL
mov     hInstance, eax
```

## what is handle

handle is just a number for Windows using.

## create windows

- register windows class

`invoke RegisterClassEx, addr @stWndClass`

args define in a struct named WNDCLASSEX.

```asm
WNDCLASSEX      STRUCT
cbsize          DWORD           ; length of the struct
style           ...             ; style
lpfnwndproc     ...             ; 
cbclsextra
cbwndextra
hinstance       ...             ; instance handle
hicon           ...             ; pic of the window
hcursor
hbrbackground
lpszmenunamne   ...             ; windows menu
lpszclassname   ...             ; address of the show str
hiconsm
WNDCLASSEX      ENDS
```

- create windows

`invoke CreateWindowEx, dwExStyle, lpClassName,lpWindowName,dwStyle,x,y,nWidth,nHeight,hWndParent,\
        hMenu,hInstance,lpParam`

## message loop

```asm
    .while  TRUE
            invoke  GetMessage, addr @stMsg,NULL,0,0
            .break  .if eax == 0
            invoke  TranslateMessage, addr @stMsg
            invoke  DispatchMessage, addr @stMsg
    .endw


MSG STRUCT
    hwnd        DWORD       ?
    message     DWORD       ?
    wParam      DWORD       ?
    lParam      DWORD       ?
    time        DWORD       ?
    pt          POINT       <>

MSG ENDS
```

`invoke GetMessage, lpMsg, hWnd, wMsgFilterMin,wMsgFilterMax`

## windows process

the address of the process is important.

code style:

`WindowProc proc    hwnd,uMsg,wParam,lParam`



        
## 附注：

这里记录下。在xp sp2中 shellcode 使用\x00b构造时候会有错误.
情况是这样. 查看user32.dll 中的messageboxa 函数.得到地址为77d10000+0004050b
在shellcode时 直接mov eax, 77d5050b 的机器码为b8 0b 05 d5 77 。 进行栈溢出测试时会发现b8后的数据被改为00 cc ... 这样。 最后只好使用meeeageboxa函数下的一个另一个函数地址。弹窗才可以显示。




