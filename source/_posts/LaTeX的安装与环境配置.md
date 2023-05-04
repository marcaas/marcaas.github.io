---
title: LaTeX的安装与环境配置
date: 2023-04-10 08:05:58
tags:
- [LaTeX]
- [环境配置]
---

word用这打公式过于麻烦，于是把扔了好几年的LaTeX给捡回来。这里我选择在WSL（Windows Subsystem for Linux）中安装TeX Live。并使用vscode作为编辑器进行文档编辑。

<!-- more -->

# 简介

TeX Live 旨在成为启动和运行TeX 文档制作系统的直接方式。它提供了一个全面的 TeX 系统，其中包含适用于大多数 Unix 的二进制文件，包括 GNU/Linux 和macOS，以及 Windows。它包括所有主要的 TeX 相关程序、宏包和免费软件字体，包括对全球多种语言的支持。许多 Unix/GNU/Linux操作系统通过它们自己的发行版和包管理器提供 TeX Live。

# TeX Live下载安装

## 进入官网

[官网](https://tug.org/texlive/)

## 下载

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230410082153.png)

## 选择安装方式

这里选择ISO镜像安装

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230410084251.png)

## 选择镜像网站

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230410084405.png)

速度如果较慢也可以直接使用[清华tuna](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)的源

## 安装TEX Live

在正式安装前，用户需要在bash中执行

```sh
sudo apt install fontconfig gedit
```

以减少后续字体问题和文本编辑问题，在Windows11中，WSL已经能够自动完成可视化工作，这为一些用户提供了便利。为避免安装速度过慢，可按如下操作更改文件sources.list。

对原始文件进行备份
```sh
 sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

接下来执行
```sh
 sudo gedit /etc/apt/sources.list
```

按照[清华大学镜像网站](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)的内容对链接进行替换，保存退出，再执行

```sh
sudo apt update && sudo apt upgrade
```

换源并更新，若更改错误，可执行

```sh
 sudo cp /etc/apt/sources.list.bak /etc/apt/sources.list
```

接下来，在主系统中将镜像挂载，例如挂载到 X:\ ，而后进入bash并执行如下命令

```sh
sudo mkdir /mnt/x
sudo mount -t drvfs X: /mnt/x
```

执行

```sh
/mnt/x/install-tl
```

并选择路径为

```sh
~/texlive/2023
```

安装完毕后，修改环境变量为

```sh
# Add TeX Live to the PATH, MANPATH, INFOPATH
export PATH=~/texlive/2023/bin/x86_64-linux:$PATH
export MANPATH=~/texlive/2023/texmf-dist/doc/man:$MANPATH
export INFOPATH=~/texlive/2023/texmf-dist/doc/info:$INFOPATH
```

有关字体的处理变为

```sh
 cp ~/texlive/2023/texmf-var/fonts/conf/texlive-fontconfig.conf ~/.fonts.conf/09-texlive.conf
```

然后执行

```sh
fc-cache -fv
```

实际上, 无论将 TEX Live 安装到哪里, 字体的配置文件都可以仿照上面只复制于用户文件夹中.

## 卸载 TeX live
下面提供几种卸载方法，卸载后需注意清理环境变量

### 卸载源内的版本

如果要卸载从源内安装的 TeX Live，啸行推荐使用 synaptic package manager，在 Terminal 中执行

```sh
sudo apt install synaptic
```

即可安装，安装后打开，搜索 texlive 即可看到与之相关的包，右键标记以删除即可，当然用户也可以直接使用

```sh
dpkg -l | grep 'TeX␣Live'
```

找到相应的包名 \<package name>, 之后使用类似

```sh
sudo apt autoremove --purge <package name>
```

的命令删除各个包。通常

```sh
sudo apt autoremove --purge tex-common
```

就可以删除源内版本

### 手动卸载

如果是从光盘镜像安装, 直接删除文件夹即可. 先在 Terminal 中执行

```sh
kpsewhich -var-value TEXMFROOT
```

来查询安装路径, 进而通过 sudo rm -rf 命令进行删除. 默认安装的用户直接运行

```sh
sudo rm -rf /usr/local/texlive/2023
rm -rf ~/.texlive2023
```

卸载完成后，可以进一步移除之前设置的环境变量。在终端中执行

```sh
gedit ~/.bashrc
```

然后移除之前在文件末尾添加的

```sh
# Add TeX Live to the PATH, MANPATH, INFOPATH
export PATH=/usr/local/texlive/2023/bin/x86_64-linux:$PATH
export MANPATH=/usr/local/texlive/2023/texmf-dist/doc/man:$MANPATH
export INFOPATH=/usr/local/texlive/2023/texmf-dist/doc/info:$INFOPATH
```

并保存退出. 同时在 Terminal 中执行

```sh
sudo visudo
```

在 secure_path 中删除

```sh
/usr/local/texlive/2023/bin/x86_64-linux:
```

然后依次Ctrl + X , Y , Enter 保存退出.

### 使用 tlmgr 工具

同样, 用户也可以使用 tlmgr 工具卸载

```sh
sudo tlmgr remove --all
```
