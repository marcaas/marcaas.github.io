---
title: 记录一个将java程序封装成windows服务的小错误
date: 2023-03-24 09:40:19
tags:
    - [java]
    - [封装]
    - [windows服务]
---
![](https://w.wallhaven.cc/full/zy/wallhaven-zyxvqy.jpg)
## 概述
记录在将java程序封装成windows服务时的一个小细节，由于名称问题使得程序从运行java.exe变成了不断递归运行自己的问题。

<!-- more -->

# 一、错误操作复现

## 操作流程
* 编译源程序
* 解压编译好的文件
* 修改配置文件
* 编写服务脚本
* 查看运行结果（日志）

## 1. 编译源程序
这里我收到的是已经编译好的程序，在java项目根目录下的target文件夹内可以找到一个打包好的nanridata-bin.tar.gz文件。

如果需要自己编译，可以使用maven命令编译源码并打包

```sh
# 编译
mvn compile

# 打包
mvn package
```

## 2. 解压编译好的文件

解压tar.gz文件到一个合适的位置，这里tar.gz实际上就是对编译好的文件进行了两次压缩打包，所以需要解压两次，可以使用[7zip](https://7-zip.org/)进行解压。

解压后的文件结构如下图所示：

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324102717.png)

其中bin中为启动脚本和服务脚本，conf为配置文件，lib中为程序库。

## 3. 修改配置文件

这里我主要修改RabbitMQ字段和机组测点信息。

## 4. 编写服务脚本

原linux脚本命令如下：

```sh
cd "${PROJ_BIN}" &&  nohup java -cp "${PROJ_BIN}:${PROJ_HOME}/conf:${PROJ_HOME}/lib/*" ${main_class} &
```

这里我将脚本放置在bin文件夹下，并根据linux命令修改为windows命令，内容如下：

```sh
java -cp "..\bin;..\conf;..\lib\*" cn.hecos.iec.IEC104Start 
```

注意此处我的脚本被我命名成了java.cmd。

我尝试运行了一下它，出现了下图的情况：

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/%E5%8A%A8%E7%94%BB.gif)

于是寄了。

# 二、错误原因

在别人的指导下，我才意识到犯了个多离谱的错误，这个脚本的名字叫什么都行，唯独不能叫java，因为脚本内容是调用java命令执行后续操作，这使得系统不知道该调用环境变量中的java.exe还是我写的java.cmd脚本，于是选择了其中我的那个，开始递归递归调用。

# 三、修改后的程序运行

* 修改文件名
* 部署服务
* 运行并查看日志

## 1. 修改文件名

将文件名修改为admin.cmd

## 2. 部署服务

使用[nssm](http://www.nssm.cc/)对改名后的脚本文件admin.cmd进行部署。

打开终端，cd到nssm的安装位置

nssm的使用方法可以双击nssm.exe看到

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324111501.png)

```sh
cd C:\${yourPath}\${nssmversion}\${systemVersion}
```

运行nssm的部署程序

```sh
nssm install
```

填入相应信息

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324111929.png)

点击Install service服务即被部署好了，可以打开服务界面查看

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324112235.png)

运行服务并查看日志

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324112611.png)

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230324112740.png)

二者时间对应，至此。
