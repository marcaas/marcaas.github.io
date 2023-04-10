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
