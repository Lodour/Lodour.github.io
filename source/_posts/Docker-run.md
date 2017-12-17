title: 安装并使用Docker
categories: Docker
tags:
  - Docker
  - 容器
date: 2017-01-17 13:15:56
---

在Mac OS上安装并使用Docker的笔记。

<!--more-->

## 下载

直接去[官网下载](https://www.docker.com/products/docker#/mac)。

当然，可以看到旁边一段说明，这里截取最后一段：

> Requires Apple macOS Yosemite 10.10.3 or above 
> For previous versions get [Docker Toolbox](https://www.docker.com/products/docker-toolbox)

看了下自己的系统是`OS X EI Captian 10.11.6`，看起来没有什么问题。

## 准备

安装并打开以后报错，说安装了4.3.28版本的VirtualBox，无法运行。

后来找到官方文档，才发现有更详细的[安装说明](https://docs.docker.com/docker-for-mac/#/what-to-know-before-you-install)，提到了：

> VirtualBox prior to version 4.3.30 must NOT be installed (it is incompatible with Docker for Mac)
> > Note: If your system does not satisfy these requirements, you can install [Docker Toolbox](https://docs.docker.com/toolbox/overview/), which uses Oracle Virtual Box instead of HyperKit.

印象中我是没有安装过VirtualBox的，而是安装了VMware Fusion，不知道是不是顺带装上去的，索性搜索所有名字包含`VirtualBox`或者`VBox`的文件强行删掉了。

这个时候打开Docker仍然提示安装了VirtualBox，纠结了一下觉得可能是已经写到环境变量之类里了？于是重启解决了。

## 加速

看了看文档，首先想拉一个ubuntu的镜像过来，然而`docker pull ubuntu`却提示网络连接失败，难道是被墙了？

于是找到国内的镜像加速服务[DaoCloud](https://www.daocloud.io/)，注册账号，进入控制台，可以在导航栏看到“加速器”选项。

> 右键点击桌面顶栏的 docker 图标，选择 Preferences ，在 Advanced 标签下的 Registry mirrors 列表中加入下面的镜像地址:
> > http://391ac645.m.daocloud.io
> 点击 Apply & Restart 按钮使设置生效。
> Docker Toolbox 等配置方法请参考[帮助文档](http://guide.daocloud.io/dcs/daocloud-9153151.html#docker-toolbox)。

Docker重启后，`docker pull ubuntu`很快就下好了。

## 使用
> 参考[官方文档](https://docs.docker.com/)
