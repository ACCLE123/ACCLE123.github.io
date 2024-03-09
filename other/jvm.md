### jvm

[极简指南](https://doocs.github.io/jvm/10-class-loader.html)
[黑马jvm](https://www.bilibili.com/video/BV1r94y1b7eS?p=17&vd_source=4c93ee74c1ac86465e2c42663f6c6c16)
剑指jvm(书)
深入理解java虚拟机(书)



#### 类的装载 

java中的数据类型: 基础数据类型 引用数据类型

> 基础数据类型可以直接使用 引用数据类型需要执行 类的加载 才可以使用

类的装载: 将.class 字节码文件 加载到java内存中(虚拟内存)


类的装载分为:
- 加载(loading)
- 链接(linking)
- 初始化(initialize)

**loading**: 
这个阶段通过类加载器ClassLoader实现

1. 读取.class字节码文件 
2. 在方法区创建对应类的数据结构 InstanceKlass(constant pool、虚方法表)
3. 在堆区创建.class对象指向方法区的结构 （Class a = A.class 反射的基础、静态变量）

**linking**: 

1. verification


2. perparation
为静态变量初始化为0


3. resolution
将符号引用(constant pool中)变为直接引用

**initialize**:
执行clinit方法： 这个方法由 静态赋值语句 和 静态语句块 合并

##### ClassLoader

和classloader相关的技术

- spi机制
- 类的热部署
- tomcat类的隔离
- 面试： Delegation Model & Custom ClassLoader
- arthas的底层

类加载器的分类
1.bootstrap class loader : 最底层的classloader(c++编写)
2. extension class loader 
3. application class loader 加载classpath下的类文件

Delegation Model:
类加载器收到加载请求时 委派给 父类的类加载器进行加载
直到 bootstrap class loader

这样可以防止java自己的类被覆盖

`ClassLoader`类 几个重要的函数

```
loadClass: 实现类加载的逻辑 Delegation Model
findClass: 将.class字节码文件读入
defineClass: native interface 将字节码文件加载到 堆区 和 方法区
resolveClass: linking阶段
```

重写loadClass方法 打破算清委派机制
冲写findClass方法 实现从不同的地方加载类

**黑马老师讲了一个特别有意思的点子： 从数据库中加载类**


