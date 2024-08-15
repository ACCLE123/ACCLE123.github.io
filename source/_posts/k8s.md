---
title: k8s
date: 2024-07-05 21:22:08
tags:
---

### k8s design

api对象是核心: 每引入一个新的功能，就会引入新的api对象

#### k8s api

k8s api是对 api对象 进行crud的借口

apiserver负责处理k8s api

#### 声明式

k8s api为声明式 (sql也是声明式)

与之对比的是 命令式

- 声明式: 我需要查询id=1的用户(结果)
- 命令式: 我需要 通过b+树 查询到id=1的用户(过程)

声明式api的优点

1. 后期优化的可能性
2. 幂等

#### 避免简单封装

简单封装会隐藏内部机制

比如 replicaset 和 statusfulset 是两种不同的pod集合 需要区分

### etcd

etcd是k8s中**唯一**的持久化存储

pod的信息  secret 、configmap中的信息 ...都存储在etcd中

etcd的特点：

1. 高可用 多个节点部署etcd
2. 强一致 raft算法保证



### api resource

api资源 也叫 api对象

api对象是k8s的关键

api对象三大属性: metadata(元数据) status(状态)  spec(规范)   


#### pod

pod是container的抽象 核心单位

#### deployment

是pod的抽象

#### service

用于内部通信 内部IP

#### ingress

用于外部通信

#### cdr

自定义资源

istio中的组件都是cdr


### istio

服务网格

#### istiod

control plane

以pod的形式存在

管理着全部pod

#### gateway

以pod的形式存在

#### virtual service

转发规则 作用在service上

#### destination rule

请求进入规则 用于作用在 service上
