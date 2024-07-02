---
title: 6.2 intership
date: 2024-07-02 10:31:29
tags:
---


### go


### k8s

[crash course](https://www.youtube.com/watch?v=s_o8dwzRlu4)

#### node 和 pod

node分为: control plane nodes 和 worker nodes

##### control plane nodes

1. API Server
2. Controller Manager
3. Scheduler
4. etcd


##### Worker Nodes

1. kubelet
2. kube-proxy
3. Container Runtime


#### services ingress

services: pods永久的ip

ingress: 用来路由ip

#### configmap secret 

用来进行外部配置

#### volume

用来进行持久性存储

#### deployment statefulset

用来进行复制

pod是container的抽象
deployment是pod的抽象

#### configuration

master中的API Server

etcd位置status



#### minikube kubectl