---
title: leetcode300
date: 2024-04-26 13:33:05
tags:
---

I plan to code the 1-300 problem in leetcode, in order to pass the coding test.

### 1

time complexity is $O(nlogn)$ by using hash table

### 2

#### dummy
dummy is a special head, which has a node.
the normal head is nullptr.
dummy to avoid that head has no next pointer.

#### list insert

1. insert into head

```c++
ListNode* h = nullptr;

ListNode* t = new ListNode(1);

// insert
t->next = h;
h = t;
```

2. insert into last

```c++
// dummy can simplity 
ListNode* h = new ListNode();
ListNode* cur = h;

ListNode* t = new ListNode(1);

cur->next = t;
cur = cur->next;

```

### 3

hash table ans two-pointer algorithm

### 4

recursion
hard

it is a hard problem contained many border problem.

### 5

hard

two-pointer