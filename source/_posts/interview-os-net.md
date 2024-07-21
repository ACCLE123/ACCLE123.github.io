---
title: interview-os-net
date: 2024-05-30 20:29:01
tags:
---

#### shell脚本常用内置变量

1. `$0` 该脚本文件名
2. `$n` 传输的第n个参数
3. ` $$` 该shell脚本的进程id
4. `$#` 传递的参数个数
5. `$@` 传递的全部参数
6. `$*` 传递的全部参数，以字符串的形势传入
7. `$!` 上一个后台进程的进程id
8. `$?` 上一个命令运行状态 0表示成功


#### 文件系统


#### cookie session token

cookie: 存储在游览器的kv对，每次给服务器请求都会带上cookie

session: 前端发起请求时，服务器生成一个sessionId存储到数据库，前端每次发请求带着sessionId就不需要用户名密码.

session: 方案会在服务器存储sessionId,用户多会导致存储大量sessionId.

token: jwt技术(json web token), 服务器保存jwt的密文, 游览器通过cookie的方式保存jwt token
