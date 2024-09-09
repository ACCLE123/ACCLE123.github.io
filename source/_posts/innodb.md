---
title: innodb
date: 2024-09-09 11:08:20
tags:
---


mysql8.4官方文档

### innodb内存结构

In-memory structure

---

#### buffer pool

主要结构是一个LRU双链表 存储了一些页(page)

分为两段 5/8的新生段(new sublist) 和 3/8的老年段(old sublist)

优先淘汰old sublist

当页被第一次访问时 放入老年段

当在页面被第二次访问时 放入新生段

被read-ahead载入的数据 不会由老年段 提升到新生段

---

#### change buffer

由于二级索引的修改操作可能会影响大量数据 所以change buffer来缓存这些dml

change buffer在内存中是buffer pool的一部分

change buffer记录 修改二级索引的 dml操作

当page被载入buffer pool中 将这些dml 会merge到page上

merge后会正常写入硬盘

所以索引并不是越多越好 索引过多 这样一次同步会带来很大的代价

---

#### adaptive hash index

访问频率高的数据 会被放入一个内存中的hash表中

---

#### log buffer

dml操作会先被记录到log buffer

一般情况 事务提交前 需要把log buffer 同步到 redo log(硬盘结构)中

取决于 `innodb_flush_log_at_trx_commit`字段

* 0: 性能最佳 写入log buffer即可提交事务
* 1: 安全性最好 写入redo log才可以提交事务
* 2: 折中方案

--- 

### innodb硬盘结构

On-disk structure

---

#### tables


#### indexes

#### tablespaces

#### doublewrite buffer

#### redo log

#### undo log
