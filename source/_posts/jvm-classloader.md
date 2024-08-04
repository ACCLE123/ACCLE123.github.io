---
title: jvm classloader
date: 2024-08-04 14:16:55
tags:
---

[剑指jvm](https://weread.qq.com/web/reader/30932dd0813ab8194g0175fd)

### 类转载的过程

加载->链接->初始化


#### 加载

将.class文件放入内存中

加载阶段需要使用**类加载器**

#### 链接

分为验证 准备 解析

将符号引用变为直接引用

#### 初始化

调用<clinit>方法

> clinit方法是线程安全的，多个线程尝试初始化一个类，只有一个线程可以只能clinit方法


### 类加载器