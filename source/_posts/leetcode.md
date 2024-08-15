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

### 26 移除重复元素

```c++
int removeDuplicates(vector<int>& nums) {
    int k = 0;
    for (int x : nums)
        if (k == 0 || x != nums[k - 1])
            nums[k++] = x;
    return k;
}
```


### 80 移除重复元素2

```c++
int removeDuplicates(vector<int>& nums) {
    int k = 0;
    for (int x : nums)
        if (k < 2 || (nums[k - 1] != x || nums[k - 2] != x))
            nums[k++] = x;
    return k;
}
```

### 189 翻转数组

```c++
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    k %= n;

    int i = 0, j = n - 1;
    while (i < j) swap(nums[i++], nums[j--]);

    i = 0, j = k - 1;
    while (i < j) swap(nums[i++], nums[j--]);

    i = k, j = n - 1;
    while (i < j) swap(nums[i++], nums[j--]);
}

```

### 45 跳跃游戏2

f[i]: 表示从0跳到i 最少的步数

结论： f[i]一段一段的， 单调递增，每次+1

```c++
int jump(vector<int>& nums) {
    int n = nums.size();
    vector<int> f(n + 1);

    for (int i = 1, j = 0; i < n; i++) {
        while (j + nums[j] < i) j++;
        f[i] = f[j] + 1;
    }
    return f[n - 1];
}
```