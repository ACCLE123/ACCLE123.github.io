---
title: redis2
date: 2024-08-03 14:30:57
tags:
---

[黑马](https://www.bilibili.com/video/BV1cr4y1671t?p=145&vd_source=d16831a284464de419a47ae6c0e03fea)
[redis源码分析](https://weread.qq.com/web/reader/d35323e0597db0d35bd957b)

### 数据结构

### 网络模型

### 通信协议


### 内存策略

#### 内存过期

1. 惰性删除
2. 周期删除

#### 内存淘汰

1. lru
2. lfu

### 一致性问题

1. 修改删除缓存
2. 查询建立缓存

先修改数据库，再删除缓存

### 复制

主从redis： 主负责写操作 从负责读操作

#### 主从搭建

`docker-compose.yml`

```yml
version: '3'
services:
  redis-master:
    image: redis:latest
    container_name: redis-master
    ports:
      - "6389:6379"
    command: ["redis-server", "--appendonly", "yes"]

  redis-slave1:
    image: redis:latest
    container_name: redis-slave1
    ports:
      - "6390:6379"
    command: ["redis-server", "--slaveof", "redis-master", "6379", "--appendonly", "yes"]
    depends_on:
      - redis-master

  redis-slave2:
    image: redis:latest
    container_name: redis-slave2
    ports:
      - "6391:6379"
    command: ["redis-server", "--slaveof", "redis-master", "6379", "--appendonly", "yes"]
    depends_on:
      - redis-master
```


`info replication`: 查看集群信息

机制:
1. 指令传播：建立主从关系后 主节点自动同步写操作给从节点
2. 心跳检测：从节点主动发起`ack <offset>`给主节点 


#### 主从同步

每一个redis有一个replication backlog，这个东西是一个循环队列，记录的主的写指令

redis-cli是redis服务器的客户端

从服务器是主服务器的客户端

`psync <hostid> <offset>`: 从向主请求同步

`hostid`: 从会保存上一个主的id

`offset`: replication backlog中同步了的数据大小

主收到psync后：

1. 对比自身id和hostid是否一致
2. 一致 向从发送 部分同步 指令
3. 不一致 向从发送 全同步 指令 并附带自己的id 让从记录hostid


### Sentinel