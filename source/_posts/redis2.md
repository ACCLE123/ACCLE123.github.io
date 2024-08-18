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

