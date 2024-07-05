---
title: dp special sequence
date: 2024-06-30 15:27:00
tags:
---

[特殊子序列](https://leetcode.cn/circle/discuss/tXLS3i/)

[leetcode周赛404 第三题](https://leetcode.cn/problems/find-the-maximum-length-of-valid-subsequence-ii/description/)

### 数值子序列

这类dp的特点 

* 子序列合法 和 原序列排序相关
* 子序列合法 和 数值相关
* 一般求最大长度

状态定义:

 `f[i]` 数值为 `i` 的最大合法子串长度