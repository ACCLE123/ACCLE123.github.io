---
title: scc
date: 2024-06-09 14:20:18
tags:
---

有向图的强连通分量

### 题目

[atcoder](https://atcoder.jp/contests/abc357/tasks/abc357_e)

### 模版

```c++
int dfn[N], low[N], timestamp;
int stk[N], tt = -1;
bool in_stk[N];
int id[N], Size[N], scc_cnt;

void targan(int u) {
    stk[++tt] = u;
    in_stk[u] = true;
    dfn[u] = low[u] = ++timestamp;

    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (!dfn[j]) {
            targan(j);
            low[u] = min(low[u], low[j]);
        } else if (in_stk[j]) low[u] = min(low[u], dfn[j]);
    }

    if (low[u] == dfn[u]) {
        int j;
        scc_cnt++;
        do {
            j = stk[tt--];
            in_stk[j] = false;
            id[j] = scc_cnt;
            Size[scc_cnt]++;
        } while (j != u)
    }
}

```