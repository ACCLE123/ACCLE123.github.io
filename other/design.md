### design pattern

一开始觉得设计模式其实很多此一举
写了一点mini-spring真的受不了了 发现稍微系统的总结一下


#### 简单工厂模式
> 所有的工厂模式都强调一点：两个类A和B之间的关系应该仅仅是A创建B或是A使用B，而不能两种关系都有。《设计模式的艺术》

simple factory pattern == static factory pattern

factory: 提供静态方法create 用来提供产品

product: 抽象产品

concrete product:  具体产品

**有时候会将 factory和product放到一起**

工厂模式的优点:
1. 将创建和使用完全分开
2. 使一个类可以有多个构造函数


##### 工厂方法模式

factory method pattern = factory pattern = virtual constructor pattern = polymorphic factory pattern

将工厂进行抽象

factory: 抽象工厂

concrete factory: 具体工厂


为了简化使用流程，可以在factory中添加product使用的方法
