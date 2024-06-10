---
title: docker
date: 2024-06-08 14:22:21
tags:
---
### docker

> 配环境问题已经困扰了我不是一天两天，多懂一点docker可以少很多麻烦

[dockerhub](https://hub.docker.com/)

#### 概念

- container
- image
- volume
- network

#### 指令

```bash
docker pull
docker images
docker run
docker start
docker stop
docker rm
docker rmi
...
```

在.zshrc中做一个影射

```bash
alias docker-ps="docker ps --format 'table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}'"
alias docker-ps-a="docker ps --format 'table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}' -a"
```

关于docker run的一些详细参数

```
-d: 在后台运行
-p: 指定映射端口 暴露端口:容器内端口
--name: 指定contain名字
-v: 进行挂载，~/myvolume:usr/path or volume:/usr/path
-e: 添加环境变量
-it 容器通过bash运行
```

*mac下挂载volume volume存储在docker自己创建的虚拟机中*

#### dockerfile

` docker build -t image_name .`
制作ubuntu系统带着c环境的例子

```Dockerfile
# Use an official Ubuntu runtime as a parent image
FROM ubuntu:latest

# Set the working directory in the container
WORKDIR /usr/src/app

# Update and install necessary dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    g++-12 \
    clang-15

# Set the default C++ compiler alternatives
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 100 \
    && update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-12 100 \
    && update-alternatives --install /usr/bin/clang clang /usr/bin/clang-15 100

# Set environment variables
ENV CC=gcc
ENV CXX=g++

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# CMD specifies the command to run on container start
CMD ["/bin/bash"]

```

启动容器
`docker run -it -d --name my_linux image_name`
进入容器
`docker exec -it container_name bash `

#### dockercompose



#### java