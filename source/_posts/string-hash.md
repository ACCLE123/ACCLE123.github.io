---
title: string hash
date: 2024-07-21 15:44:23
tags:
---

### 多项式

效果: 将substr操作从`O(n)` 优化到 `O(1)`

预处理`O(n)` 比较相等`O(1)`

右边为低位 左边为高位

题目: 回文子串的最大长度、后缀数组

```c++
#include <iostream>
#include <cstring>
#define ULL unsigned long long
#define N 100010
#define P 131
using namespace std;

ULL p[N], h[N];
int n, m;
char s[N];

ULL get(int l, int r) {
    return h[r] - h[l - 1] * p[r - l + 1];
}

int main() {
    cin >> n >> m;
    cin >> s + 1;
    
    p[0] = 1;
    for (int i = 1; i <= n; i++) {
        p[i] = p[i - 1] * P;
        h[i] = h[i - 1] * P + s[i] - 'a' + 1;
    }
    
    while (m--) {
        int l1, r1, l2, r2;
        cin >> l1 >> r1 >> l2 >> r2;
        if (get(l1, r1) == get(l2, r2)) cout << "Yes" << endl;
        else cout << "No" << endl;
    }
    return 0;
}
```

### 最小表示法

环字符串的统一表示方法

预处理`O(n)` 比较相等`O(1)`

题目: 雪花雪花、项链、隐藏密码

```c++
void get_min(int a[]) {
    static int b[12];
    for (int i = 0; i < 12; i++)
        b[i] = a[i % 6];
    int i = 0, j = 1;
    while (i < 6 && j < 6) {
        int k = 0;
        while (b[i + k] == b[j + k]) k++;
        if (k >= 6) break;
        if (b[i + k] > b[j + k]) i += k + 1;
        else j += k + 1;
        if (i == j) i++;
    }
    i = min(i, j);
    for (int k = 0; k < 6; k++)
        a[k] = b[i + k];
}
```

