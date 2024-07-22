---
title: io multiplexing
date: 2024-07-18 12:04:00
tags:
---

linux的io多路复用 epoll
macos、freeBSD多路复用 kqueue

### select and poll

1. 任何一个链接都是文件(一种特殊的文件叫socket)
2. linux系统管理文件使用文件描述符(file descriptor)

select 和 poll 两者的实现逻辑类似

程序通过select 或 poll 告诉kernel 不断扫描 files，将变化的file返回

`O(n)`的复杂度

### epoll

[epoll](https://medium.com/@avocadi/what-is-epoll-9bbc74272f7c)

`epoll` 是一个内核对象 用来管理多个文件描述符的事件通知

底层由 红黑树 和 链表


`ready list` 是一个链表，便利这个链表，认为是`O(1)`，因为ready list中fd个数和 总的管理的fd个数没有关系

红黑树用来维护file descriptor 的有序性, 根据id查找可以做到`logn`

### Level and Edge

#### 事件类型

Level triggered event and Edge triggered event

边缘触发事件 和 水平触发事件

边缘触发事件: file descriptor在进入ready list时 触发一次，触发后file descriptor会被移除ready list

水平触发事件: file descriptor只要在ready list中就会不断触发事件，触发后假如还可以读取，那么继续放入ready list,没有数据继续读区则移除

#### 文件描述符类型

There are two file descriptors kind blocking and non-blocking. The blocking descriptors block the process when it is not ready. On the other hand, the non-blocking file descriptor never blocks the process.

边缘触发事件通常搭配Non-block file descriptor，因为：

1. 避免数据丢失
2. 避免死锁





