---
title: xzdp
date: 2024-08-19 11:25:28
tags:
---

### 缓存类型选择

#### ByID

根据id查询单条数据

- hash
- string

hash: 可以修改其中的单条字段，但数据结构复杂

string: 数据结构简单，但无法只修改其中单条字段

### 缓存穿透

#### 概念

指某个热点key过期 导致全部缓存打到数据库上

#### 解决方案

1. 加锁构建缓存，其余查询等待缓存构建完成
2. 逻辑过期，加锁开线程构建缓存，获取锁失败的线程返回过期字段

#### 测试

### 缓存工具类

实现string类型的缓存工具类

方法:
1. 存储string 设置过期时间
2. 存储string 设置逻辑过期时间
3. 查询string 假如没有 缓存空值
4. 查询string 假如没有 开线程构建缓存


### 分布式id

#### 特性
1. 无重复
2. 单调增
3. 安全

#### 常见解决方案
1. 雪花算法
2. [leaf](https://tech.meituan.com/2017/04/21/mt-leaf.html)
3. redis

### 分布式锁

#### 悲观锁 乐观锁

乐观锁 和 悲观锁 作用于 数据库的 行 或 表

mvcc的实现采用了乐观锁的思想

悲观锁

```sql

BEGIN;

SELECT stock FROM products WHERE product_id = 1 FOR UPDATE;

UPDATE products SET stock = stock - 1 WHERE product_id = 1;

COMMIT;

```

乐观锁

```sql

SELECT stock FROM products WHERE product_id = 1;

```

上面两种锁都是mysql锁

#### redis锁

利用setnx


