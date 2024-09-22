---
title: redis 数据结构
date: 2024-05-15 23:41:20
tags:
---

#### sds

simple dynamic string

```c++
struct sdshdr {
    int len; 
    int free;  
    char buf[];    
};
```

##### 语法细节

sizeof(struct sdshdr) 是8 char数组大小为0

##### 空间预分配

思想： 在字符串增长时 为结构体分配更多的空间 减少之后字符串拓展重新分配的次数

代码在`sdsMakeRoomFor` 函数中

##### 惰性空间释放

思想： 字符串缩短时 不重新分配内存 将多余的字节 存入free中


##### 二进制安全

因为有len的存在，可以存储`\0`，所以可以存储二进制文件


#### listNode

redis中的链表是双向链表

```c++

typedef struct listNode {

    // 前置节点
    struct listNode *prev;

    // 后置节点
    struct listNode *next;

    // 节点的值
    void *value;

} listNode;

```

为了管理方便

```c++
typedef struct list {

    // 表头节点
    listNode *head;

    // 表尾节点
    listNode *tail;

    // 节点值复制函数
    void *(*dup)(void *ptr);

    // 节点值释放函数
    void (*free)(void *ptr);

    // 节点值对比函数
    int (*match)(void *ptr, void *key);

    // 链表所包含的节点数量
    unsigned long len;

} list;
```



#### dict

```c++
typedef struct dictht {
    
    // 哈希表数组
    dictEntry **table;

    // 哈希表大小
    unsigned long size;
    
    // 哈希表大小掩码，用于计算索引值
    // 总是等于 size - 1
    unsigned long sizemask;

    // 该哈希表已有节点的数量
    unsigned long used;

} dictht;
```

```c++

typedef struct dictEntry {
    
    // 键
    void *key;

    // 值
    union {
        void *val;
        uint64_t u64;
        int64_t s64;
    } v;

    // 指向下个哈希表节点，形成链表
    struct dictEntry *next;

} dictEntry;
```

```c++
typedef struct dict {

    // 类型特定函数
    dictType *type;

    // 私有数据
    void *privdata;

    // 哈希表
    dictht ht[2];

    // rehash 索引
    // 当 rehash 不在进行时，值为 -1
    int rehashidx; /* rehashing not in progress if rehashidx == -1 */

    // 目前正在运行的安全迭代器的数量
    int iterators; /* number of iterators currently running */

} dict;
```

##### redis hash表

表的大小只可能为$2^n$

可以简化%运算

`hash(key) % table_size == hash(key) & (table_size - 1)`


##### hash值的计算

redis中使用了murmur hash算法
具体逻辑在`dictGenHashFunction`函数中


##### hash冲突

抽屉原理: 元素个数 > 集合个数，将元素放入集合中，必然会有一个集合存在两个元素

可以hash的东西是无限的，hash表大小有限，必然会存在冲突

redis中使用拉链法来解决hash冲突

##### rehash

当hash表变得 过于稀疏 或者 过于密集，这个时候需要改变hash表大小，需要进行rehash操作
(我们用负载因子，来表示稀疏或密集)

1. 创建新表
2. 重新计算hash值
3. 插入新表

具体逻辑在`dictRehash`函数中, 这个函数将表0中的entry迁移到表1中

##### 渐进式rehash

可以发现上面的操作很耗时O(n)
渐进式rehash： 在rehash的过程中可以进行 增删改查操作

1. get操作 查询 表1 和 表0
2. set操作 作用与 表1



#### skiplist

跳跃表

维护有序集合的数据结构

```c++
typedef struct zskiplistNode {

    // 成员对象
    robj *obj;

    // 分值
    double score;

    // 后退指针
    struct zskiplistNode *backward;

    // 层
    struct zskiplistLevel {

        // 前进指针
        struct zskiplistNode *forward;

        // 跨度
        unsigned int span;

    } level[];

} zskiplistNode;

```

```c++

typedef struct zskiplist {

    // 表头节点和表尾节点
    struct zskiplistNode *header, *tail;

    // 表中节点的数量
    unsigned long length;

    // 表中层数最大的节点的层数
    int level;

} zskiplist;u

```



#### intset

整数集合

#### ziplist

压缩链表

#### object

对象