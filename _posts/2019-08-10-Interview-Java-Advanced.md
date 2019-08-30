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

## [Java 多线程](https://github.com/CyC2018/CS-Notes/blob/master/notes/Java%20并发.md)

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

#### 线程之间的协作

***join***
在线程中调用另一个线程的 join() 方法，会将当前线程挂起，而不是忙等待，直到目标线程结束。

***wait & notify & notifyAll***
调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。

* 只能用在同步方法或者同步控制块中使用。
* wait 和 sleep 区别：
  * wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
  * wait() 会释放锁，sleep() 不会。

***await & signal & signalAll***
java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

* 相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。
* 使用 Lock 来获取一个 Condition 对象。

#### volatile 与 synchronized

对象锁，就是就是synchronized 给某个对象 加锁
用了double check，实际可以不用 volatile，但为什么还要用：
线程都有monitor，计数器

#### 线程间通信

* 同步：多个线程通过synchronized关键字这种方式来实现线程间的通信。
  * 本质上就是“共享内存”式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。
* while轮询的方式
  * 注意volatile关键字的可见性
* wait/notify机制
* 管道通信
  * 使用java.io.PipedInputStream 和 java.io.PipedOutputStream进行通信

#### [Blocking Queue](https://www.jianshu.com/p/b1408e3e3bb4)

java.util.concurrent包中的Java BlockingQueue接口表示一个线程安全的队列。

**操作方法：**

||Throws Exception|特殊值|阻塞|超时|
|--|--|--|--|--|
|Insert|add(o)|offer(o)|put(o)|offer(o, timeout, timeunit)|
|Remove|remove(o)|poll()|take()|poll(timeout, timeunit)|
|Examine|element()|peek()|||

1. Throws Exception: 如果尝试的操作不可能立即发生，则抛出一个异常。
2. 特殊值：如果尝试的操作不能立即执行，则会返回一个特殊值（通常为true / false）。
3. 阻塞:如果尝试的操作不可能立即执行，那么该方法将阻塞。
4. 超时:如果尝试的操作不可能立即执行，则该方法调用将阻塞，但不会超过给定的超时。返回一个特殊值，告诉操作是否成功（通常为true / false）。

无法将null插入到BlockingQueue中。

**[BlockingQueue 接口的实现类](https://fangjian0423.github.io/2016/05/10/java-arrayblockingqueue-linkedblockingqueue-analysis/)：**
建议阅读JDK 源码，lock 用的 ReentrantLock.

* ArrayBlockingQueue：是一个有界的阻塞队列，其内部实现是将对象放到一个数组里。
* DelayQueue：对元素进行持有直到一个特定的延迟到期。
* LinkedBlockingQueue：内部以一个链式结构(链接节点)对其元素进行存储。
* PriorityBlockingQueue：类似于LinkedBlockingQueue，但其所含对象的排序不是FIFO，而是依据对象的自然排序顺序或者是构造函数所带的Comparator决定的顺序。
* SynchronousQueue：同步队列。同步队列没有任何容量，每个插入必须等待另一个线程移除。

#### [Double Check locking](https://www.cnblogs.com/tuhooo/p/7723126.html)

Double-checked Locking (DCL)用来在lazy initialisation 的单例模式中避免同步开销的一个方法。

```Java
多线程中是不安全的，判断instance是否为空以及新建一个实例都不是原子操作

public static Instance getInstance() {
    if(instance == null) {
        instance = new Instance();
    }
    return instance;
}

解决：
1. 用synchronize 给临界区加锁做同步处理（有比较大的性能损耗的）

public synchronized static Instance getInstance() {}


2. 双重检查锁定（double-checked locking):
可以减少加锁和对象初始化的过程，大大减少了synchronized带来的性能开销

private static Instance instance;
public static Instance getInstance() {
    if (instance == null) {
        synchronized (DoubleCheckLock.class) {
            if (instance == null)
                instance = new Instance();
        }
    }
    return instance;
}

3. 基于volatile的双重检查锁定的解决方案:

private volatile static Instance instance;

public static Instance getInstance() {
    if (instance == null) {
        synchronized (DoubleCheckLock.class) {
            if (instance == null)
                instance = new Instance();
        }
    }
    return instance;
}

4. 基于类初始化的解决方案
JVM在类的初始化阶段（即在Class被加载后，且被线程使用之前），会执行类的初始化。
在执行类的初始化期间，JVM会去获取一个锁，这个锁可以同步多个线程对同一个类的初始化。

public class InstanceFactory {
    private static class InstanceHolder {
        public static Instance instance = new Instance();
    }

    public static Instance getInstance() {
        return InstanceHolder.instance ;  //这里将导致InstanceHolder类被初始化
    }
}
```

## IO & NIO

## 并发 CAS

[CAS 原理剖析](https://juejin.im/post/5a73cbbff265da4e807783f5)
[并发编程之 CAS 的原理](https://juejin.im/post/5ae753d8f265da0ba56753ca)
[Java并发编程：volatile关键字解析](https://www.cnblogs.com/dolphin0520/p/3920373.html)


## 反射

通过一个对象获得完整的包名和类名
testReflect.getClass().getName()

Class<?> clazz = Class.forName("net.xsoftlab.baike.TestReflect");
Field[] field = clazz.getDeclaredFields();
Method method[] = clazz.getMethods();


## 单例模式

单例模式的意思就是只有一个实例。单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。

两种实现：

* 懒汉模式（类加载时不初始化）

```Java
public class LazySingleton {
    //懒汉式单例模式
    //比较懒，在类加载时，不创建实例，因此类加载速度快，但运行时获取对象的速度慢
    private static LazySingleton intance = null;//静态私用成员，没有初始化

    private LazySingleton()
    {
        //私有构造函数
    }

    public static synchronized LazySingleton getInstance()  //静态，同步，公开访问点
    {
        if(intance == null)
        {
            intance = new LazySingleton();
        }
        return intance;
    }
}
```

* 饿汉式单例模式（在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快）

```Java
public class EagerSingleton {
    //饿汉单例模式
    //在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快
    private static EagerSingleton instance = new EagerSingleton();//静态私有成员，已初始化

    private EagerSingleton()
    {
        //私有构造函数
    }

    public static EagerSingleton getInstance()  //静态，不用同步（类加载时已初始化，不会有多线程的问题）
    {
        return instance;
    }
}
```

## 工厂模式

工厂方法模式是简单工厂的仅一步深化， 在工厂方法模式中，我们不再提供一个统一的工厂类来创建所有的对象，而是针对不同的对象提供不同的工厂。也就是说每个对象都有一个与之对应的工厂。

定义：
　　定义一个用于创建对象的接口，让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类。
　　这次我们先用实例详细解释一下这个定义，最后在总结它的使用场景。

