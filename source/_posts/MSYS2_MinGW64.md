---
title: 关于windows系统使用MSYS2构建C/C++环境并配置VSCode的记录
date: 2022-03-21 20:51:02
tags: 
    - [C/C++]
    - [环境配置]
    - [MSYS2]
    - [MinGW64]
categorige:
    - [C/C++,环境配置]
---

![ ](https://w.wallhaven.cc/full/j3/wallhaven-j3o9dm.png)

由于前一段时间感觉电脑的空间过于拥挤于是重装了windows的系统使得原来的代码环境都需要重新构建，而相比于Python的简单粗暴，C/C++环境就显得有一些复杂，再加上以前都是直接从网上随便找几篇教程来效仿，一直都没有较为系统地安装过一次。在新系统安装VSCode的时候在[官网文档](https://code.visualstudio.com/docs/languages/cpp)中发现官方发布了教程且使用了MinGW64举例，这正符合我的需求，于是便有了这篇记录。

<!--more-->

## VSCode安装

首先我们需要安装[VSCode](https://code.visualstudio.com/)，并安装名为C/C++的扩展

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/cpp-extension.png)


## MinGW64安装
由于C/C++语言需要编译器编译后才可被机器运行，而windows和VSCode都不内置编译器（而Linux和macOS系统是内置编译器的），所以我们这里要安装编译器。

我们可以先测试一下我们的系统中是否装有编译器，使用如下代码

``` bash
$ g++ --version
```
或
```
$ clang --version
```
如果没有出现版本信息则说明我们需要安装编译器。
### 安装MSYS2
我们通过[MSYS2](https://www.msys2.org/)来安装MinGW64，MSYS2对最新的GCC、Mingw-w64等等常用的C语言工具提供支持。

1. 首先下载[MSYS2的安装包](https://github.com/msys2/msys2-installer/releases/download/2022-03-19/msys2-x86_64-20220319.exe)
2. 运行安装包（需要注意的是MSYS2要求系统是64位且是win7或更新的版本）
3. 设置安装目录

![ ](https://www.msys2.org/images/install-2-path.png)

4. 安装完成，我们直接运行它

![ ](https://www.msys2.org/images/install-3-finish.png)

5. 我们进入了MSYS2的界面，接下来需要更新程序包的数据库并更新程序包，使用如下代码

``` MSYS2
$ pacman -Syu
```

程序包数据库会开始更新

```MSYS2
$ pacman -Syu
:: Synchronizing package databases...
 mingw32                        805.0 KiB
 mingw32.sig                    438.0   B
 mingw64                        807.9 KiB
 mingw64.sig                    438.0   B
 msys                           289.3 KiB
 msys.sig                       438.0   B
:: Starting core system upgrade...
warning: terminate other MSYS2 programs before proceeding
resolving dependencies...
looking for conflicting packages...

Packages (6) bash-5.1.004-1  filesystem-2021.01-1
             mintty-1~3.4.4-1  msys2-runtime-3.1.7-4
             pacman-5.2.2-9  pacman-mirrors-20201208-1

Total Download Size:   11.05 MiB
Total Installed Size:  53.92 MiB
Net Upgrade Size:      -1.24 MiB

:: Proceed with installation? [Y/n]
:: Retrieving packages...
 bash-5.1.004-1-x86_64            2.3 MiB
 filesystem-2021.01-1-any        33.2 KiB
 mintty-1~3.4.4-1-x86_64        767.2 KiB
 msys2-runtime-3.1.7-4-x86_64     2.6 MiB
 pacman-mirrors-20201208-1-any    3.8 KiB
 pacman-5.2.2-9-x86_64            5.4 MiB
(6/6) checking keys in keyring       100%
(6/6) checking package integrity     100%
(6/6) loading package files          100%
(6/6) checking for file conflicts    100%
(6/6) checking available disk space  100%
:: Processing package changes...
(1/6) upgrading bash                 100%
(2/6) upgrading filesystem           100%
(3/6) upgrading mintty               100%
(4/6) upgrading msys2-runtime        100%
(5/6) upgrading pacman-mirrors       100%
(6/6) upgrading pacman               100%
:: To complete this update all MSYS2 processes including this terminal will be closed. Confirm to proceed [Y/n]
```

按任意键后退出MSYS2界面

6. 我们从开始菜单中找到MSYS2 MSYS程序并运行，使用如下代码更新基础程序包

```MSYS2
$ pacman -Su
```

数据包开始更新

```MSYS2
$ pacman -Su
:: Starting core system upgrade...
 there is nothing to do
:: Starting full system upgrade...
resolving dependencies...
looking for conflicting packages...

Packages (20) base-2020.12-1  bsdtar-3.5.0-1
              [... more packages listed ...]

Total Download Size:   12.82 MiB
Total Installed Size:  44.25 MiB
Net Upgrade Size:       3.01 MiB

:: Proceed with installation? [Y/n]
[... downloading and installation continues ...]
```

7. 现在MSYS2已经准备好了，接下来我们需要安装mingw-w64、GCC等一系列工具

```MSYS2
$ pacman -S --needed base-devel mingw-w64-x86_64-toolchain
```

这个命令会安装一些我们需要用到的工具

```MSYS2
$ pacman -S --needed base-devel mingw-w64-x86_64-toolchain
warning: file-5.39-2 is up to date -- skipping
[... more warnings ...]
:: There are 48 members in group base-devel:
:: Repository msys
   1) asciidoc  2) autoconf  3) autoconf2.13  4) autogen
   [... more packages listed ...]

Enter a selection (default=all):
:: There are 19 members in group mingw-w64-x86_64-toolchain:
:: Repository mingw64
   1) mingw-w64-x86_64-binutils  2) mingw-w64-x86_64-crt-git
   [... more packages listed ...]

Enter a selection (default=all):
resolving dependencies...
looking for conflicting packages...

Packages (123) docbook-xml-4.5-2  docbook-xsl-1.79.2-1
               [... more packages listed ...]
               m4-1.4.18-2  make-4.3-1  man-db-2.9.3-1
               mingw-w64-x86_64-binutils-2.35.1-3
               mingw-w64-x86_64-crt-git-9.0.0.6090.ad98746a-1
               mingw-w64-x86_64-gcc-10.2.0-6
               mingw-w64-x86_64-gcc-ada-10.2.0-6
               mingw-w64-x86_64-gcc-fortran-10.2.0-6
               mingw-w64-x86_64-gcc-libgfortran-10.2.0-6
               mingw-w64-x86_64-gcc-libs-10.2.0-6
               mingw-w64-x86_64-gcc-objc-10.2.0-6
               mingw-w64-x86_64-gdb-10.1-2
               mingw-w64-x86_64-gdb-multiarch-10.1-2
              [... more packages listed ...]

Total Download Size:    196.15 MiB
Total Installed Size:  1254.96 MiB

:: Proceed with installation? [Y/n]
[... downloading and installation continues ...]
```

现在我们的MinGW已经准备好了

### 配置环境变量

在搜索框内搜索环境变量，选择编辑系统环境变量

![ ](https://raw.githubusercontent.com/marcaas/picgo/main/2.png)

选择环境变量并双击path项

![ ](https://raw.githubusercontent.com/marcaas/picgo/main/3.png)

新建环境变量，地址为 ...(MYSYS2安装路径)...\msys64\mingw64\bin，保存并返回。到这一步，我们的MinGW就基本安装完成了。

打开任意终端分别输入

``` bash
gcc --version
```

``` bash
g++ --version
```

``` bash
gdb --version
```

若均有版本号输出则证明安装成功。

## Hello World

为了检验编译器已经安装完成并且能够正确使用，我们用一个最简单的程序"HelloWorld"来检验。

### 创建helloworld.cpp文件

新建一个文件夹用VSCode打开它，此时可能会弹出是否信任该文件夹的窗口，我们直接选择信任（因为该文件夹是你自己创建的），新建一个helloworld.cpp文件。

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/new-file.png)

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/hello-world-cpp.png)

添加helloworld的代码

``` c++
#include <iostream>

int main()
{
    std::cout << "Hello World" << std::endl;
}
```

使用ctrl+S保存代码。

### 生成helloworld.exe可执行文件

选择上方栏中的 **终端** > **运行生成任务**（快捷键为ctrl+shift+B）。

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/run-build-task.png)

此时会弹出下拉菜单让我们选择要使用的编译器，这里我们选择g++。

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/gpp-build-task-msys64.png)

这时观察左边的工作区就会发现多了一个helloworld.exe文件，则可执行文件生成成功。

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/hello-world-exe.png)

### 运行Hello World

在终端中输入

``` bash
.\helloworld
```
如果所有步骤都准确无误的话你会在终端中看到类似下面的反馈

![ ](https://code.visualstudio.com/assets/docs/languages/cpp/run-hello-world.png)

到这里我们已经简单地把C/C++环境部署在了计算机中并学会了使用VSCode调用编译器生成可执行文件。

## Debug helloworld.cpp

接下来我们找到上边栏的 **运行** > **启动调试**（快捷键F5），并在弹出的下拉窗口中选择g++

![ ](https://code.visualstudio.com/assets/docs/cpp/mingw/build-and-debug-active-file.png)

VSCode会创建一个 **launch.json** 文件

``` json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "g++.exe - Build and debug active file",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": false,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "C:\\msys64\\mingw64\\bin\\gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "C/C++: g++.exe build active file"
    }
  ]
}
```

该文件是关于g++调试的配置文件，到此我们的编译和调试功能均可用，后续更多细节可以参考[官方文档](https://code.visualstudio.com/docs/cpp/config-mingw)在此不多赘述。