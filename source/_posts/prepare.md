---
title: prepare
date: 2024-06-12 17:05:00
tags:
---

### 二维地图可达性

并查集维护联通块


### 给定rand(x) 从n人选m人

n人存到数组中
每次rand(n), 选中的人换到最后一个位置，rand(n - 1) 执行m次

### 100w玩家排序

平衡树


### 苦力怕爆炸算法实现

xy坐标系 考虑正方形选实体方块点 进行判断

极坐标系 直接判断

### c语言编译过程中链接是怎么实现的 

gcc: 预处理 编译 汇编 链接

预处理: 宏定义替换 头文件展开

编译: .c->.s转化为汇编语言

汇编: .s->.o汇编转机器码

链接: 目标文件和库文件链接 形成可执行文件

链接: /usr/lib目录下

动态链接文件: `.so` linux    `.dylib` macos

静态链接文件: `.a`

动态链接和静态链接: 动态链接 运行时加载 静态链接 编译加载
