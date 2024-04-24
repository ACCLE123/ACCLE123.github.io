---
title: shortest path
date: 2024-04-23 19:20:30
tags:
---


### shortest path

there are three methods to solve this problem.

### bfs

bfs can solve this problem when all edge values are 1.

### dijkstra

dijkstra is one of the greedy algorithms.

it can solve this problem, provided that edge values are greater than 0.

$O(n^2)$

```c++
void dijkstra() {
    memset(st, false, sizeof st);
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n; i++) {
        int t = -1;
        for (int j = 1; j <= n; j++)
            if (st[j] == false && (t == -1 || dist[t] > dist[j]))
                t = j;
        st[t] = true;
        for (int j = 1; j <= n; j++)
            dist[j] = min(dist[j], dist[t] + g[t][j]);
    }
}
```

$O(n*logm)$

```c++
void dijkstra() {
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    memset(dist, 0x3f, sizeof dist);
    memset(st, false, sizeof st);
    dist[1] = 0;
    heap.push({dist[1], 1});

    while (heap.size()) {
        PII tt = heap.top();
        heap.pop();

        int t = tt.second;

        if (st[t]) continue;
        st[t] = true;

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[i];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                heap.push({dist[j], j});
            }
        }
    }
}

```

### spfa

the theroy of spfa is dp. 
spfa can solve the difference constraint and finding the circle in a graph.

```c++
void spfa() {
    memset(st, false, sizeof st);
    memset(dist, 0x3f, sizeof dist);
    
    hh = 0, tt = 0;
    q[tt++] = 1;
    st[1] = true;
    dist[1] = 0;
    
    while (hh != tt) {
        int t = q[hh++];
        if (hh == N) hh = 0;
        st[t] = false;

        for (int i = h[t]; i != -1; i = ne[i]) {
            int j = e[t];
            if (dist[j] > dist[t] + w[i]) {
                dist[j] = dist[t] + w[i];
                if (st[j] == false) {
                    st[j] = true;
                    q[tt++] = j;
                    st[j] = true;
                    if (tt == N) tt = 0;
                }
            }
        }
    }
}
```