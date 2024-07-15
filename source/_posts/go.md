---
title: go
date: 2024-07-14 13:20:54
tags:
---

### GMP

代码在 `runtime/runtime2.go` 和 `runtime/proc.go`

G: 用户线程 也叫进程 goroutine
M: 内核线程 work thread
P: 调度器 proccessor

1. 每一个G都有自己的栈
2. 每一个P都有一个队列(LRQ)存储了G, 全局有一个存储G的队列(GRQ)
3. 一个G的某一时刻对应一个正在运行的函数, 一个G可以执行多个函数

#### M0 G0

1. M0在全局，负责一些初始化操作
2. G0用来进行垃圾回收 管理调度，不执行任何用户代码
3. 每一个M都会对应一个G0
4. main函数必须由M0执行

#### 阻塞

系统态阻塞: 在G执行syscall时，执行这个G的M会被阻塞，这时会将M对应的P交给别的M进行处理

用户态阻塞: channel操作 network I/O阻塞(netpoller)，这时会阻塞G，不会阻塞M，这个被阻塞的G，会被移到某个等待队列上，等待被别的G唤醒


### Memory Mode

定义多个G共享内存时的行为规范

The Go memory model specifies the conditions under which reads of a variable in one goroutine can be guaranteed to observe values produced by writes to the same variable in a different goroutine.

1. 数据竞争: 多个G访问同一块内存，并且至少有一个G为写操作，那么就会发生数据竞争
2. 同步原语: synchronization primitives
3. `Happens-before` : `A Happens-before B`，那么A的数据操作对于B是可见的


互斥锁`sync.WaitGroup` 和 `chan` 都可以实现 `Happens-before`


