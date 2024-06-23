---
title: lca
date: 2024-06-23 11:32:50
tags:
---


lca: 最近公共祖先 树上差分 树上求最大值


模版

### build

```c++
void bfs(int u) {
    memset(depth, 0x3f, sizeof depth);
    depth[0] = 0;
    depth[u] = 1;
    int hh = 0, tt = -1;
    q[++tt] = u;
    
    while (hh <= tt) {
        int t = q[hh++];
        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (depth[j] > depth[t] + 1) {
                depth[j] = depth[t] + 1;
                fa[j][0] = t;
                q[++tt] = j;
                
                for (int k = 1; k <= 15; k++) 
                    fa[j][k] = fa[fa[j][k - 1]][k - 1];
            }
        }
    }
}
```


### lca


```c++
int lca(int a, int b) {
    if (depth[a] < depth[b]) swap(a, b);
    
    for (int k = 15; k >= 0; k--)
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
            
    if (a == b) return a;
    
    for (int k = 15; k >= 0; k--)
        if (fa[a][k] != fa[b][k]) {
            a = fa[a][k];
            b = fa[b][k];
        }
    return fa[a][0];
}

```