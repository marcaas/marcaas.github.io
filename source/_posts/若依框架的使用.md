---
title: 若依框架的使用
date: 2023-04-17 08:51:23
tags:
    - [SpringBoot]
    - [Vue]
    - [若依框架]
---

简介：

这是简介

<!-- more -->

# 若依框架克隆到本地

```sh
git clone git@gitee.com:y_project/RuoYi-Vue.git
```

目录结构如下

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417085438.png)

# 后端配置

## 数据库导入

使用Navicat导入sql文件夹中的两个文件得到相关的表结构

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417090225.png)

## 更改数据库配置

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417092224.png)

将ry-vue更改为本地数据库名称，用户名和密码也换成本地数据库的对应用户名和密码（加引号）。

## 更改后端程序运行端口（如需要）

默认为8080，如需要可以自行更改

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417092619.png)

## 配置redis

端口默认6379

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417103238.png)

下载[redis安装包](https://github.com/tporadowski/redis/releases)，并安装。

安装后双击redis-server.exe启动服务端，双击redis-cli.exe启动客户端连接服务端，在客户端输入 “ping”，出现“PONG”，即证明连接成功

## 启动后端程序

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417103725.png)

观察到启动成功

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417103817.png)

# 前端配置

## 安装依赖

使用vscode打开项目中的ruoyi-ui文件夹，终端运行

```sh
npm install
```

## 运行前端程序

安装完依赖后，继续运行（此时要求后端程序处于运行状态，不然前端会找不到后端的端口）

```sh
npm run dev
```

浏览器自动弹出登陆界面

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417105103.png)

此处执行了启动服务的命令，如需要使用其他命令，可以查看 package.json 文件

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230417105755.png)

# 模块自定义

## 定时任务



