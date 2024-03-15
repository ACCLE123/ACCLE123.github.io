### data_structure


#### 线段树

segment tree

以 logn 的复杂度 进行 区间修改 单点修改 区间查询

##### 初始化



##### 单点修改



##### 区间查询



##### 区间查询



#### union-find

进行动态的维护一些集合

```c++
struct UnionFind {
    vector<int> d;
    UnionFind(int n): d(n, -1) {}
    int find(int x) {
        if (d[x] < 0) return x;
        return d[x] = find(d[x]);
    }
    bool unite(int x, int y) {
        x = find(x); y = find(y);
        if (x == y) return false;
        if (d[x] > d[y]) std::swap(x, y);  
        d[x] += d[y];
        d[y] = x;
        return true; 
    }
    bool same(int x, int y) { return find(x) == find(y); }
    int size(int x) { return -d[find(x)]; }
};
```
