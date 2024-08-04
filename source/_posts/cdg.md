---
title: cdg
date: 2024-08-02 12:08:58
tags:
---

腾讯金融的面试题总结

来自
[CDG-腾讯提前批-一面](https://www.nowcoder.com/discuss/647580753970089984?sourceSSR=users)
[CDG-腾讯金融科技-二 面](https://www.nowcoder.com/discuss/648302980785090560?sourceSSR=users)
[腾讯 CDG-金融科技 一面](https://www.nowcoder.com/feed/main/detail/836bcfee26de408482c786bacaa5d047?sourceSSR=users)

### spring

#### spring是什么？spring和springboot区别

springboot 有自动配置(自动配置（Auto-Configuration）) 

springboot 内嵌了tomcat服务器

#### 循环引用是什么？怎么解决

#### bean的创建方法和引用方法

#### Spring的启动流程？

#### 用过Spring框架吗？说说一个HTTP请求从打到服务器到被解析处理到最后返回的全过程链路，越详细越好

#### 哪个组件负责HTTP请求的解析？

DispatcherServlet

#### Java的Spring用到了哪些设计模式？越多越好



### java

#### HashMap的扩容机制

1. 当 count / size >= 负载因子时触发扩容
2. size *= 2 触发 rehash

#### 代理模式是怎么用到的？如果要你自己用代码实现动态代理，应该怎么做？如何做到原生接口来实现？

#### Java数据结构用过哪些？

1. ArrayList
2. HashMap
3. HashSet

#### 为什么链表转红黑树的临界个数是8？为什么红黑树退化成链表的临界个数是6？

转化为红黑树 为了加快时间
转化为链表 为了节约空间
8 和 6 是为了实现上述优化的临界 同时6 和 8 之间存在一定差距，这样可以避免频繁转换

### java并发

#### java内存模型JMM

主内存: 所有变量存储在主内存

工作内存: 每个线程都有自己的工作内存

#### 讲讲Java并发系统

1. 线程和线程池 Thread Executor
2. 锁 synchronized synchronized
3. 并发集合和原子变量

#### 现在创建十个线程，每个线程有一个跑步的方法，如何保证十个线程同时执行跑步方法，保证赛跑的公平性？

相似场景: 10个玩家同时进入游戏

CountDownLatch 相当于 go 语言中的waitgroup

CountDownLatch只能减少计数器

waitgroup可以增加和减少计数器

利用ready和begin来实现 同步进入游戏


#### 直接重启服务器，线程池的关闭流程是怎么样的？

1. 线程池中执行任务的线程会直接停止
2. 队列中的任务会直接丢失

#### 如果我需要在服务运行期间关闭线程池，应该怎么办？

1. shutdown(): 停止接受新任务，正在执行的任务 和 队列中的任务 都会被执行完，该方法不会阻塞
2. shutdownNow(): 停止接受新任务, 正在执行的任务 全部执行interrupt，队列中的任务全部作为返回值，都会被执行完，该方法不会阻塞
3. awaitTermination(): 等待 正在执行的任务 和 队列中的任务 被执行完，该方法阻塞。

### jvm

#### jvm内存结构

#### jvm问zgc和g1，垃圾回收算法

#### JDK8的垃圾回收器是什么？有什么特点？

- Serial GC：单线程，适合简单、低并发环境。
- Parallel GC：多线程，注重高吞吐量。
- CMS GC：低延迟，适合交互式应用。
- G1 GC：适合大内存，能更好地控制暂停时间。

#### 垃圾回收算法

1. 标记清楚
2. 标记整理 （老年代）
3. 复制 （新生代）

#### 为什么要分代

分代指的是对堆进行划分


#### 如果我自己定义一个类，类名叫 String，这个类能生效吗？

1.	类名冲突：
•	由于 String 是 Java 标准库中的类名，自己定义一个同名类可能会导致混淆和意外错误。
2.	包作用域：
•	如果你在自己的包中定义 String 类，Java 将根据当前包的作用域解析类名。比如，如果你的类在 com.example 包中定义，那么在 com.example 中 String 将引用你定义的类，而不是 java.lang.String。
3.	使用完全限定名：
•	在需要使用标准库中的 String 时，必须使用完全限定名 java.lang.String 以避免冲突

#### 讲讲类加载过程，并说明为什么是双亲委派模型？

双亲委派模型是一种类加载机制，加载类时优先让父类加载器尝试加载。如果父类加载器无法加载，再由子加载器尝试。它的优点是

具体过程

1.	启动类加载器（Bootstrap ClassLoader）：
•	加载 Java 核心库。
2.	扩展类加载器（Extension ClassLoader）：
•	加载 lib/ext 目录下的扩展类库。
3.	应用类加载器（Application ClassLoader）：
•	加载应用程序类路径下的类。


### net

#### tcp是什么， udp是什么，三次握手的seq可以一样吗、同一个端口可以接受tcp和udp连接吗、如果手机用tcp连接突然关机又开机，tcp会重新连上吗

TCP 和 UDP 的简介
- **TCP (Transmission Control Protocol)**：一种面向连接的协议，提供可靠的、有序的数据传输，广泛用于 HTTP、FTP 等应用。
- **UDP (User Datagram Protocol)**：一种无连接的协议，提供快速但不可靠的数据传输，常用于实时应用如视频流和在线游戏。

三次握手的 SEQ 可以一样吗？
- 在三次握手中，客户端和服务器的初始序列号（SEQ）通常是不同的，它们是随机生成的，以增加连接的安全性和可靠性。

同一个端口可以接受 TCP 和 UDP 连接吗？
- 是的，同一个端口可以同时接受 TCP 和 UDP 连接。这是因为 TCP 和 UDP 使用不同的传输层协议，操作系统可以区分和处理不同类型的流量。

手机用 TCP 连接突然关机再开机会重新连上吗？
- 不会。如果手机突然关机，原有的 TCP 连接会中断。重新开机后，之前的连接状态信息丢失，需要重新建立新的连接。

#### 大文件分片上传具体怎么做的，如果文件丢失怎么处理、文件损坏怎么办

大文件分片上传通常包含以下步骤：
	1.	文件分片：将大文件分成多个小片段（chunks），每个片段大小相同。
	2.	上传每个片段：通过多次请求分别上传每个片段，可以并行上传以提高速度。
	3.	合并片段：所有片段上传成功后，服务器将这些片段合并成完整文件。

处理文件丢失和损坏
	1.	文件丢失：通过片段编号和校验来确保所有片段都被成功上传。丢失的片段可以重新上传。
	2.	文件损坏：每个片段上传时计算校验和（如 MD5），服务器验证后存储。上传完成后，服务器再次验证整体文件的完整性。


#### HTTP的底层协议是什么？

tcp

#### 说说HTTPS的握手和加密过程

非对称加密加密密钥

对称加密加密传输数据

#### HTTP协议有什么特点？说得越多越好

#### 了解半链接吗？什么是半链接攻击？怎么解决？

#### 流量控制机制大概流程



### redis

#### 商品点赞排行榜用Redis里的什么结构实现？zset底层的数据结构和原理是什么？

#### 分布式锁解决的问题

普通的锁 只能锁住单个节点的线程
分布式 锁住多个节点的线程

#### redis分布式锁相关

`SETNX` 实现分布式锁

1. setnx 假如成功 说明获取锁成功
2. 设置过期时间
3. 释放锁

需要确保持有锁的线程 才能释放锁 使用lua脚本实现

redis中lua脚本具有原子性


#### redis怎么实现热点数据的发现，具体该怎么做

[热key和大key解决](https://fengxiu.tech/archives/48798.html)

热key问题: 某个key被高频访问，导致全部请求打在一个节点上

热key发现:

1. 预估
2. 代理层统计
3. `redis-cli --hotkeys`

热key的解决:


大key问题: 会造成阻塞

大key发现:

1. `redis-cli --bigkeys`

解决方法:

1. unlink进行后台删除
2. 业务拆分 或者 数据结构拆分

#### 如何实现Redis的高可用？

哨兵模式

##### 持久化

rdb: 某一时刻将全部数据以二进制的方式写入硬盘

aof: 将执行的指令追加到一个日志文件中

##### 集群

- 主从(高并发)

主节点 负责写

从节点 负责读

主节点将数据修改同步到从节点

全量同步：使用rdb文件进行同步

增量请求: 通过版本号+偏移量进行同步

- 哨兵(高可用)

监控通知

自动故障恢复

基于心跳进行检测

主观下线： 当少量哨兵 发现没有心跳 

客观下线： 超过一定数量哨兵 发现没有心跳 



- 分片(大量写 大量存储)

多个主节点

多个主节点做心跳检测

用hash槽来确定每个key该放在哪个主节点上


### mysql

#### mysql索引结构，为什么b+树，和b树对比优势，聚簇索引、非聚簇索引区别，索引为什么快、索引一定快吗、怎么排查慢sql、select *为什么慢、mysql意向锁是什么

#### MySQL的索引是什么结构，有什么特点？



### docker

#### docker的层次结构

- Docker deamon
采用c/s架构,服务端采用一个守护进程来接受客户端的命令
- Docker Image
- Docker Container
- Docker Registry

#### docker安装mysql和redis需要把哪些东西映射出来，为什么

映射内容:
1. 数据
2. 配置文件

因为方便管理

### else

#### 红黑树是一种什么样的结构？有什么特点？向红黑树中插入一个数，内部的平衡机制是怎么样的？

#### threalocal怎么避免内存泄漏

内存泄漏的原因是线程的生命周期 和 变量绑定

调用remove方法清理

#### 微服务，微服务有什么好处，如何拆分，除了功能外还有什么拆分思路

独立部署、拓展性、开发效率

按数据一致性进行拆分


#### 非对称加密，对称加密有哪些算法

对称加密： AED
非对称加密： RSA、ECC

### 算法

#### 三色旗

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int i = 0, j = 0, k = nums.size() - 1;
        while (i <= k) {
            if (nums[i] == 0) swap(nums[i++], nums[j++]);
            else if (nums[i] == 1) i++;
            else swap(nums[k--], nums[i]);
        }
    }
};
```

#### 一个数组、子数组和大于一个数的长度最小值

#### 最长递增子序列