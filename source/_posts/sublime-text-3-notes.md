title: Sublime Text 3 折腾笔记
date: 2016-03-26 21:54:21
categories:
  - 技术
tags:
  - "Sublime Text 3"
  - C++
  - Python
---

从零开始折腾Sublime Text 3
> For C++, Python and Markdown

<!--more-->

# 准备工作

## 安装Sublime Text 3

* 在Sublime Text的[官网](http://www.sublimetext.com/3)下载并安装

## 安装插件包管理器

> 安装完毕后可以在`Preferences - Package Settings`里管理已经安装的插件

### 自动安装
* 打开后按快捷键``Ctrl + ` ``，输入以下代码后回车
```python
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

### 手动安装
* 点击`Preferences > Browse Packages`菜单
* 进入打开的目录的上层目录，然后再进入`Installed Packages/`目录
* 下载[Package Control.sublime-package](https://sublime.wbond.net/Package%20Control.sublime-package)并复制到`Installed Packages/`目录
* 重启Sublime Text 3

# 配置C++环境

因为Sublime Text自带的设置是在Sublime内部运行的，并且不支持输入，如果运行出错会导致sublime卡死= =、

所以自己新建编译系统，设置为在终端中运行。

## Windows

### 安装MinGW
* 在[官网](http://www.mingw.org/)下载，在管理器中分别选择`gcc/g++/gdb`安装
* 将`gcc/g++`的安装目录添加到环境变量`PATH`，如`C:\MinGW\bin;`

### 创建编译系统
* 进入Sublime Text, 选择`Tools - Build System - New Build System`
* 删除默认内容，并输入以下json代码，保存为`MyCpp.sublime-build`
> `-Wall`和`-D MANGOGAO`以及`-std=c++0x`等命令都是可选的

```json
{
    "cmd": [
        "g++",
        "-Wall",
        "${file}",
        "-o",
        "${file_path}/${file_base_name}"
    ],
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
    "working_dir": "${file_path}",
    "selector": "source.c, source.c++",
    "encoding": "cp936",
    "variants": [
        {
            "name": "Build",
            "cmd": [
                "cmd",
                "/c",
                "g++",
                "-Wall",
                "-D MANGOGAO",
                "${file}",
                "-std=c++0x",
                "-o",
                "${file_path}/${file_base_name}"
            ]
        },
        {
            "name": "BuildAndRun",
            "cmd": [
                "cmd",
                "/c",
                "g++",
                "-Wall",
                "-D MANGOGAO",
                "${file}",
                "-std=c++0x",
                "-o",
                "${file_path}/${file_base_name}",
                "&&",
                "start",
                "cmd",
                "/c",
                "${file_path}/${file_base_name} & echo.&pause"
            ]
        }
    ]
}
```

### 设置快捷键
* 在Sublime中，选择`Preferences - Key Bindings Users`
* 在原有的`[`和`]`中加入以下json代码，注意json每一条是以_逗号_结尾的
* 里面的`f5`和`f9`可以自己设置，如改为`ctrl+b`和`ctrl+shift+b`等
* 可以发现，`args.variant`的值是和编译系统中`variant.name`相互对应的

```json
[
    {
        "keys": [
            "f5"
        ],
        "command": "build",
        "args": {
            "variant": "Build"
        }
    },
    {
        "keys": [
            "f9"
        ],
        "command": "build",
        "args": {
            "variant": "BuildAndRun"
        }
    }
]
```

## Mac OS X

这一部分在[How to include bits/stdc++.h on OS X](http://busyliving.me/System/Inlucde-bits-stdc++-h-on-OSX/)中已经详细说明了。

## 推荐插件

* 格式化代码`SublimeAStyleFormat`

# 配置Python环境

内置的Python设置同样是不支持输入的

## 安装Python

略= =、记得添加环境变量

## 安装插件
* 在Sublime中,按快捷键`Ctrl + Shift + P`(Mac是`cmd + shift + p`)
* 输入`install`，可以看见默认选中了`Package Control: Install Package`，回车选中
* 等待状态栏的`=`加载完毕后，继续输入`SublimeREPL`，回车安装
* 安装完毕后可以看到`Tools->SublimeREPL->Python`中的一些运行选项了

## 设置快捷键
* 和上面类似，添加以下json代码，其中的`f7`同样是可以根据需要修改的

```json
{
    "keys": [
        "f7"
    ],
    "command": "repl_open",
    "caption": "Python",
    "mnemonic": "p",
    "args": {
        "type": "subprocess",
        "encoding": "utf8",
        "cmd": [
            "python",
            "-i",
            "-u",
            "$file"
        ],
        "cwd": "$file_path",
        "syntax": "Packages/Python/Python.tmLanguage",
        "external_id": "python"
    }
}
```

* 保存后，按`f7`就能在新建的标签页中运行当前python文件，并且运行结束后支持继续输入

## 推荐插件

* 格式化代码`Python PEP8 Autoformat`

## 推荐设置

* 将Python的Tab缩进强制转换为空格

在`Preferences -> Settings – More -> Syntax Specific – User`中添加以下代码并保存
```json
{
    "tab_size": 4,
    "translate_tabs_to_spaces": true
}
```

# 配置Markdown环境

## Markdown Preview

这个插件安装后默认快捷键是`cmd + m`，自动转到浏览器中预览写好的markdown文档。
