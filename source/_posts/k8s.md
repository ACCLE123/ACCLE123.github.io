---
title: k8s
date: 2024-07-05 21:22:08
tags:
---

### k8s

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
