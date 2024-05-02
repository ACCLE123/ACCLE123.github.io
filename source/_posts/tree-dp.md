---
title: tree-dp
date: 2024-05-01 15:25:16
tags:
---

### tree knapsack problem

it is a 0-1 knapsack extension. when we choose a stuff, we have to choose its father stuff.

it can be treat as a grouped knapsack problem.

we need to write 'for' three times

``` c++
for group
    for volume
        for each stuff
```

```c++
void dfs(int u) {
    for (int i = h[u]; i != -1; i = ne[i]) {
        int son = e[i];
        dfs(son);
        
        for (int j = m; j >= 0; j--)
            for (int k = 0; k <= j; k++)
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
    }
    for (int j = m; j >= v[u]; j--)
        f[u][j] = f[u][j - v[u]] + w[u];
    for (int j = 0; j < v[u]; j++)  
        f[u][j] = 0;
}
```

### tree diameter

### cover tree node or edge