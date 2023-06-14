---
title: Maven的配置与使用
date: 2023-03-08 15:31:01
tags:
    - [Maven]
    - [Springboot]
---

之前在创建Springboot项目时出现了pom爆红的情况，maven也不下载依赖，怀疑可能是maven版本过高导致，故重新对maven进行降级和配置。

<!-- more -->

* Maven 3.5.3

# 一、Maven下载
[官网](https://maven.apache.org/)下载3.5.3版本
# 二、安装和配置
* 解压到常用的存放环境的位置
* 配置maven3.5.3的setting.xml文件（1、本地仓库；2、阿里云镜像加速）
* 配置maven3.5.3的环境变量
* 在idea中设置maven
## 1. 解压
## 2. 配置maven的setting.xml文件
打开maven根目录下的conf文件夹中的setting.xml文件，找到localRepository词条，对其进行编辑，将自己的本地仓库地址填入对应位置

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308154558.png)

然后配置远程仓库，找到mirrors词条，对其进行修改，这里使用阿里云仓库

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308155205.png)

## 3. 配置环境变量
在系统环境变量中新建MAVEN_HOME，路径为安装路径

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308155523.png)

打开终端，输入

```
mvn -version
```

检查是否配置成功（如图所示配置完成）

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308155719.png)

## 4. 在idea中设置maven3.5.3
打开设置

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308160021.png)

选择为新项目设置

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308160058.png)

对此三处进行配置

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308160318.png)

配置完成后新建一个maven项目

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230308161333.png)

均无报错，说明配置完成。

maven项目导入依赖成功后我又创建了一个Springboot项目进行测试，结果依然出现了依赖无法导入的问题，在网上查找了相关问题的帖子后，认为可能是Springboot更新版本后远程仓库的jar包的名称有变化，导致maven搜索不到老版本Springboot的某个依赖包。而创建Springboot项目时，idea会默认选择2.6版本的Springboot，如下图所示

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230309092744.png)

于是我将其修改为2.7.6后重新建立依赖，没有报错，至此。