---
title: "Docker Getting Started"
date: 2023-03-21T13:19:38+08:00
---

```sh

# 运行以下命令在 Docker 容器中克隆 GitHub 仓库：
# 该命令的含义是，以 alpine/git 镜像为基础，
# 启动一个名为 repo 的容器，并在容器内执行 git clone 命令，
# 将 https://github.com/docker/getting-started.git 仓库克隆到当前目录下的 . 文件夹中。
docker run --name repo alpine/git clone https://github.com/docker/getting-started.gited/ .

# 然后，运行以下命令将克隆的代码复制到本地文件系统中：
# 该命令的含义是，将名为 repo 的容器中 /git/getting-started/ 
# 文件夹下的所有文件和子文件夹复制到当前目录下。
docker cp repo:/git/getting-started/ .


docker run -d -p 80:80 docker/getting-started
# You'll notice a few flags being used. Here's some more info on them:

# -d - run the container in detached mode (in the background)
# -p 80:80 - map port 80 of the host to port 80 in the container
# docker/getting-started - the image to use
```

## Dockerfile

* [Dockerfile for Dev](https://zhuanlan.zhihu.com/p/440325928)

```dockerfile
FROM ubuntu
LABEL org.opencontainers.image.authers="Frank Chu"

# env variable
ENV DEBIAN_FRONTEND noninteractive

# Time zone
ARG TZ=Asia/Shanghai
ENV TZ ${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 具体来说，/usr/share/zoneinfo 目录下保存了各个时区的信息，而/etc/localtime 和 /etc/timezone 文件则记录了系统当前的时区信息。上述指令中的含义是，将容器时区设置为 $TZ 变量指定的时区。

# 具体操作为：

# /usr/share/zoneinfo/$TZ 表示找到 $TZ 对应的时区文件（例如 Asia/Shanghai）；
# ln -snf /usr/share/zoneinfo/$TZ /etc/localtime 表示将 $TZ 对应的时区文件软连接到 /etc/localtime，从而将容器时区设置为指定的时区；
# echo $TZ > /etc/timezone 表示将 $TZ 值写入 /etc/timezone 文件，从而使系统中其他程序也能读取到容器的时区信息。
# 这个指令通常会在 Dockerfile 中的一开始设置，以确保后续操作中时间信息的正确性。
```

## Ubuntu

* [Unbuntu Started](https://yeasy.gitbook.io/docker_practice/container/run)
* [Ubuntu Proxy](https://docs.docker.com.zh.xy2401.com/network/proxy/)

```sh
docker pull ubuntu

# new container
# docker run [--name <string>] <image> <command> <args>
# docker run -itd [--name ubuntu-test] ubuntu
docker run -t -i ubuntu-testing ubuntu:18.04 /bin/bash

# start existing container
# docker start [-i --interactive] <container>
docker start ubuntu-testing

# execute command in running container
# docker exec <container> <command>
docker exec -it ubuntu-testing bash
```