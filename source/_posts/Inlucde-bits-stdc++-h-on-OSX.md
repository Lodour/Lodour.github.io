title: How to include bits/stdc++.h on OS X
date: 2015-12-15 23:47:44
categories: 技术
tags: 
	- C++
	- "Mac OS"
---

Mac OS X里使用的gcc和g++实际上是clang和clang++，clang虽然支持部分的c++11扩展，但是`bits/stdc++.h`这一类头文件是不支持的，如果需要用到这类头文件，就需要手动安装gcc并用gcc进行编译。

<!--more-->

# About clang
* 在Terminal中查看gcc(clang)的版本信息,`gcc -v`

> Apple LLVM version 7.0.0 (clang-700.1.76)
> Target: x86_64-apple-darwin14.5.0
> Thread model: posix

可以看到gcc实际上是clang的，而非真正的gcc。clang加上一定的参数可以支持c++11扩展，但是不能包含c++11的头文件，因此下一步需要安装真正的gcc编译器。

# Download && Install
* 前往[High Performance Computing for Mac OS X](http://hpc.sourceforge.net/)下载对应osx版本的gcc，这里我选择的是[gcc-4.9-bin.tar.gz](http://prdownloads.sourceforge.net/hpc/gcc-4.9-bin.tar.gz?download).

* 在Terminal中进入下载文件存放的目录，并进行如下操作后，gcc会自动安装到`/usr/local`中
``` sh
User$ cd ~/Downloads/
User$ gunzip gcc-4.9-bin.tar.gz
User$ sudo tar -xvf gcc-4.9-bin.tar -C /
User$ (Input Password)
```

# Compatible with clang
默认情况下，`/usr/bin`目录下是有多个到clang和clang++的替身（快捷方式）的，同样也可以直接使用clang和clang++命令代替原来的gcc和g++。现在需要建立真正gcc和g++的软连接：
``` sh
# Check whether new gcc and g++ exist
User$ cd /usr/local/bin
User$ ls -al | grep 'gcc\|g++'
# If we have gcc and g++, use root
User$ sudo su -
User$ Input Password...
# Delete previous gcc and g++
root$ cd /usr/bin
root$ rm gcc
root$ rm g++
# Add soft-link to new gcc and g++
root$ ln -s /usr/local/bin/gcc gcc
root$ ln -s /usr/local/bin/g++ g++
# Done.
root$ logout
```

现在的gcc和g++就是真正的gcc和g++了，如果需要使用原来的clang编译器，使用命令clang和clang++即可。

# Configure Sublime Text 3
* 现在来配置sublime的c++和c++11编译选项。
* New Build System -> MyCpp.sublime-build
``` json
{
    "cmd": [
        "clang++",
        "${file}",
        "-o",
        "${file_path}/${file_base_name}"
    ],
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
    "working_dir": "${file_path}",
    "selector": "source.c, source.c++",
    "variants": [
        {
            "name": "Build",
            "cmd": [
                "bash",
                "-c",
                "clang++ '${file}' -o '${file_path}/${file_base_name}.out' -D 'MANGOGAO' -W"
            ]
        },
        {
            "name": "BuildAndRun",
            "cmd": [
                "bash",
                "-c",
                "clang++ '${file}' -o '${file_path}/${file_base_name}.out' -D 'MANGOGAO' -W && open '${file_path}/${file_base_name}.out'"
            ]
        }
    ]
}
```

* New Build System -> MyCpp0x.sublime-build
``` json
{
    "cmd": [
        "g++",
        "${file}",
        "-o",
        "${file_path}/${file_base_name}"
    ],
    "file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:? (.*)$",
    "working_dir": "${file_path}",
    "selector": "source.c, source.c++",
    "variants": [
        {
            "name": "Build",
            "cmd": [
                "bash",
                "-c",
                "g++ '${file}' -o '${file_path}/${file_base_name}.out' -D 'MANGOGAO' -std=c++0x -W"
            ]
        },
        {
            "name": "BuildAndRun",
            "cmd": [
                "bash",
                "-c",
                "g++ '${file}' -o '${file_path}/${file_base_name}.out' -D 'MANGOGAO' -std=c++0x -W && open '${file_path}/${file_base_name}.out'"
            ]
        }
    ]
}
```

*注:配置中的`-D 'MANGOGAO'`和`-W`是编译选项，可以自行选择。*

* 顺便更新了一下快捷键设置
``` json
[
    {
        "keys": [
            "super+b"
        ],
        "command": "build",
        "args": {
            "variant": "Build"
        }
    },
    {
        "keys": [
            "super+shift+b"
        ],
        "command": "build",
        "args": {
            "variant": "BuildAndRun"
        }
    }
]
```
