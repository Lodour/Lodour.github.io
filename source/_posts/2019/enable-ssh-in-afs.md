---
title: 设置 AFS 无密码登陆
comments: true
categories: []
tags: []
mathjax: false
cc_license: true
date: 2019-09-16 10:15:02
updated: 2019-09-16 10:15:02
---

设置免密码、免 Duo 登陆 CS @ UW-Madison 服务器。

<!--more-->

## 背景

学校的 CS 服务器使用 [AFS](https://en.wikipedia.org/wiki/Andrew_File_System) 和 [Kerberos](https://web.mit.edu/kerberos/) 对登陆请求进行认证，因此每次 ssh 或者 scp 都要输入密码、手动同意 Duo 请求。

记录一下使用 krb5 保存登陆的 ticket，并在之后的登陆过程中自动加载。

## 在本地保存登陆密码

krb5 有多种不同的版本，并且每个版本的调用方式不同。

### Mac 自带的 krb5

一般来说 Mac 会自带 krb5 工具，检测方法如下：

```bash
❯ which ktutil
/usr/bin/ktutil

❯ ktutil --version
ktutil (Heimdal 1.5.1apple1)
Copyright 1995-2011 Kungliga Tekniska Högskolan
Send bug-reports to heimdal-bugs@h5l.org
```

类似以上的输出表示正在使用 Mac 自带的版本，这个版本是非交互式的，优先使用。

使用如下命令将登陆密码保存到本地文件，注意域名必须大写：

```bash
ktutil -k ~/.ssh/krb5.key add -p gy@CS.WISC.EDU --kvno=1 -e aes256-cts-hmac-sha1-96
```

### Brew / Conda 的 krb5

特殊情况也可以使用 Brew 或者 Conda 来安装 krb5:

```bash
❯ brew install krb5
❯ which krb5
/opt/homebrew/opt/krb5/bin/ktutil

❯ conda install krb5
❯ which krb5
/opt/homebrew/Caskroom/miniforge/base/envs/.../bin/ktutil
```

这两个版本都是交互式的，保存密码的方式有所不同：

1. 执行 `ktutil`
2. 输入 `add_entry -password -p gy@CS.WISC.EDU -k 1 -e aes256-cts-hmac-sha1-96`
3. 输入登陆密码
4. 输入 `write_kt ~/.ssh/krb5.key`
5. 输入 `quit`

{% note warning %}
两种方法都需要将密码文件设置仅自己可读写 `chmod 600 ~/.ssh/krb5.key`
{% endnote %}

## 初始化登陆 Ticket

利用保存的密码初始化登陆 ticket，有效期为 30 天：

```bash
kinit -t ~/.ssh/krb5.key -l 30d gy@CS.WISC.EDU
```

学校的服务器不支持更长的时间，可以使用以下 alias 方便续期：

```sh
alias mykinit="/usr/bin/kinit -t ~/.ssh/krb5.key -l 30d gy@CS.WISC.EDU"
```

## 设置 SSH 使用 Ticket

在本地的 `~/.ssh/config` 中添加以下内容：

```
Host *
  GSSAPIAuthentication yes
  GSSAPIDelegateCredentials yes
Host svr1 svr2 svr3
  Hostname %h.cs.wisc.edu
  User gy
```

在本地的 `/etc/krb5.conf` 中添加以下内容：

```toml
[libdefaults]
default_realm = CS.WISC.EDU
dns_lookup_realm = true
dns_lookup_kdc = true

[domain_realm]
cs.wisc.edu = CS.WISC.EDU
.cs.wisc.edu = CS.WISC.EDU
```

此时使用 `ssh svr1` 就可以跳过密码和 Duo 登录了。

## 常见问题

### Permission Denied

遇到权限问题可能是由于错误使用 sudo 造成的，此时默认的密码存储位置是 `/etc/krb5.keytab`。

可以尝试使用 sudo 进行写入和初始化操作：

```sh
sudo ktutil -k /etc/krb5.keytab add -p gy@CS.WISC.EDU --kvno=1 -e aes256-cts-hmac-sha1-96
sudo kinit -t /etc/krb5.keytab -l 30d gy@CS.WISC.EDU
```

也可以临时初始化 ticket 来排除其他问题（需要输入密码）：

```sh
sudo kinit -l 30d gy@CS.WISC.EDU
```

### PyCharm 登陆失败

一般来说，只需要在 PyCharm 中配置使用 `ssh_config` 就可以免密码登陆。

对于其他未知问题，需要在菜单 `Help` 中选择 `Show Log in Finder` 进行 Debug。

### 其他 Debug 方法

下面是一些其他 krb5 的命令，需要视情况与 sudo 混合使用。

* 查看所有可用的 ticket：`klist`
* 查看所有保存的 ticket：`ktutil list`
* 删除所有缓存：`kdestory -A`
* 查看 krb5 的内部输出：`export KRB5_TRACE=/dev/stdout`

## 扩展

### 在 tmux 中离线访问 AFS

因为 AFS 的特殊原因，screen 或者 tmux 后台在退出登录后可能无法访问 AFS 内容。

此时需要在服务器中使用上面的方法再次设置和初始化 ticket，并将密码保存至 AFS 以外的地方。

保存以下内容至 `~/bin/afs` （也可以是其他名字，需要将目录加入 PATH 环境变量）

```sh
#!/bin/zsh

# Setting
name="gy@CS.WISC.EDU"
keyfile="/somewhere/outside/afs/krb5.key"

# DO NOT EDIT
cmd=(${(q)argv})
pagsh -c "kinit -k -t ${keyfile} ${name} && aklog && (${SHELL} -ic ${(qq)cmd}; kdestroy)"
```

此时，使用 `afs screen` 或者 `afs tmux` 就可以开启允许访问 AFS 的后台进程。
