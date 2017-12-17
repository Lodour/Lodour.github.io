title: 在Mac OS上使用OpenMP
categories: "Mac OS"
tags:
  - "Mac OS"
  - "OpenMP"
date: 2017-04-11 10:29:21
---
记录在Mac上测试使用OpenMP的过程。

<!--more-->

# 系统环境
|项目|版本|
|:-:|:-:|
|Mac OS|EI Captian 10.11.6|
|Xcode|Version 8.2.1 (8C1002)|

# LLVM
在终端中输入命令`gcc -v`查看版本信息，可以看到以下输出。

```bash
Apple LLVM version 8.0.0 (clang-800.0.42.1)
Target: x86_64-apple-darwin15.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

因此实际上Mac中使用的并非MinGW，于是接下来安装clang版本的OpenMP。

# clang-omp
首先找到了[clang-omp官网](http://clang-omp.github.io/)。

实际上根据官网最上面的提示：

> Development activity of OpenMP support in clang/llvm compiler has moved to www.llvm.org. Please get OpenMP-enabled clang (OpenMP 3.1 is fully supported in clang/llvm 3.7) and contribute to its further development there. This web-site is maintained for archival purposes only.

可以发现clang-omp项目已经被包含到了`clang/llvm 3.7`以及更高的版本，而上面查看的版本信息`clang-800.0.42.1`，事实证明是不包含的。

事实上，如果尝试使用`brew install clang-omp`直接安装，会报错找不到`clang-omp`。

# openmp.llvm
找到[OpenMP of LLVM 官网](http://openmp.llvm.org/)尝试安装openmp。

```bash
$ svn co http://llvm.org/svn/llvm-project/openmp/trunk openmp
$ cd openmp/runtime
$ mkdir build && cd build
$ cmake .. -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++
$ make && make install
```

安装成功后，会显示如下信息：

```bash
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/lib/libomp.dylib
-- Installing: /usr/local/include/omp.h
```

这个时候实际上还是存在问题的，如使用以下代码进行测试。

```cpp
#include <cstdio>
#include <omp.h>
using namespace std;

int main(int argc, const char * argv[]) {
    int nthreads, tid;
    omp_set_num_threads(8);
#pragma omp parallel private(nthreads, tid)
    {
        tid = omp_get_thread_num();
        printf("Hello World from OMP thread %d\n", tid);
        if (tid == 0) {
            nthreads = omp_get_num_threads();
            printf("Number of threads is %d\n", nthreads);
        }
    }
    return 0;
}
```

使用`g++ test.cpp -fopenmp -o test`可能会出现以下几种报错：

* `ld: symbol(s) not found for architecture x86_64`

* `clang: error: unsupported option '-fopenmp'`

这是因为我们目前使用的仍然是LLVM。

# LLVM Again
使用homebrew安装新版的LLVM：`brew install llvm`

为了与系统自带的LLVM区分，添加如下符号链接：

```bash
sudo ln -s /usr/local/opt/llvm/bin/clang /usr/local/bin/clang-omp
sudo ln -s /usr/local/opt/llvm/bin/clang++ /usr/local/bin/clang-omp++
```

# 测试
## 手动编译
`clang-omp++ test.cpp -fopenmp -fplugin=/usr/local/lib/libomp.dylib -o test`

## Xcode 8
参考[Stack Overflow](http://stackoverflow.com/questions/33668323/clang-omp-in-xcode-under-el-capitan)的回答。

* 项目`Build Settings`
	* 新建项目`CC: /usr/local/bin/clang-omp`
	* `Other C Flags`中添加`-fopenmp`
	* `Header Search Paths`中添加`/usr/local/include`
	* `Library Search Paths`中添加`/usr/local/lib`
	* `Enable Modules (C and Objective-C)`设置为`No`
* 项目`Build Phases`
	* `Link Binary With Libraries`中添加`/usr/local/lib/libiomp5.dylib`
* 为Xcode添加符号链接
	* `sudo ln -s /usr/local/bin/clang-omp++ /usr/local/bin/clang++-omp`
* 包含头文件
	* 根据回答中所说是`#include <libiomp/omp.h>`
	* 本文所遇到的情况应该是`#include <omp.h>`

