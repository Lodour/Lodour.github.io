title: 操作系统(2)第三次实验课
categories: 操作系统
tags:
  - 操作系统实验
  - 系统功能调用
date: 2016-12-13 14:47:29
---

内存映射系统功能调用。

<!--more-->

代码保存在`exp3.c`中。

```cpp
/*
   系统功能调用mmap
   void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
   1. addr  : 虚拟空间地址，NULL表示由系统自动分配
   2. length: 映射空间的长度
   3. prot  : 内存权限rwx
   4. flags : 共享或私有
   5. fd    : 文件标识符
   6. offset: 文件偏移量
*/

#include <stdio.h>
#include <stdlib.h>
#include <sys/mman.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
	// 打开文件
	int fd = open("./exp3.c", O_RDWR);
	// 获取文件长度
	off_t length = lseek(fd, 0, SEEK_END);
	// 文件映射到内存
	char *p = mmap(NULL, length, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
	// 输出内存中的数据
	printf("%s\n", p);
	// 取消映射
	munmap(p, length);
	// 关闭文件
	close(fd);
	return 0;
}
```

实际上就是代码自己打印自己。

```bash
$ gcc exp3.c -o exp3
$ ./exp3
```
