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

hash table and two-pointer algorithm

### 4

recursion
hard

it is a hard problem contained many border problem.

find k in two ordered sequences

```c++
double findk(vector<int>& nums1, int i, vector<int>& nums2, int j, int k) {
    if (nums1.size() - i > nums2.size() - j) return find(nums2, j, nums1, i, k);
    if (nums1.size() == i) return nums2[j + k - 1];
    if (k == 1) return min(nums1[i], nums2[j]);

    int si = min(i + k / 2, (int)nums1.size()), sj = j + (k - k / 2);

    if (nums1[si - 1] < nums2[sj - 1]) return find(nums1, si, nums2, j, k - (si - i));
    else return find(nums1, i, nums2, sj, k - (sj - j));
}
```

### 5

hard

two-pointer

### 10

dp

the `*` can match 0 or n

### 11



### 30



### 32

