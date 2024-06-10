---
title: prepare for interview
date: 2024-05-23 16:14:57
tags:
---

### java

#### 静态代码块执行时机

java中的数据类型分为基本数据类型 和 引用数据类型，引用数据类型需要加载到内存中才可以使用。

类的生命周期： 加载->链接->初始化->使用->卸载

链接阶段： 验证->准备->解析

静态变量 和 静态代码块

静态变量在 准备阶段 进行数据预分配(0, null, ...)

准备阶段不会为static final赋值，static final在编译阶段赋值

初始化阶段执行clinit方法：

<clinit> 方法的执行顺序

1. 静态变量初始化：按照它们在类中声明的顺序进行。
2. 静态代码块：按照它们在类中出现的顺序执行。

#### java线程状态

操作系统进程状态：

就绪 运行 阻塞 (挂起)

1. 就绪: 时间片用完
2. 运行: cpu在执行
3. 阻塞: io操作 或 sleep
4. 挂起: 进程从内存中到外存中

阻塞状态 恢复会到 就绪状态

java中线程的状态

java进程中线程的状态

1. RUNNABLE: jvm认为线程是可运行的，但是可能对应操作系统进程的(运行 阻塞 挂起), jvm不会知道到底是操作系统进程的哪个状态
2. BLOCK: synchorized导致
3. WAITING: wait()
4. TIME_WAITING: sleep(long), wait(long), ...

#### java io 和 nio

nio: Non-Blocking IO

1. nio面向buffer(使用直接内存)进行读写，使用channel进行双向通信，io面向字节流进行读写
2. nio可以使用非阻塞读写，使用selector实现多路复用，io不可以

#### jvm内存结构

线程私有: 程序计数器， java栈，c栈
线程共享: 方法区、java堆

程序计数器: 存储下一条需要执行的字节码指令，如果native方法，计数器为undefined

java栈(java虚拟机栈): 每执行一个方法，创建一个栈帧，方法执行完栈帧自动销毁，栈帧中的变量不需要gc来进行释放

栈帧: 局部变量表(slot为单位)，操作数栈，动态链接，方法返回地址

本地方法栈: native方法的方法栈

java堆: 存储对象和数组，gc进行管理，分为新生代，老年代，永久代。逻辑地址是连续的，物理地址不一定。(intern字符串缓存 和 静态变量 放在java堆中)

方法区: 存储类的基础信息，方法区放在元空间中，元空间放在本地内存中

本地内存(native memory): 不由jvm管理，由操作系统，里面有元空间 和 直接内存


#### ThreadLocal

ThreadLocal类中有一个ThreadLocalMap的静态内部类，每一个线程都有自己的ThreadLocalMap

```java
private void set(Thread t, T value) {
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        map.set(this, value);
    } else {
        createMap(t, value);
    }
}
```

从set可以看出，从Thread中取到自己的ThreadLocalMap

ThreadLocalMap key为ThreadLocal value为Object

造成内存泄漏的代码

```java
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value;

    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}

```

弱引用在垃圾回收时会直接将对象回收。

value是强引用，key是弱引用

导致key被回收但是value没有被回收



### springboot

#### spring bean生命周期

BeanDefination: 描述bean对象的信息

构造函数->依赖注入-> Aware接口->BeanPostProcessor#before->始化方法->BeanPostProcessor#after(aop)->销毁bean

 
#### springboot 自动配置原理

`@SpringBootApplication` 注解下

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```

`@SpringBootConfiguration` Configuration注解 

`@ComponentScan` 包扫描注解，默认扫描Application类在的包和他的子包

`@EnableAutoConfiguration` 该注解用于实现自动装配

在`@EnableAutoConfiguration` 下 存在注解 `@Import(AutoConfigurationImportSelector.class)` 导入ImportSelector类，该类实现了方法`selectImports`，这个方法从META的文件中导入类的全类名,将类放入ioc容器中


#### spring事物失效场景

1. 抛出异常为检查异常
2. 没有抛出异常
3. 方法为private
...

####  

 

### mysql

#### 四种隔离级别

读未提交(Read Uncommitted)、读提交(Read Committed)、可重复读(Repeatable Read)、串行化(Serializable)

mysql默认隔离级别是 Repeatable Read

#### mysql存储引擎

`> show engines;`

##### innodb

事物 外键 行级锁 

b+树作为索引

innodb存储结构（一个table）: 表空间 段 区 页 行


##### myisam

不支持事物 外键

不支持 行级锁    支持 表级锁

b+树作为索引

##### mymemory

hash表、b+树作索引

#### 锁

全局锁、表锁、行锁

#### select语句执行流程

连接器->查询缓存->解析sql->预处理->优化->执行

查询缓存在8.0之后被删去, 因为缓存命中率低

##### 连接

mysql 使用tcp协议, 三次握手建立连接

##### 解析sql

词法分析 + 语法分析

##### 预处理->优化->执行

预处理(prepare)->优化(optimize)->执行(execute)

预处理: 检查字段时候存在, * 的展开

优化: 确定执行计划(选择使用哪个索引), 可以用explain语句查看执行计划

执行: 从存储引擎读取数据

#### Innodb 存储结构

表空间->段->区->页->行

页(page): 16k 读写的单位

区(extent): 1M 数据量大时 为保证页连续 使用区为单位划分

段(segment): 数据段 索引段 回滚段

索引段存放非叶子节点

数据段存放叶子节点

b+树的节点是页(page)

#### 行的存储结构





### redis


#### 雪崩 击穿 穿透

##### 雪崩

为了保证一致性，缓存数据一般会设置过期时间

当同一时间，大量缓存数据过期 或 redis宕机，全部请求都会访问到数据库，导致数据库宕机

大量数据同时过期：
* 随机过期时间
* 互斥锁: 缓存未命中时，在请求数据库 和 构建缓存 当过程加锁
* 后台更新: 让缓存数据永久有效，业务线程不负责更新缓存，更新缓存的操作交给后台更新
    * 后台线程不断检测缓存
    * 消息队列通知

缓存预热: 当业务上线的时候，提前将数据缓存起来，后台更新也适合干这件事

redis宕机:
* 熔断限流: redis服务不再接受请求，直接返回错误。 对请求进行限流。
* 构建redis集群

##### 击穿

某个热点数据过期，对这个数据的全部请求都会访问到数据库，导致数据库宕机

缓存击穿 可以认为是 缓存雪崩的子集

可以用后台更新策略来进行解决

##### 穿透

是一种恶意攻击

请求的数据 既不在缓存中 也不在数据库中，导致大量请求都在越过缓存 请求数据库

* 拒绝非法请求
* 缓存空值或默认值
* 使用布隆过滤器

布隆过滤器： 初步判断数据时候在数据库中。
在写入数据时，将数据做N个hash进行标记。
过滤器认为不存在的数据 必然是不存在， 拒绝访问数据库。
过滤器认为存在的数据 可以存在 也 可以不存在，可以访问数据库。


#### 缓存一致性

Cache Aside(旁路缓存)




#### redis的持久化操作

AOF: 将执行的指令追加到一个日志文件中

RDB: 在某一时刻，将全部数据以二进制的方式写入硬盘


### juc

#### synchronized底层原理

Monitor 管程

重量级锁

jvm底层使用c++实现，用于管理对象锁。


```
WaitSet
EntryList
Owner
```

对象调用wait方法后,线程会到WaitSet(Waiting).

没抢到锁的线程,会到EntryList(BLOCKED).

##### 重量级锁
1. 阻塞和唤醒线程，操作系统会从用户态切换内核态，这个过程很耗时
2. 线程被阻塞后，需要保存状态，进行上下文切换

jdk1.6 引入 轻量级锁 和 偏向锁。 在没有竞争的情况下 使用轻量级锁 或 偏向锁

##### 轻量级锁

每次加锁 用cas操作加锁对象中对象头的两位

##### 偏向锁


每次加锁 用cas操作加锁对象中对象头的两位 
会记录线程id, 在同一个线程进行加锁时(锁重入)， 不会反复cas


#### java内存模型

工作内存(本地内存): 线程私有

主内存: 线程共享

#### cas

比较交换: 是一个原子操作, 调用native方法实现

应用在aqs和原子变量

cas操作用于实现乐观锁

#### volatile

保证可见行

阻止指令重排序


#### ConcurrentHashMap

HashMap: 数组+链表+红黑树

ConcurrentHashMap在发生冲突时，会锁住头节点(链表或者红黑树)



### design pattern

#### 单例模式

饿汉式

```java
class Singleton {
    public static final Singleton instance = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() {
        return instance;
    }
}
```

懒汉式
```java
class Singleton {
    public static volatile Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

```

#### 工厂模式

##### 简单工厂

##### 工厂

常用


##### 抽象工厂



#### 策略模式



### jvm

#### gc垃圾判断

1. 引用分析
2. 可达性分析

可达性分析: 与gcroot相连的类

gcroot:
1. java栈 栈帧中本地变量表中 的变量
2. 静态变量 静态常量
2. c栈中的变量

#### gc垃圾回收算法

##### 标记清楚


##### 标记整理


##### 复制



#### 类的加载


### 分布式理论

#### cap

1. 一致性 consistancy
2. 可用性 availability
3. 分区容错性 partition torerance


 