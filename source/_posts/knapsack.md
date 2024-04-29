---
title: knapsack
date: 2024-04-29 13:36:57
tags:
---

### 0/1 knapsack

each stuff is just used solely once.

there are a lot of question is 0/1 knapsack problem.

and we are requerid to get some value about maximum, minimum, count and so on.

$O(n * m)$

```c++
init(); // initital the f
for (int i = 1; i <= n; i++)
    for (int j = 0; j <= m; j++) {
        f[i][j] = max(f[i][j], f[i - 1][j]) // don't take i
        if (j >= v[i]) f[i][j] = max(f[i][j], f[i - 1][j - v[i]] + w[i]); // take i
    }

```

it is can be simpliy

```c++
init();
for (int i = 1; i <= n; i++)
    for (int j = m; j >= v[i]; j--)
        f[j] = max(f[j], f[j - v[i]] + w[i]);
```

### unbounded knapsack

we can take each stuff unlimit.

$O(n * m)$

```c++
init();
for (int i = 1; i <= n; i++)
    for (int j = v[i]; j <= m; j++)
        f[j] = max(f[j], f[j - v[i]] + w[i])
```