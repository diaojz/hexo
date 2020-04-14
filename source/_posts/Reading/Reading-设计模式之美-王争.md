---
title: 设计模式之美
date: 2019-12-3 23:39:54
tags: Reading
categories: Reading
---

> 记录学习`设计模式之美`专栏重要知识点。

# 1. 业务开发基于`贫血模式` MVC违背OOP吗？
## 总结：
1. OOP四大特性：封装、继承、多态、抽象
2. 接口
3. 抽象类
4. 面向过程风格
5. 基于接口实现编程
6. 多用组合少用继承

## MVC（Model View Controller）
基于贫血模式， 被称为`反模式` 
展示层、逻辑层、数据层

### Web项目后端对应的设计模式（基于数据库开发）
1.Respository层： 负责数据访问
2.Service层：     负责业务逻辑
3.Controller层：  负责暴露接口


<!--more-->

## 领域驱动设计（DDD）Domain Driven Design
基于充血模式
2004年被提出，微服务加快了认知

## 贫血模式（Anemic Domain Model）
只包含数据，不包含业务逻辑的类，
基于SQL的CRUD操作。
平日一般都是基于SQL开发，先看接口需要查询什么数据，写完SQL查到数据，完善一下调用接口即可

### 受欢迎原因:
    1.大多时候，业务简单，用不着
    2.充血模式有难度，需要更多设计
    3.思维固化，转型有成本

## 充血模式（Rich Domain Model）
数据和业务封装到同一个类中。典型的面向对象风格

### 什么时候使用
业务复杂的项目。业务调研，领域模型设计。 


# 13.如何对接口鉴权这样一个功能开发做面向对象分析？(OOA)
## 面向对象分析（OOA）
产出：详细的需求描述

# 14.如何利用面向对象设计和编程开发接口鉴权功能？(OOD、OOP)
## 面向对象设计（OOD）
产出：类
1. 划分职责进⽽识别出有哪些类；
    1. 罗列功能点，找相同属性
    2. 单一职责
    3. 模块划分，可缩小范围
2. 定义类及其属性和⽅法；
    1. 动词：方法
    2. 名词：属性
3. 定义类与类之间的交互关系；
    1. 泛化、实现、关联、聚合、组合、依赖 （UML中定义为6种）
    2. 做简化，保留4种。  泛化（继承）、实现、依赖、组合（组合、聚合、关联）
4. 将类组装起来并提供执⾏⼊⼝。
    1. 统一封装，对外提供入口

### 泛化（继承关系） extends
```java
public class A { ... }
public class B extends A { ... }
```
### 实现（接口和实现关系）implements
```java
public interface A {...}
public class B implements A { ... }
```
### 依赖（只要 B 类对象和 A 类对象有任何使用关系，我们都称它们有依赖关系）
```java
public class A {
  private B b;
  public A(B b) {
    this.b = b;
  }
}
//或者
public class A {
  private B b;
  public A() {
    this.b = new B();
  }
}
//或者
public class A {
  public void func(B b) { ... }
}
```
### 组合（组合、聚合、关联）
```java
//组合，包含关系(鸟和翅膀)
public class A {
  private B b;
  public A() {
    this.b = new B();
  }
}

//聚合，包含关系（课程和学生）
public class A {
  private B b;
  public A(B b) {
    this.b = b;
  }
}

//关联，包含以上2种
。。。
。。。
```

## 面向对象编程（OOP）

# 15.单一职责原则（SRP）
> Single Responsibility Principle
> A class or module should have a single reponsibility.
> 一个类或模块只负责完成一个职责（功能）。

不能脱离业务空谈。可以先做粒度粗的类，后面发展起来之后，再拆分更细的粒度。`持续重构`
## 技巧
1. 行数过多，影响可读和可维护
2. 依赖过多其他类，不符合高内聚，低耦合
3. 私有方法过多，应该独立到新类中，提供public方法，提高复用性
4. 很难起名字，说明职责不清晰
5. 集中在几个属性或方法中。例：在`UserInfo`中多数是操作`address`的话。

# 16.对扩展开放、修改关闭（OCP）
> Open Closed Principle 
> sofeware entities(modules,classes,functions,etc) should be open for extension, but closed for modification.
> 软件实体（模块、类、方法等）应该“对扩展开放，对修改关闭”

> 多数设计模式目标都是： 解决扩展性
> 比如如下几种：
1. 多态
2. 依赖注入
3. 基于接口而非实现编程
4. 装饰、策略、模板、职责链、状态

# 单继承、多实现
* interface   接口 
* abstract    抽象类
* extends     继承
* implements  实现

# 17.里氏替换（LSP）跟多态有哪些区别？ 哪些代码违背的LSP？
> Liskov Substitution Principle
> 初衷：使用子类完美继承父类，并做了增强

## 区别
里式替换：设计原则
多态：是面向对象语法，是思路

## 违背LSP原则
> 原理：按照协议来设计  Design By Contract
子类和父类的关系：更像 接口和实现的关系。

1. 子类违背父类  声明要实现的功能
2. 子类违背父类  对输入、输出、异常的约定
3. 子类违背父类  注释所罗列的任何特殊说明

> 检验方法： 拿父类的单元测试区验证子类，有不通过的，则说明有违背的地方

# 接口隔离原则（ISP）
> Interface Segregation Principle

## 接口的类型：
1. 一组接口集合
    1. 如果只被部分使用，就应该隔离
    2. 不强迫调用者依赖不用的接口
2. 单个API或函数
    1. 只需要部分功能，拆分更细粒度的函数
3. OOP中的接口
    1. 接口尽量单一，不要让`实现类`或`调用者`依赖不需要的接口
    2. 拆分为多个`协议`

## 与单一职责的区别
1. 侧重
    1. 单一职责侧重：模块、类、接口设计
    2. 接口隔离侧重：接口设计
2. 角度
    1. 接口隔离提供间接判断单一职责标准
    2. 如果调用者只使用了部分功能，那接口的设计就不够职责单一

# 19.控制反转（IOC）、依赖反转、依赖注入，这三者区别和联系
> 控制反转，是比较笼统的设计思想。
> Inversion Of Control

例子：模板设计模式、依赖注入。

## 依赖注入（DI）
> 是一种具体编码技巧。
> Dependency Injection

> 不通过`New()`的方式在类`内部`创建依赖类对象，
> 而是将依赖的类对象在`外部`创建好之后，
> 通过`构造函数`、`函数参数`等方式传递或注入给类使用。

## 依赖注入框架（DI Framework）
> 把对象的创建、组装（注入）等工作，交给框架
> Java Spring框架

## 依赖反转原则（DIP）
> Dependency Inversion Principle
> 依赖倒置原则

> High-level modules should't depend on low-level modules. 
> Both modules should depend on abstractions.
> In addition, abstractions should't depend on details.
> Detail depend on abstractions.

> 高层模块不依赖低层模块
> 所有模块通过抽象相互依赖
> 除此之外，抽象不要依赖实现
> 实现依赖抽象


# 20. KISS、 YAGNI原则
> Keep it simple and stupid
> Keep it short and simple
> Keep it simple and straightforward
> 
> 尽量保持简单

1. 不要使用同时看不懂的技术实现
2. 不同重复造轮子
3. 不要过度优化

## YAGNI原则
> You ain't gonna need it
> 你不会需要它

> 不要设计当前用不到的功能，当前如果用不到就不要做。
> 但是该预留得预留。可扩展


# 21. 提高代码复用性，DRY原则
> Don't repeat yourself
> 不要重复自己

## DRY原则
1. 实现逻辑重复
2. 功能语义重复
3. 代码执行重复

## 代码复用性
1. 减少代码耦合
2. 满足单一职责
3. 模块化
4. 业务和非业务逻辑分离
5. 通用代码下沉
6. 继承、多态、抽象、封装
7. 应用模板等设计模式

# 22.迪米特法则（LOD）实现高内聚，低耦合

## 高内聚
> 指导类本身的设计
> 功能相近的应该放到同一个类中
> 不相关的功能，不放到同一类中

## 松耦合
> 指导类与类之间的设计
> 类和类之间的依赖关系清晰简单
> 一个类的修改，另一个应该不用或很少改动

1. 依赖注入
2. 接口隔离
3. 基于接口而非实现编程
4. 迪米特法则

## 迪米特法则（LOD）
> Law of Demeter
>
> 另一种叫法
> The least knowledge principle
> 最小知识原则

> Eash unit should have only limited knowledge about other units:
> only units "closely" related to the current unit.
> Or: Each unit should only talk to its friends;
> Don't talk to strangers.
>
> 每个模块应该只了解和它关系密切的模块
> 只和自己的朋友说话，不和陌生人说话

1. 不该有直接依赖关系的类之间，不要有依赖
2. 有依赖关系的类之间，尽量只依赖必要的接口（有限知识）

## 思考
单一职责：自身出发
迪米特法则：关系出发
基于接口编程：使用者角度

