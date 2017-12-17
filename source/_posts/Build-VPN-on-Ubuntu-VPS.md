title: How to build VPN on an ubuntu VPS
date: 2015-12-25 23:37:42
categories: 技术
tags:
	- VPN
	- Ubuntu
---
利用[DigitOcean](www.digitocean.com)上Ubuntu-14.04的VPS搭建自己的vpn，这是我的[推广链接](https://www.digitalocean.com/?refcode=f489e85e2d21)。

<!--more-->

**注: 我的vps公网ip为`aaa.bbb.ccc.ddd`**

* 首先利用ssh远程登录到vps
* 安装pptpd服务
``` sh
$ sudo apt-get install pptpd
```

* 编辑配置文件
``` sh
$ sudo nano /etc/pptpd.conf
```

* 配置文件最下加入以下内容,0-100即表示分配的ip段
``` sh
localip aaa.bbb.ccc.ddd
remoteip aaa.bbb.ccc.0-100
```

* 设置dns
``` sh
$ sudo nano /etc/ppp/pptpd-options
```

* 找到`#ms-dns`，加入以下内容
``` sh
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

* 设置vpn账号
``` sh
$ sudo nano /etc/ppp/chap-secrets
```

* 账号格式，以tab键相隔，保留双引号
``` sh
"your_username"   *   "your_password"   *
```

* 重启服务
``` sh
$ sudo /etc/init.d/pptpd restart
```

* 设置转发，并去掉`net.ipv4.ip_forward=1`这一行的注释
``` sh
$ sudo nano /etc/sysctl.conf
```

* 接下来的一些操作
``` sh
sudo sysctl -p
sudo apt-get install iptables
sudo iptables -t nat -A POSTROUTING -s aaa.bbb.ccc.0/24 -o eth0 -j MASQUERADE
sudo iptables-save > /etc/iptables-rules
sudo nano /etc/network/interfaces
```

* 末尾加入
``` sh
pre-up iptables-restore < /etc/iptables-rules
```

* 继续操作
``` sh
sudo iptables -A FORWARD -s aaa.bbb.ccc.0/24 -p tcp -m tcp --tcp-flags SYN,RST SYN -j TCPMSS --set-mss 1200
sudo iptables-save > /etc/iptables-rules
```

* 修复一个bug
``` sh
sudo nano /etc/init.d/pptpd
```

* 找到下面这一行，修改为下面的第二行
``` sh
status_of_proc "$PIDFILE" "$DAEMON" "$NAME" && exit 0 || exit $?
status_of_proc -p "$PIDFILE" "$DAEMON" "$NAME" && exit 0 || exit $?
```

* 重启服务，查看状态，显示` * pptpd is running`则为成功
``` sh
/etc/init.d/pptpd restart
/etc/init.d/pptpd status
```

现在，用任意vpn工具按照刚才设置的内容进行链接即可。