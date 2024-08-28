---
title: infra
date: 2024-08-27 14:20:48
tags:
---

### 面试

1. 自我介绍 往mysql redis go,java,c++语言特性上引导 表示研究过jvm 声明k8s只是了解
2. 出现连续的问题不会的时候 请求换方向 记录问题

### 讯飞paas总结


#### istio中注册的数据流

1. istiod 从 api server中获取pod的信息
2. istiod 通过 xDS协议(一种rpc通信) 向 envoy通信
3. istiod控制 destination rule 、virtual service 、 gateway等进行流量监控

#### gin的底层

#### java设计一个yaml序列化 反序列化 工具包