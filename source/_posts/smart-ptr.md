---
title: smart ptr
date: 2024-08-13 10:21:32
tags:
---


### shared_ptr

对指向内存维护一个共享计数器


### weak_ptr

打破循环引用

底层是通过lock方法

lock方法在weak_ptr调用时 共享计数器>0 生成shared_ptr 共享计数器==0 生成空的shared_ptr

### 打破循环引用

```c++

#include <iostream>
#include <memory>
using namespace std;

struct Node {
  shared_ptr<Node> next;
  weak_ptr<Node> pre;

  int val;
};

int main() {
  shared_ptr<Node> node1 = make_shared<Node>();
  shared_ptr<Node> node2 = make_shared<Node>();

  node1->next = node2;
  node2->pre = node1;


  return 0;
}

```


### unique_ptr

独占
