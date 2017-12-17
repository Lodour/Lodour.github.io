title: Create Your Own CMD on OSX
date: 2016-01-29 18:01:36
categories: 技术
tags: bash
---
平常会用一些自己写的脚本，每次用都要cd到目录下再执行，索性把常用的脚本都用简单的命令连接起来。

<!--more-->

# 用到的文件

`~/.bash_profile`文件用于设置一些环境变量，当用户登录时候仅执行一次

`~/.bashrc`每次打开新的shell时，该文件被读取

# 设置
* 编辑`~/.bash_profile`文件，若没有则新建一个，添加以下内容
``` sh
source ~/.bashrc
```

* 编辑`~/.bashrc`文件，若没有则新建一个，按以下格式添加内容
``` sh
# .bashrc

# 设置脚本的路径
MY_SCRIPT_DIR=/Users/User/Scripts/

# 设置命令
alias sqlmap='python $MY_SCRIPT_DIR/Tools/sqlmap/sqlmap.py'
```

# 生效

执行以下命令，使刚才添加的命令立即生效

``` sh
source ~/.bash_profile
```

之后每次启动电脑都会自动生效，并且使用别名是支持添加参数的。
