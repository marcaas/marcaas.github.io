---
title: Docker部署python项目挂载flask练习记录
date: 2023-03-31 15:42:29
tags:
    - [python]
    - [flask]
    - [docker]
---

简介

<!-- more -->

# 项目结构

我的项目结构以及app.py内容如下

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230331155821.png)

命令行执行

```sh
python app.py
```

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230331160147.png)

浏览器输入 127.0.0.1:7090 可以打开此服务的网站

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230331160432.png)

ctrl + C 结束进程

# 依赖信息

## 生成项目依赖文件 requirements.txt

命令行执行

```sh
pip freeze > requirements.txt
```

生成requirement.txt文件，存放当前项目环境所有安装的依赖库

# Docker部署

## 新建Dockerfile文件

在项目根目录创建一个Dockerfile文件，内容如下

```docker
# Docker image for flask python run
# VERSION 1.0
# Author: Weiye
# 基础镜像使用python:3.7
FROM python:3.7
# 将服务器 requirements.txt 文件复制到 容器 /project/目录下
COPY requirements.txt /project/
# 指定容器工作目录为 /project/
WORKDIR /project/
# 安装 项目依赖
RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
# 运行
ENTRYPOINT ["python","app.py"]

# 编译
# docker build -t weiye/flask-project:v1 .

# run
#docker run -it -p 7090:7090 --name flask-project \
#-v /mydata/flaskProject:/project \1
#-d weiye/flask-project:v1

```

完成文件夹创建后，把整个项目放在服务器上的如下文件夹中

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230331163545.png)

## Docker编译镜像

进入项目根目录，看到有下面几个文件即可

![](https://raw.githubusercontent.com/marcaas/hexoPicgo/master/20230331164046.png)

执行

```sh
docker build -t marcaas/flask-project:v1
```
