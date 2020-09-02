---
title: "PE-Struct"
date: 2020-09-02T13:06:01+08:00
draft: false
categories: ['二进制']
tags: ['asm']
---

## NT头数据结构
- **SIGNATURE**:-> DWORD: 4个字节 50450000h('PE'00)
- **IMAGE_FILE_HEADER**
- **IMAGE_OPTIONAL_HEADER32**
<!--more-->

## IMAGE_FILE_HEADER :20个字节
```c
typedef struct _IMAGE_FILE_HEADER {
    WORD Machine; //每个cpu都会有一个machine值，Inter x86为0x014C
    WORD NumberOfSections; //文件中存在的节区数量，大于0
    DWORD TimeDateStamp;
    DWORD PointerToSymbolTable;
    DWORD NumberOfSymbols;
    WORD SizeOfOptionalHeader; //指出NT头IMAGE_OPTIONAL_HEADER32的长度
    WORD Characteristics;  //用来标识文件的属性0x0002h->可执行 0x2000h->DLL文件
} IMAGE_FILE_HEADER, *PIMAGE_FILE_HEADER;
```

## IMAGE_OPTIONAL_HEADER32 : PE头结构体中最大
- Magic: IMAGE_OPTIONAL_HEADER32时为10B，64时为20B
- AddressOfEnterPoint: EP的RVA值。最先执行的代码起始地址
- ImageBase: 一般而言。可执行exe是0x00400000， DLL是0x10000000， EIP寄存器的值为ImageBase+AddressOfEnterPoint
- SectionAlignment，FileAlignment: 节区在内存的最小单位，节区在磁盘文件中的最小单位。磁盘文件或内存的节区大小必须为最小单位的整数倍
- SizeOfImage: PE Image在虚拟内存中所占空间的大小
- SiseOfHeader: 整个PE头的大小。
- SubSystem: 用来区分系统驱动(*.sys) 与普通的可执行文件。
    - 1->Drive 2->Gui 3->Cui
- NumberOfRvaAndSizes: 用来指定DateDirectory数组(默认16)的个数。PE通过这个看实际数组大小，所以可能大小不是16.
- DateDirectory: 结构体


## 节区头
- **IMAGE_SECTION_HEADER**:
    - VirtualSize: 内存中节区所占大小
    - VirtualAddress: 节区起始地址(RVA)
    - SizeOfRawData: 磁盘文件中节区所占大小
    - PointerToRawData: 磁盘文件中节区起始位置
    - Characteristics: 节区属性

### RVA to RAW
1. 查找RVA所在节区
2. RAW = RVA - VirtualAddress + PointerToRawDAta


# IAT impot address table 导入地址表

## IMAGE_IMPORT_DESCRIPTOR
> 记录着PE文件要导入哪些库文件
> 导入多少库就存在多少个IMAGE_IMPORT_DESCRTPIOR结构体
```c
typedef struct _IMAGE_IMPORT_DESCRIPTOR {
    union {
	DWORD Characteristics; 
	DWORD OriginalFirstThunk;  // INT( Import Name Table) address (RVA)
    };
    DWORD TimeDateStamp;
    DWORD ForwarderChain;
    DWORD Name;  //库的名字的string 地址 (RVA)
    DWORD FirstThunk;   // IAT( Import Address Table) address (RVA)
} IMAGE_IMPORT_DESCRIPTOR;

typedef struct _IMAGE_IMPORT_BY_NAME {
    WORD Hint;
    BYTE Name[1];
    } IMAGE_IMPORT_BY_NAME, *PIMAGE_IMPORT_BY_NAME;
```

## IMAGE_IMPORT_DESCRIPTOR的内容在PE体中，位置信息在PE头中
> IMAGE_OPTIONAL_HEADER32.DataDirectory[1].VirtualAddress的值
