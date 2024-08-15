---
title: leetcode
date: 2024-08-15 10:19:38
tags:
---


leetcode 脑筋急转弯 

### 31 下一个排列

1. 从结尾开始找 找到第一个 严格升序的位置
2. 找到这个位置之后 严格大于 这个数的最小的数
3. 交换这两个数
4. 交换后升序变降序


### 169 多数元素

非常bt的题

```c++

int majorityElement(vector<int>& nums) {
    int t = -1, cnt = 0;
    for (int num : nums) {
        if (t == num) cnt++;
        else cnt--;
        
        if (cnt < 0) {
            cnt = 1;
            t = num;
        }
    }
    return t;
}

```

### 