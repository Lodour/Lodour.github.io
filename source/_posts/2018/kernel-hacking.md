---
title: 如何编译并测试 Linux 内核
date: 2018-12-21 16:01:45
updated: 2018-12-21 16:01:45
categories: [笔记]
tags: [Linux, Operating System]
mathjax: false
cc_license: true
---

编译 Linux 内核，并在用户态 Linux 进程（User Mode Linux）或实际环境中进行测试。

<!--more--> 

## 获取内核源码
* 在 https://www.kernel.org 下载最新版本的内核代码，如 `linux-4.19.12.tar.xz`。
* 使用 `tar -xvf linux-4.19.12.tar.xz` 解压。

## User Mode Linux（UML）
### 介绍
用户态 Linux 在 http://user-mode-linux.sourceforge.net 有比较详细的介绍，其主要目的在于将内核编译为独立的可执行文件 `linux`，并作为用户态的进程运行，从而允许比较方便地对内核进行调试。为了使用编译用户态 Linux，需指定环境变量 `ARCH` 为 `um`，或在每次 `make` 命令中添加 `ARCH=um` 参数。不过因为指定了特殊的架构，用户态 Linux 的缺点就在于一些仅由其他架构如 `x86-64` 和 `arm` 等支持的特性就无法使用了。

### 编译
* 设置编译架构 `export ARCH=um`
* 清理编译环境 `make mrproper`
* 设置默认编译选项 `make defconfig`
* 通过菜单设置编译选项 `make menuconfig`
* 最终编译 `make`

### 文件系统
仅依靠 Linux 的可执行文件是无法启动的，还需要提供至少一个文件系统，并挂载为系统内的根目录 `/` 或某个设备 `/dev/ubda` 上。这里所说的文件系统实际上是一个镜像文件（Image），包含了默认 Linux 文件系统的一些内容，根据情况还可能嵌入了一些工具包，如 Glibc 等，但是会导致镜像文件偏大。可以参考在 http://fs.devloop.org.uk 列出的一些文件系统。

此处选择 64 位的 Gentoo，使用 `bunzip -d Gentoo-AMD64-root_fs.bz2` 解压后即可得到其镜像文件。

### 启动
* 使用命令 `./linux ubda=Gentoo-AMD64-root_fs` 启动用户态 Linux 进程。
* 其他更详细的启动参数可以参考 `./linux --help`。

## 实际环境
### 介绍
在实际环境部署内核时，需要将新编译的内核安装至系统目录，修改引导项，重新启动后才能进入新编译的内核。

### 编译
* 整体编译流程和 UML 相同，只是不需要另外指定 `ARCH`。
* 可以生成默认的配置文件，也可以从 `/boot` 目录下复制当前系统的配置文件。

### 安装
* 安装模块 `make modules_install`
* 安装内核 `make install`

### 启动
若使用 Grub 进行引导启动，则引导文件为 `/boot/grub/menu.lst`，并且在安装内核的过程中会提示是否自动修改。之后可以选择把新内核放在第一个，并设置 `fallback=1`，从而在新内核启动失败后自动切换为旧内核。
