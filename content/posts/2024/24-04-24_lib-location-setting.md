---
title: 程序编译链接和运行时的函数库路径设置
subtitle: 
date: 2024-04-24T14:20:58+08:00
lastmod: 2024-04-24T14:20:58+08:00
draft: false
author: Xinyu Yang
authorLink: 
keywords:
  - C
  - LD_LIBRARY_PATH
  - LIBRARY_PATH
description: 
images: 
tags:
  - c
categories:
  - record
hiddenFromHomePage: false
hiddenFromSearch: false
featuredImage: 
featuredImagePreview: ""
toc:
  enable: true
math:
  enable: false
comment:
  enable: true
lightgallery: false
license: ""
---
程序在编译链接时的函数库 (lib) 与运行时不一定相同，并且可以通过多种不同的方法进行设置，这里对常用的设置方法进行总结。
<!--more-->

程序在编译的时候会查找头文件和函数库，其查找路径可通过 `gcc -print-search-dirs` 进行查看，或者在编译或者预处理的时候添加 `-v` 选项。就会打印链接的时候所搜索的路径。

```bash
$ gcc -print-search-dirs
install: /usr/lib/gcc/x86_64-linux-gnu/11/
programs: =/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/bin/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/bin/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/bin/
libraries: =/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/lib/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/lib/x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../lib/:/lib/x86_64-linux-gnu/11/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/11/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../x86_64-linux-gnu/lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../:/lib/:/usr/lib/

$ gcc -v -o hello hello.c
...
LIBRARY_PATH=/usr/lib/gcc/x86_64-linux-gnu/11/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../x86_64-linux-gnu/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../../lib/:/lib/x86_64-linux-gnu/:/lib/../lib/:/usr/lib/x86_64-linux-gnu/:/usr/lib/../lib/:/usr/lib/gcc/x86_64-linux-gnu/11/../../../:/lib/:/usr/lib/
...
```

在编译链接的时候，如果想要添加其他的路径，可以通过两种方式完成：

1. 程序在链接的时候通过调用 `ld` 程序进行程序与函数库的链接，其搜索路径固定，无法通过文件进行配置。不过可以通过 `gcc` 的 `-L` 选项来添加函数库的路径，如 `-L /usr/local/lib`。
2. 或者是环境变量 `LIBRARY_PATH` 进行添加，如 `export LIBRARY_PATH=LIBRARY_PATH:/usr/local/lib/` 。

使用上述方法配置后，可以在编译程序的时候使用 `-v` 进行确认，看是否设置成功了。

默认情况下，使用 `gcc` 进行编译的时候，程序是动态链接的。这意味着在程序编译的时候，只会保存一些基本的信息，如链接信息、符号表等。程序执行的时候，才会去动态查找函数库的二进制代码。因此，即使程序顺利通过编译，有时候也会在执行的时候会报 “无法找到 lib” 之类的错误。这是因为程序编译和执行的时候所查找的函数库路径不同。如果在编译的时候指定了有效的路径，但是程序执行的时候没有指定，那么就可能出现这种情况。

程序在执行的时候会去 `/etc/ld.so.cache` 文件里查找函数库的路径。该文件由 `ldconfig` 生成，通过 `ldconfig -v` 可以查看目前所查找到的函数库列表。

```bash
$ ldconfig -v
...
/lib/x86_64-linux-gnu: (from /etc/ld.so.conf.d/x86_64-linux-gnu.conf:3)
        libpcsclite.so.1 -> libpcsclite.so.1.0.0
        libBrokenLocale.so.1 -> libBrokenLocale.so.1
        libnss3.so -> libnss3.so
        libunwind-ptrace.so.0 -> libunwind-ptrace.so.0.0.0
        libnuma.so.1 -> libnuma.so.1.0.0
...
```

`ldconfig`从其搜索路径中查找所有的函数库，搜索路径可以通过 `/etc/ld.so.conf` 进行配置，也可以通过 `gcc` 选项`-Wl,-rpath=`进行指定，但是其只影响运行时动态库的路径搜索，与链接无关。这意味着：即使使用该参数也并不能省略 `-L` 指定链接路径。

除此之外，还可以使用 `LD_LIBRARY_PATH` 来指定动态链接库的位置。其用法与 `LIBRARY_PATH` 类似。