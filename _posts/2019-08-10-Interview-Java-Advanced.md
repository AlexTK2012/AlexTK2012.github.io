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

#### 线程状态

* 新建（New）
* 可运行（Runnable）：Java线程中将就绪（ready）和运行中（running）统称为可运行
* 阻塞（Blocked）：线程阻塞于锁
* 无限期等待（Waiting）：进入该状态的线程需要等待其他线程做出一些特定动作（通知或中断）
* 限期等待（Timed Waiting）：指定的时间后自行返回
* 死亡（Terminated)
![thread](/img/in-post/post-interview/Java-thread-state.png)

#### 创建线程

***继承 Thread 类***

* 重写该类的run()方法。该方法为线程执行体。
* 创建Thread子类的实例。即线程对象。
* 调用线程对象的start()方法启动该线程

```Java
public static class MyThread extends Thread {
    public void run() {
        // ...
    }
}
new MyThread().start();
```

***实现 Runnable 接口***

* 重写该接口的run()方法。该方法为线程执行体。
* 创建Runnable实现类的实例。并以此实例作为Thread的target来创建Thread对象。该Thread对象才是真正的线程对象。
* 调用线程对象（该Thread对象）的start()方法启动该线程。

```Java
public class MyRunnable implements Runnable {
    public void run() {
        // ...
    }
}
new Thread(new MyRunnable(),"thread1").start();

new Thread(()->{...},"thread2").start();
```

***实现 Callable 接口***

* 与 Runnable 相比，Callable 可以有返回值，返回值通过 FutureTask 进行封装。
* 通过Future 对象的 get() 获取到Callable任务返回的Object了。get方法是阻塞的，即：线程无返回结果，get方法会一直等待。

```Java
public class MyCallable implements Callable<Integer> {
    public Integer call() {
        return 123;
    }
}
FutureTask<Integer> ft = new FutureTask<>(new MyCallable());

FutureTask<Integer> ft = new FutureTask<>(() -> {
    Thread.sleep(100);
    return 1;
});
new Thread(ft).start();
```

建议 **实现接口 > 继承 Thread**：

* Java 不支持多重继承，因此继承了 Thread 类就无法继承其它类，但是可以实现多个接口；
* 类可能只要求可执行就行，继承整个 Thread 类开销过大。

***线程池 Executors类***

Executor：提供了一系列工厂方法用于创建线程池，返回的线程池都实现了ExecutorService接口。

* newCachedThreadPool()：创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
  * 线程池为无限大，当执行第二个任务时第一个任务已经完成，会复用执行第一个任务的线程，而不用每次新建线程。
* newFixedThreadPool(int nThreads)：创建定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
* newSingleThreadExecutor()：创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
* newScheduledThreadPool()：创建一个支持定时及周期性的任务执行的线程池，多数情况下可用来替代Timer类。

```Java
ExecutorService threadPool = Executors.newFixedThreadPool(10);
threadPool.execute(()->{});
```

线程池作用：

* 减少了创建和销毁线程的次数，每个工作线程都可以被重复利用，可执行多个任务。
* 调整线程池中工作线线程的数目，防止因为消耗过多的内存。

核心参数：

corePoolSize：核心线程数，线程池启动时就会创建的线程数量。即使核心线程是空闲的，也不会被回收，除非调用了allowsCoreThreadTimeOut方法为true。

maximumPoolSize：最大线程数，线程池中最大的线程数量。

keepAliveTime：线程超时时间，看源码可知，该参数的意义是线程从工作队列中取出任务的超时时间。

unit：超时时间的单位。

workQueue：工作队列。

threadFactory：线程工厂，要实现ThreadFactory接口，线程池创建线程时会调用ThreadFactory的newThread方法创建线程。

RejectedExecutionHandler：饱和策略。

#### 守护进程

Java中有两类线程：User Thread(用户线程)、Daemon Thread(守护线程) 。

* 当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。
* 线程启动之前使用 setDaemon() 方法可以将一个线程设置为守护线程。
* 在守护线程中产生的新线程也是守护线程。

#### 线程切换

***join***
在线程A 中调用另一个线程B 的 join() 方法，会将当前线程A 挂起，而不是忙等待，直到目标线程B 结束。

***wait & notify & notifyAll***
调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。

* 只能用在**同步方法或者同步控制块**中使用。
* notify()或者notifyAll()方法并不是真正释放锁，必须等到synchronized方法或者语法块执行完才真正释放锁；
* 调用notifyAll()方法能够唤醒所有正在等待这个对象的monitor的线程，唤醒的线程获得锁的概率是随机的，取决于cpu调度；
* **wait 和 sleep 区别**：
  * wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
  * wait() 会释放锁，sleep() 不会；

***await & signal & signalAll***
java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

* 相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。
* 使用 Lock 来获取一个 Condition 对象。

#### 线程间通信

* 同步：多个线程通过synchronized关键字这种方式来实现线程间的通信。
  * 本质上就是“共享内存”式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。
* while轮询的方式
  * 注意volatile关键字的可见性
* wait/notify机制
* 管道通信
  * 使用java.io.PipedInputStream 和 java.io.PipedOutputStream进行通信

#### 监视器和锁

#### 互斥与同步

synchronized
ReentrantLock

#### volatile 与 synchronized

执行控制的目的是控制代码执行（顺序）及是否可以并发执行。

内存可见控制的是线程执行结果在内存中对其它线程的可见性。根据Java内存模型的实现，线程在具体执行时，会先拷贝主存数据到线程本地（CPU缓存），操作完成后再把结果从线程本地刷到主存。

区别：

* volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
* volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
* volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性
* volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
* volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化。

#### [Blocking Queue](https://www.jianshu.com/p/b1408e3e3bb4)

java.util.concurrent 包中的Java BlockingQueue 接口表示一个线程安全的队列。

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

## [Error与异常](https://blog.csdn.net/iblade/article/details/78196016)

Throwable： 有两个重要的子类：Exception（异常）和 Error（错误），二者都是 Java 异常处理的重要子类，各自都包含大量子类。异常和错误的区别是：异常能被程序本身可以处理，错误是无法处理。

Error（错误）:是程序无法处理的错误，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。比如：OutOfMemoryError, StackOverFlowError.

Exception（异常）:是程序本身可以处理的异常。包括运行时异常和非运行时异常(编译异常)。

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
