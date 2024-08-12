---
title: cap
date: 2024-08-07 15:51:54
tags:
---

### cap

#### Consistency

数据库要求一致


#### Availability

总是可以请求

#### Partition Tolerance

不同的网络出现故障 无法相互通信 系统仍然可用

CA系统: 传统的关系型数据库

CP系统: 要求一致性

AP系统: 要求可用性

支付系统：强一致+高可用，极端情况下可以牺牲可用性

### BASE

BASE是AP系统的设计原则

基本可用性（Basically Available）、软状态（Soft state）和 最终一致性（Eventually consistent）

### ACID

ACID是CA系统的设计原则

原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和 持久性（Durability）

