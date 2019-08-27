---
layout:     post         
title:      "Java-高级"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

## [JDK1.8](https://www.jianshu.com/p/5b800057f2d8)

[主要新特性讨论](https://www.runoob.com/java/java8-new-features.html)

#### 语言新特性

* Lambda表达式和函数式接口，允许将函数作为参数，使用函数式编程的概念。
* 接口的默认方法和静态方法
* 方法引用
  * 构造器引用，语法是Class::new
  * 静态方法引用，语法是Class::static_method
  * 某个类的成员方法的引用，语法是Class::method
  * 某个实例对象的成员方法的引用，语法是instance::method
* 重复注解，@Repeatable注解定义重复注解
* 更好的类型推断
* 拓宽注解的应用场景:注解几乎可以使用在任何元素上，局部变量、接口类型、超类、接口实现类、函数的异常定义。

#### Java官方库的新特性

* Optional
* Streams
* Date/Time API(JSR 310)
* Nashorn JavaScript引擎: 可以在JVM上开发和运行JS应用
* Base64
* 并行数组
* 并发性: 为java.util.concurrent.ConcurrentHashMap类添加了新的方法来支持聚焦操作

## Java 多线程

![thread](/img/in-post/post-interview/Java-thread-state.png)

#### 创建线程

实现 Runnable 接口

* 重写该接口的run()方法。该方法为线程执行体。
* 创建Runnable实现类的实例。并以此实例作为Thread的target来创建Thread对象。该Thread对象才是真正的线程对象。
* 调用线程对象（该Thread对象）的start()方法启动该线程。

实现 Callable 接口

* 与 Runnable 相比，Callable 可以有返回值，返回值通过 FutureTask 进行封装。

继承 Thread 类

* 重写该类的run()方法。该方法为线程执行体。
* 创建Thread子类的实例。即线程对象。
* 调用线程对象的start()方法启动该线程

建议 **实现接口 > 继承 Thread**：

* Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；
* 类可能只要求可执行就行，继承整个 Thread 类开销过大。

PS：start() 和 run() 方法区别在，start()方法被用来启动新创建的线程，start()内部调用了run()方法。

#### 控制线程

* join线程：join方法用线程对象调用，如果在一个线程A中调用另一个线程B的join方法，线程A将会等待线程B执行完毕后再执行。
  
#### 基础线程机制

Executor
Daemon
sleep()
yield()

#### 守护进程

http://wiki.jikexueyuan.com/project/java-interview-bible/multi-thread.html

https://www.nowcoder.com/discuss/214906?type=2

#### 互斥与同步

synchronized
ReentrantLock

#### volatile 与 synchronized

对象锁，就是就是synchronized 给某个对象 加锁
用了double check，实际可以不用 volatile，但为什么还要用：
线程都有monitor，计数器

实现 block queue，线程安全的


#### 线程间通信

* 同步：多个线程通过synchronized关键字这种方式来实现线程间的通信。
  * 本质上就是“共享内存”式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。
* while轮询的方式
  * 注意volatile关键字的可见性
* wait/notify机制
* 管道通信
  * 使用java.io.PipedInputStream 和 java.io.PipedOutputStream进行通信

## IO & NIO

## 并发 CAS

[CAS 原理剖析](https://juejin.im/post/5a73cbbff265da4e807783f5)
[并发编程之 CAS 的原理](https://juejin.im/post/5ae753d8f265da0ba56753ca)
[Java并发编程：volatile关键字解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)


## 反射


## 单例模式

