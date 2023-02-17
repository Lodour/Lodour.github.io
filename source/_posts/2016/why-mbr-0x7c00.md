---
title: 为什么 MBR 加载到内存的 0x7c00 处
comments: true
categories: [笔记]
tags: [Operating System]
mathjax: false
cc_license: true
date: 2016-12-03 23:35:47
updated: 2016-12-03 23:35:47
---

课本上提到 BIOS 会将 MBR (Master Boot Record) 加载到内存的 0x7c00 处。

然而并没有解释为什么不是在 0x0000 处，也没有解释这个迷之地址的由来。

<!--more-->

## IBM PC 5150

IBM PC 5150 是 IBM 中 x86 系列 PC 最早的一台电脑，使用了 **Intel 8088 (16bit)** 芯片和 16KiB RAM。

计算机通电后，调用 19 号中断，检查并读取存储在 ROM 中的 BIOS，并对硬件进行检查。

检查通过后，则读取引导设备第一个扇区 (512B) 的内容 (MBR)，并加载到内存 0x7c00 处。

因此实际上，0x7c00 是在 BIOS 中设定的。

## 为什么是0x7c00

当时计算机搭配的是 DOC 1.0 系统，需要的最少内存是 32KB，也就是对应着内存中 `0x0000~0x7fff` 部分。

8086/8088 芯片需要 0x0000~0x03ff 部分的内存来存储中断向量，之后是 BIOS 的数据区。

引导扇区是 512B，引导程序所需的数据和堆栈区需要额外的 512B，加起来是 1KB。

所以呢，32KB 的最后 1KB 就用来存储 MBR 了。

1KB 占用 `0x0400` 长度的地址，`0x7fff - 0x0400 + 0x1 = 0x7c00`

或者说，内存第 31KB 开始的地址就是 0x7c00，这也就是 0x7c00 的由来了。

## 其他

当操作系统启动以后，引导扇区就没有用处了，这部分内存就可以被 OS 使用。

其中 32KB 内存的分配情况大致如下：

```
+--------------------- 0x0000
| Interrupts vectors
+--------------------- 0x0400
| BIOS data area
+--------------------- 0x05__
| OS load area
+--------------------- 0x7c00
| Boot sector
+--------------------- 0x7e00
| Boot data/stack
+--------------------- 0x7fff
```

[Reference](http://www.glamenv-septzen.net/en/view/6)

## 引导程序

实际上，加载到 0x7c00 处的引导程序是一段被编译成二进制文件的汇编代码，例如：

```x86asm
; boot.asm
; By Roll
Start:
    org 07c00h            ; 程序加载到7c00处
    mov ax, cs
    mov ds, ax
    mov es, ax
    call ClearScreen      ; 清屏
    call DispStr          ; 输出字符串
    jmp $
ClearScreen:              ; (CH, CL) - (DH, DL) 清屏
    mov al, 0
    mov ah, 6
    mov bh, 00h           ; 黑底黑字
    mov cx, 0
    mov dh, 24
    mov dl, 79
    int 10h
    ret
MovCursor:                ; 移动光标到BH:(DH, DL)
    mov ah, 2
    mov bh, 0
    mov dx, 0
    int 10h
    ret
DispStr:                  ; 输出字符串ds:bp
    call MovCursor
    mov ax, BootMessage
    mov bp, ax
    mov cx, [BootMessageLen]
    mov ax, 01301h
    mov bx, 0000ah        ; 0ah = 黑底绿字
    mov dl, 0
    int 10h
    ret
BootMessage:
    db ">> Roll Operating System 1.0 <<", 13, 10
    BootMessageLen dw $ - BootMessage
FillBlank:
    times 510-($-$$) db    0  ; 510 -  (当前地址 - 起始地址) = 填补至510B
    dw 0xaa55              ; 512B
```

当然，由于引导程序是直接与 BIOS 交互，因此就基本是使用 10 号中断了。



