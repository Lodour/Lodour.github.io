title: 操作系统(2)第二次实验课
categories: 操作系统
tags:
  - 操作系统实验
  - 库函数调用
date: 2016-12-06 16:27:44
---

C语言文件操作相关库函数调用。

<!--more-->

讲解了5个常用的文件相关的库函数调用，具体见代码注释。

实际上，lfy写的代码里有些问题：

1. free后的空间不应该继续被使用

2. open一个不存在的文件，默认权限为0，需要通过参数设置，或`chmod 70`

以下是代码`exp2.c`0w0.

```cpp
/*
文件操作库函数
1. open()  打开文件       
2. read()  从文件中读数据 
3. write() 向文件中写数据 
4. close() 关闭文件       
5. lseek() 定位文件指针   
6. ioctl() 文件控制       
*/
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
main() {
	int fd;             // 文件标识符
	off_t len;          // 文件长度
	char *data_pointer; // 某数据指针	
	// 打开文件 [只读]
	fd = open("./exp2.c", O_RDONLY);
	// 获取文件长度
	len = lseek(fd, 0, SEEK_END);
	// 文件指针指向开头
	lseek(fd, 0, SEEK_SET);
	// 申请同样大小的内存
	data_pointer = (char*)malloc(len);
	// 读取文件内容，然后关闭
	read(fd, data_pointer, len);
	close(fd);
	// 打开另一个文件，[读写][不存在则创建]，[文件所有者rwx]
	fd = open("./data.out", O_RDWR | O_CREAT, S_IRWXU);
	// 写入数据
	write(fd, data_pointer, len);
	// 释放内存
	free(data_pointer);
	// 关闭文件
	close(fd);
}
```

代码实现的效果是，将`exp2.c`中的内容，复制到同目录下`data.out`文件中。

```bash
$ gcc exp2.c -o exp2
$ ./exp2
$ cat data.out
$ diff data.out exp2.c
```