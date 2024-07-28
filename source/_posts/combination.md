---
title: combination
date: 2024-07-27 21:10:16
tags:
---
组合问题 (Combination Problem)

选择的顺序没有关系

### 基本问题

1. 递归实现指数型枚举 $O(2^n)$
2. 递归实现组合型枚举 $O(C_{n}^{m})$

#### 指数枚举

```c++
#include <iostream>
#include <cstring>
#define N 16
using namespace std;

int n;
bool st[N];

void dfs(int u) {
    if (u == n) {
        for (int i = 1; i <= n; i++)
            if (st[i]) cout << i << ' ';
        cout << endl;
        return;
    }
    st[u + 1] = true;
    dfs(u + 1);
    st[u + 1] = false;
    
    dfs(u + 1);
}

int main() {
    cin >> n;
    dfs(0);
    
    return 0;
}
```


#### 组合枚举

```c++
#include <iostream>
#include <cstring>
#define N 30
using namespace std;

int n, m;
bool st[N];

void dfs(int u, int m) {
    if (n - u < m) return;
    if (!m) {
        for (int i = 1; i <= n; i++)
            if (st[i]) cout << i << ' ';
        cout << endl;
        return;
    }
    
    st[u + 1] = true;
    dfs(u + 1, m - 1);
    st[u + 1] = false;
    
    dfs(u + 1, m);
}

int main() {
    cin >> n >> m;
    dfs(0, m);
    return 0;
}
```

### 01背包概念

每个物品有选和不选两种状态


### dfs

数据范围到40 需要双向dfs

acwing
1. 递归实现指数型枚举
2. 送礼物


### 贪心


### dp

O(n * m)




