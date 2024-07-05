---
title: internship
date: 2024-07-02 10:31:29
tags:
---

### go

#### 数据组织方式

1. struct
2. interface

#### interface

go语言面向接口编程

接口是go的核心概念

##### 概念

1. 接口是一种数据类型
2. 接口是方法的集合
3. 行为定义类型 (不通过属性)
4. 鸭子类型: 任何类型只要实现了接口中的全部方法 那么就自动实现了该接口


##### 定义

```go
type Speaker interface {
    Speak()
}
```

##### 实现

```go
type Speaker interface {
	Speak()
}

type Person struct {
	name string
}

func (p Person) Speak() {
	fmt.Println(p.name, "i am person")
}
```

##### 空接口

(常用)

任何类型都实现了空接口

空接口用来处理任意类型

```go
func MyPrint(i interface{}) {
	fmt.Println(i)
}

// myPrint(123) myPrint(1.23) myPrint("123")
```

##### 类型断言

```go
var i interface{} = "hello"
s, ok := i.(string)
if ok {
	fmt.Println(s) // 输出: hello
} else {
	fmt.Println("Type assertion failed")
}
```

##### 类型选择

使用.(type) + switch 语句实现

```go
func DoingSomething(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Println("i am int", v)
	case string:
		fmt.Println("i am string", v)
	case Speaker:
		fmt.Println("i am speaker", v)
	}
}
```

##### 接口组合

小接口组合成大接口


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


### gitlab ci/cd

