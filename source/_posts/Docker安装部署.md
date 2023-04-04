---
title: Docker安装部署
date: 2023-04-04 10:00:33
tags:
    - [Docker]
---

这是简介

<!-- more -->

# 获得root权限

使用命令进入root模式

```sh
su -
```

授权文件写权限

```sh
chmmod -v u+w /etc/sudoers
```

打开文件编辑

```sh
vi /etc/sudoers
```

添加下面一行

```sh
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
${yourName}   ALL=(ALL)     ALL
```

移除文件写权限

```sh
chmod -v u-w /etc/sudoers
```

# 安装Docker

安装镜像

```sh
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

设置稳定仓库

```sh
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2

```sh
sudo yum install -y yum-utils
sudo yum install -y device-mapper-persistent-data
sudo yum install -y lvm2
```

# 将用户加入docker组

```sh
# 将普通用户加入到docker组
sudo gpasswd -a $USER docker
# 更新docker组
newgrp docker
```

# 命令

```sh
docker system df
	docker自身的内存占用
docker system prune
	docker system prune命令可以用于清理磁盘，删除关闭的容器、无用的数据卷和网络，以及dangling镜像(即无tag的镜像)
docker image
	查看docker镜像内容
docker info
	查看docker信息
docker stats
	查看容器运行内存cpu占用情况
docker update --restart=always 容器名称
	设置docker容器开机启动
less /var/lib/docker/containers/容器ID/容器ID-json.log
	docker 容器日志路径日志内容查询
docker logs --tail=10 -f 容器名称/容器id
	docker 容器日志内容实时查看
```

# docker构建容器时推荐追加的脚本

	-it -d   # 支持后台运行
	-e TZ=Asia/Shanghai  	# 指定时区
	-v /etc/localtime:/etc/localtime:ro  # 公用服务器时间
	--restart=always 	# 自动重启
