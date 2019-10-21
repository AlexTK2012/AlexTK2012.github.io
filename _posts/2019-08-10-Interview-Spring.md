---
layout:     post         
title:      "Spring"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

[Spring 69道问题](https://www.cnblogs.com/zjfjava/p/6069707.html#top)

## Spring依赖注入

三种IOC方式:

**属性注入**
通过setXXX()方法注入Bean的属性值或者依赖对象，最常用。

* Spring首先会调用bean的默认构造函数实例化bean对象，然后再通过反射的方法来调用set方法来注入属性值。
* 属性注入要求bean提供一个默认的构造函数,并且得为需要注入的属性提供set方法

**构造函数注入**
使用构造函数注入的前提是：bean必须提供带参的构造函数。

**工厂方法注入**
注解的方式注入 @Autowired,@Resource,@Required。

## [Spring中的IOC和AOP](https://juejin.im/post/5a1cd072f265da432240ef18)

IOC: 控制反转(Inversion of Control)也叫依赖注入(Dependecy Injection)

* Spring IOC 负责创建对象，管理对象 通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期
* 优点
  * IOC 或 依赖注入把应用的代码量降到最低。
  * 它使应用容易测试，单元测试不再需要单例和JNDI查找机制。
  * 最小的代价和最小的侵入性使松散耦合得以实现。
  * IOC容器支持加载服务时的饿汉式初始化和懒加载。
* 注入方式: 构造器依赖注入 & Setter方法注入

AOP: 面向切面编程。（Aspect-Oriented Programming）

* AOP可以说是对OOP的补充和完善。将程序中的交叉业务逻辑（比如安全，日志，事务等），封装成一个切面，然后注入到目标对象（具体业务逻辑）中去。

例如 *日志代码往往水平地散布在所有对象层次中，而与它所散布到的对象的核心功能毫无关系。在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。*

* 实现AOP的技术，主要分为两大类：
  * 一是采用动态代理技术，利用截取消息的方式，对该消息进行装饰，以取代原有对象行为的执行；
  * 二是采用静态织入的方式，引入特定的语法创建“方面”，从而使得编译器可以在编译期间织入有关“方面”的代码.

[参考](https://blog.csdn.net/baidu_20876831/article/details/78956220)

## [Spring MVC 工作流程](https://blog.csdn.net/qq_36761831/article/details/89053314)

## Spring Security 鉴权机制

