---
layout:     post         
title:      "Java"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

## JVM

#### 主要组成

* 类加载器（ClassLoader）
* 运行时数据区（Runtime Data Area）
* 执行引擎（Execution Engine）
* 本地库接口（Native Interface）

![jvm](/img/in-post/post-interview/Java-jvm.png)

#### [运行时数据区](https://www.cnblogs.com/vipstone/p/10263642.html)

通常所说的jvm组成指的是运行时数据区（Runtime Data Area）：

* 程序计数器（Program Counter Register）
* Java虚拟机栈（Java Virtual Machine Stacks）
* 本地方法栈（Native Method Stack）
* Java堆（Java Heap）
* 方法区（Methed Area）

#### 编译型

编译型的语言包括：C、C++、Delphi、Pascal、Fortran

解释型的语言包括：Java、Basic、javascript

有人说Java是编译型的。因为所有的Java代码都是要编译的，.java不经过编译就无法执行。

也有人说Java是解释型的。因为java代码编译后不能直接运行，它是解释运行在JVM上的，所以它是解释型的。

## [面向对象](https://www.cnblogs.com/hnrainll/archive/2012/09/18/2690846.html)

任何具有状态和行为的实体都称为对象。对象的集合称为类，它是一个逻辑实体。

**面向对象是将事物高度抽象化。**

**面向过程是一种自顶向下的编程。**

#### 三大特性

* 封装：把客观事物封装成抽象的类，类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏.
* 继承：某个类型的对象获得另一个类型的对象的属性的方法.
* 多态：一个类实例的相同方法在不同情形有不同表现形式.

#### 五大基本原则

* 单一职责原则（Single-Resposibility Principle）：一个类的功能要单一
* 开放封闭原则（Open-Closed principle）：对扩展是开放的，对更改是封闭的
* 里氏替换原则（Liskov-Substituion Principle）：子类可以替换父类并且出现在父类能够出现的任何地方
* 依赖倒置原则（Dependecy-Inversion Principle）：传统的结构化编程中，最上层的模块通常都要依赖下面的子模块来实现，所以DIP原则就是要逆转这种依赖关系。(B抽象类,A实现)
* 接口隔离原则(Interface-Segregation Principle)：模块间要通过抽象接口隔离开，而不是通过具体的类强耦合起来

## 基本数据类型和复杂数据类型

java.lang包的八个类在java中称为包装类，包装类可用于实现多态性。

|int|Integer|
|--|--|
|基本类型，直接存数值|对象，用一个引用指向这个对象|
|初始化为0|初始化为null|

## 抽象类和接口

|abstract|interface|
|--|--|
|extends来继承抽象类，不支持多重继承，只能存在一个父类|implements来实现接口，支持多重继承，多个接口|
|抽象类不能实例化，抽象方法必须由子类重写，除非子类也是抽象类|不能被实例化，因为不是类|
|抽象类可以有抽象和非抽象方法|接口只能有抽象方法。从Java 8开始，它也可以有默认和静态方法。|
|可以有构造器|不能有构造器|
|有public、protected和default这些修饰符|默认修饰符是public,不可以使用其它修饰符|
|抽象方法比接口速度要快| 接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法|

## [重写和重载](https://www.runoob.com/java/java-override-overload.html)

方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现。

* 重载 : 同名函数，不同参数类型或个数，返回值类型可以相同也可以不相同。
* 重写/覆盖 : 父类与子类之间的多态性，对父类的函数进行重新定义。

## [堆栈](https://www.zhihu.com/question/29833675)

JVM的内存主要包括3大块：堆(heap)、栈(stack)（虚拟机栈和本地方法栈）、方法区(method)，以及程序计数器。

* 栈区: 每个线程包含一个栈区，栈中只保存方法中（不包括对象的成员变量）的基础数据类型和自定义对象的引用(不是对象)，对象都存放在堆区中每个栈中的数据(原始类型和对象引用)都是私有的，其他栈不能访问。
  
  * 每个Java线程拥有自己的独立的JVM栈，也就是Java方法的调用栈；
  * 每个Java线程拥有自己的独立的本地方法栈,用于支持native方法的执行，存储了每个native方法调用的状态。
  * 栈分为3个部分：基本类型变量区、执行环境上下文、操作指令区(存放操作指令)。

* 堆区: 存储的全部是对象实例，每个对象都包含一个与之对应的class的信息(class信息存放在方法区)。jvm只有一个堆区(heap)被所有线程共享，堆中不存放基本类型和对象引用，只存放对象本身，几乎所有的对象实例和数组都在堆中分配。
* 方法区: 又叫静态区，跟堆一样，被所有的线程共享。它用于存储已经被虚拟机加载的类信息、常量、静态变量、final类型、即时编译器编译后的代码等数据。

## [Garbage Collection](https://www.jianshu.com/p/5261a62e4d29)

**用于在空闲时间不定时回收无任何对象引用的对象占据的内存空间的一种机制。** 回收的是无任何引用的对象占据的内存空间。

* 引用：如果Reference类型的数据中存储的数值代表的是另外一块内存的起始地址，就称这块内存代表着一个引用。

        强引用（Strong Reference）：如“Object obj = new Object（）
        软引用、弱引用、虚引用

* 垃圾：无任何对象引用的对象
  
        引用计数算法，早期使用。
        根搜索算法，Java和C#中都是采用根搜索算法来判定对象是否存活的。

* 回收：清理“垃圾”占用的内存空间而非对象本身.
* 发生地点：一般发生在**堆内存 Heap Memory**中，因为大部分的对象都储存在堆内存中
* 发生时间：程序空闲时间不定时回收

#### 堆内存分配区域

* 年轻代

        几乎所有新生成的对象首先都是放在年轻代的。
        新生代内存按照8:1:1的比例分为一个Eden区和两个Survivor区。
            回收时,Eden->S0,清Eden;S0满->S1,清S0;S1->老.
        当新对象生成，Eden Space申请失败（因为空间不足等），则会发起一次GC。
* 年老代

        在年轻代中经历了N次垃圾回收后仍然存活的对象，就会被放到年老代中。
        年老代中存放的都是一些生命周期较长的对象。
        老年代内存满时触发Major GC即Full GC，Full GC发生频率比较低。
        一般大对象直接进入老年代。

* 持久代 Permanent Generation

        用于存放静态文件（class类、方法）和常量等。
        永久代空间在Java SE8特性中已经被移除。取而代之的是元空间（MetaSpace）。

#### 垃圾回收机制

* 新生代GC（Minor GC/Scavenge GC）: 非常频繁(不一定等Eden区满了才触发)，一般回收速度也比较快。在新生代中，每次垃圾收集时都会发现有大量对象死去，只有少量存活，因此可选用复制算法来完成收集。
* 老年代GC（Major GC/Full GC）: 不频繁，Major GC 经常会伴随至少一次Minor GC（对整个堆进行回收）。一般都是等待老年代满了后才进行Full GC，而且其速度一般会比Minor GC慢10倍以上。
* 导致 Full GC :
  * 年老代（Tenured）被写满;
  * 持久代（Perm）被写满;
  * System.gc()被显示调用;
  * 上一次GC之后Heap的各域分配策略动态变化.

#### 垃圾回收器(GC)

略

#### 垃圾回收函数

System.gc(), finalize()

#### 触发条件

* 当应用程序空闲时,即没有应用线程在运行时,GC会被调用。
* Java堆内存不足时,GC会被调用。
* 在编译过程中作为一种优化技术，Java 编译器能选择给实例赋 null 值。

## [内存泄漏](https://www.jianshu.com/p/54b5da7c6816)

内存泄漏是指无用对象（不再使用的对象）持续占有内存或无用对象的内存得不到及时释放，从而造成内存空间的浪费称为内存泄漏。

#### 根本原因

长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄漏，尽管短生命周期对象已经不再需要，但是因为长生命周期持有它的引用而导致不能被回收。

#### 常见发生场景

* 静态集合类HashMap、Vector 引起内存泄漏
* 监听器
* 各种连接：数据库、网络连接(socket)和io连接需要显示调用close()
* 内部类和外部模块的引用
* 单例模式

#### 内存溢出

*栈内存溢出* 场景：

栈是线程私有的，生命周期与线程相同，每个方法在执行的时候都会创建一个栈帧，用来存储局部变量表（基本数据类型，对象引用类型），操作数栈，动态链接，方法出口等信息。

如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出StackOverflowError异常，方法递归调用产生这种结果。

*OutOfMemory* 场景：

* 内存中加载的数据量过于庞大，如一次从数据库取出过多数据；
* 集合类中有对对象的引用，使用完后未清空，使得JVM不能回收；
* 启动参数内存值设定的过小；
  * 添加参数-Xms（初始内存）和-Xmx（最大能够使用内存大小）可以用来限制JVM的物理内存使用量

## [集合框架](https://www.runoob.com/java/java-collections.html)

Java 集合框架主要包括两种类型的容器，一种是集合（Collection），存储一个元素集合，另一种是图（Map），存储键/值对映射.

![Java-collection](/img/in-post/post-interview/Java-collection-framework.png)

#### Collection: 最基本的集合接口

* Set: 无序,不能包含重复元素
  * HashSet: 使用HASH算法来存储集合中的元素，因此具有良好的存取和查找性能。
  * SortedSet: 此接口主要用于排序操作，即实现此接口的子类都属于排序的子类。
    * TreeSet: SortedSet接口的实现类，TreeSet可以确保集合元素处于排序状态。
  * EnumSet: 所有元素都必须是指定枚举类型的枚举值，该枚举类型在创建EnumSet时显式、或隐式地指定。EnumSet的集合元素也是有序的。
* List: 代表一个元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。
  * LinkedList: 非同步的（unsynchronized）
    * LinkedList是基于链表的数据结构，
    * 对于需要快速插入，删除元素，应该使用LinkedList。
    * insert方法在 LinkedList的首部或尾部，可被用作堆栈，队列，双向队列。
  * ArrayList: 可变大小的数组，非同步的（unsynchronized）
    * ArrayList是实现了基于动态数组的数据结构，
    * 如果需要快速随机访问元素，应该使用ArrayList。
  * Vector: 类似ArrayList，但Vector是同步的
  * Stack: 后进先出的堆栈，有push,pop,peek等方法。
* Queue: 模拟"队列"这种数据结构（先进先出 FIFO）。
  * LinkedList类实现了Queue接口，因此我们可以把LinkedList当成Queue来用。
  * remove、element、offer 、poll、peek 其实是属于Queue接口。
  * add、remove（移除并返回头部元素）、element（返回头部元素），失败会抛异常。
  * offer、poll、peek，对应情况失败会返回false、null、null

#### Map: 保存具有"映射关系"的数据

* Hashtable: 继承Map接口，实现一个key-value映射的哈希表。任何非空（non-null）的对象都可作为key或者value，HashTable是同步的。
* HashMap: 不能保证元素的顺序一样，HashMap是非同步的，并且允许null，即null value和null key
  * LinkedHashMap也使用双向链表来维护key-value对的次序
  * 容量(Capacity)，即HashMap中数组的大小; 
  * 负载因子(load factor) = 实际键值对个数 / hashmap中数组的大小
* SortedMap
  * TreeMap就是一个红黑树数据结构
* WeakHashMap: 类似HashMap，对key实行“弱引用”，如果一个key不再被外部所引用，那么该key可以被GC回收。
* IdentityHashMap
* EnumMap

**[HashMap vs HashTable vs ConCurrentHashMap 对比](https://juejin.im/post/5add97a46fb9a07aa212f4c0)**

**[HashMap扩容机制](https://www.cnblogs.com/yanzige/p/8392142.html)**

**[HashMap的put()方法](https://blog.csdn.net/thetimelyrain/article/details/99873881)**

#### 时间复杂度

||访问指定元素|查找|插入|删除|
|--|--|--|--|--|
|数组|O(1)|O(n)|O(n)|O(n)|
|链表|O(n)|O(n)|O(1)|O(1)|

HashMap 理想情况下查找的时间复杂度为 O(1)：

1. 判断key，根据key算出索引。
2. 根据索引获得索引位置所对应的键值对链表。
3. 遍历键值对链表，根据key找到对应的Entry键值对。
4. 拿到value。

## [Java 内部类](https://juejin.im/post/5a903ef96fb9a063435ef0c8)

在《Think in java》中有这样一句话 ：**使用内部类最吸引人的原因是：每个内部类都能独立地继承一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响。**

**内部类 ( inner class ) : 定义在另一个类中的类**

* 内部类方法可以访问该类定义所在作用域中的数据，包括被 private 修饰的私有数据
* 内部类可以对同一包中的其他类隐藏起来

        内部类 对象名 = 外部类对象.new 内部类( );

* 内部类可以实现 java 单继承的缺陷
* 通过匿名内部类来"优化"简单的接口实现

**[内部类的种类](https://juejin.im/post/5b7e4a04e51d4538b063eee3)**

* 成员内部类: 作为外部类的一个成员存在，与外部类的属性、方法并列。

        内部类是一个编译时的概念，一旦编译成功，就会成为完全不同的两类
* 局部内部类: 方法中定义的内部类称为局部内部类。
  
        与局部变量类似，局部内部类不能有访问说明符，因为它不是外围类的一部分，
        但可以访问当前代码块内的常量，和此外围类所有的成员。
* 静态内部类: 通常称为嵌套类。

        要创建嵌套类的对象，并不需要其外围类的对象。
        不能从嵌套类的对象中访问非静态的外围类对象。
* 匿名内部类: 匿名内部类就是没有名字的内部类。

**内部类与外部类的关系**

* 对于非静态内部类，内部类的创建依赖外部类的实例对象，在没有外部类实例之前是无法创建内部类的
* 内部类是一个相对独立的实体，与外部类不是is-a关系(继承关系)
* 创建内部类的时刻并不依赖于外部类的创建

## [关键字](http://wiki.jikexueyuan.com/project/java-interview-bible/keyword.html)

***Static***

* 表明一个成员变量或者是成员方法可以在没有所属的类的实例变量的情况下被访问。
* **static方法** 称作静态方法，由于静态方法不依赖于任何对象就可以进行访问
* **static变量** 也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。
* **static代码块** 可以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且**只会执行一次**。

***Final***

* 修饰类：表示该类不能被继承；
  * 父类的private方法会隐式地被指定为final方法。
* 修饰方法：表示方法不能被重写；
* 修饰变量：表示变量只能一次赋值以后值不能被修改（常量）。

***Volatile***

* 不能保证线程安全

***[transient](https://www.cnblogs.com/chenpi/p/6185773.html)***

* Java 变量修饰符，如果用transient声明一个实例变量，当对象存储时，它的值不需要维持。transient关键字标记的成员变量不参与序列化过程。

***assertion***

* (断言)在软件开发中是一种常用的调试方式

## [设计模式](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)

* 工厂模式：定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。
* 单例模式：该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
* Model-View-Controller（模型-视图-控制器）模式：这种模式用于应用程序的分层开发。
* ...

#### 单例模式

单例模式的意思就是只有一个实例。单例模式确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。

两种实现：

* 懒汉模式（类加载时不初始化）

```java
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

```java
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

#### 工厂模式

工厂方法模式是简单工厂的仅一步深化， 在工厂方法模式中，我们不再提供一个统一的工厂类来创建所有的对象，而是针对不同的对象提供不同的工厂。也就是说每个对象都有一个与之对应的工厂。

定义：定义一个用于创建对象的接口，让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类。
