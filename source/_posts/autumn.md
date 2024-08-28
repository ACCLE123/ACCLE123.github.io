---
title: Autumn Recruitment
date: 2024-08-17 16:40:22
tags:
---

## 总结

秋招挂汇总:

1. 字节 一面挂
2. 腾讯 二面挂
3. 讯飞qt 一面挂
4. 米哈游cpp 一面挂
5. pdd 笔试挂
6. 网易雷火 笔试挂
7. 美团 一面挂
8. 讯飞paas 一面挂
9. 腾讯广告 一面挂


流程中:
1. 百度 等结果
2. 滴滴 等结果
3. 京东二面
4. 字节机器学习一面


## leetcode

## hard

### redis 执行lua脚本时 redis故障 重启redis发生什么

1. rdb恢复redis数据库
2. aof保证lua脚本原子性

### mysql 的引擎为什么用b+树

红黑树和B+树: 

1. 红黑树的比较次数比B+树少，向下读取的层数 比B+树多
2. 比较操作CPU执行 比较快，向下读取 

B树和B+树:

### spring A方法调用B方法 只在B方法加事务 事务是否生效

```java
// controller

public Result C() {
    return A();
}

// service
public Result A() {
    B();
}

@Trasaction
public Result B() {
}

```

1. 事务基于动态代理
2. 动态代理拿到对象Service对其代理
3. 调用方法B是Service的方法B 不是代理对象的方法B
4. 解决方法是拿到代理对象 手动调用方法B

### kubectl创建deployment全过程

1. 指令传给 api server
2. api server 将信息存储到 etcd
3. controller managerment中的组件deployment controller 监控 etcd
4. deployment controller 创建一个 replicaSet, 这个replica set定义了要创建多少个pod
5. replica set controller 将pod信息存储到 etcd中
6. schduler为每个pod进行node的选择 将选择的信息存回etcd
7. node上的kubelet进行pod的创建
8. 网络插件配置为pod配置ip
9. kubelet将信息写回etcd
10. kube-proxy配置服务发现和负载均衡


## redis

### redis数据结构(5)

1. string
2. hash
3. list
4. set
5. zset(跳表)

### redis为什么怎么快(2)

1. io多路复用 + 单一工作县城
2. 内存数据库
3. 数据结构高效

### 持久化(2)

1. aof
2. rdb

### 过期键淘汰策略(1)

1. 访问删除
2. 定期删除(抽取一部分 检查是否过期)

### 内存淘汰策略(1)

1. lru (带来缓存污染问题 不常用的大数据被读入)
2. lfu (注意redis的lfu是改进版 计数器会随时间衰减)

### 缓存一致性(1)

cache aside 的一致性

读构建缓存 
写删除缓存

先修改数据库 再删除缓存

## mysql

### 隔离级别

1. 读未提交
2. 读已提交
3. 可重复读
4. 串行化

### 索引失效(2)

1. 函数、计算、隐式类型转换
2. 不符合最左前缀法则
3. 模糊查询%开头

### explain(2)

查询执行计划

### innodb特性(1)

事务 外键 行级表


### mvcc(1)

1. 记录自带会滚指针 事务id
2. 回滚指针undolog 形成多个 链表
3. readview(每一次快照读 创建 记录活跃事务id 当前的事物id)