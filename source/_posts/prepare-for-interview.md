---
title: prepare for interview
date: 2024-05-23 16:14:57
tags:
---

### java 八股

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

#### jmm

线程私有: 程序计数器， java栈，c栈
线程共享: 方法区、java堆

程序计数器: 存储下一条需要执行的字节码指令，如果native方法，计数器为undefined

java栈(java虚拟机栈): 每执行一个方法，创建一个栈帧，方法执行完栈帧自动销毁，栈帧中的变量不需要gc来进行释放

栈帧: 局部变量表(slot为单位)，操作数栈，动态链接，方法返回地址

本地方法栈: native方法的方法栈

java堆: 存储对象和数组，gc进行管理，分为新生代，老年代，永久代。逻辑地址是连续的，物理地址不一定。(intern字符串缓存 和 静态变量 放在java堆中)

方法区: 存储类的基础信息，方法区放在元空间中，元空间放在本地内存中

本地内存(native memory): 不由jvm管理，由操作系统，里面有元空间 和 直接内存

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