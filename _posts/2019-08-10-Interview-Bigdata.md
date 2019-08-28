---
layout:     post         
title:      "Big Data"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

## HDFS

#### HDFS HA

HDFS NameNode 和 YARN ResourceManger 的高可用方案类似，HDFS NameNode 对于数据存储和数据一致性的要求比 YARN ResourceManger 高得多，所以 HDFS NameNode 的高可用实现更为复杂一些。

***NameNode 高可用整体架构***
![HA](/img/in-post/post-interview/HDFS-HA.png)

NameNode 主备切换主要由 ZKFailoverController、HealthMonitor 和 ActiveStandbyElector 这 3 个组件来协同实现.

***主备切换流程***
![switch](/img/in-post/post-interview/HDSF-HA-switch.png)

## Yarn

#### Yarn 二次调度

[yarn资源调度流程](https://zhuanlan.zhihu.com/p/41151457)

Yarn 采用 Master/Slave 结构，整体采用双层调度架构。

* 在第一层的调度是 ResourceManager 和 NodeManager: ResourceManager 是 Master 节点，相当于 JobTracker，包含 Scheduler 和App Manager 两个组件，分管资源调度和应用管理；NodeManager 是 Slave 节点，可以部署在独立的机器上，用于管理机器上的资源。NodeManager 会向 ResourceManager 报告它的资源数量、使用情况，并接受 ResourceManager 的资源调度。
* 第二层的调度指的是 NodeManager 和 Container。NodeManager 会将 Cpu&内存等资源抽象成一个个的 Container，并管理它们的生命周期。
  
通过采用双层调度结构将 Scheduler 管理的资源由细粒度的 Cpu&内存变成了粗粒度的 Container，降低了负载。在 App Manager 组件中也只需要管理 App Master，不需要管理任务调度执行的完整信息，同样降低了负载。通过降低 ResourceManager 的负载，变相地提高了集群的扩展性。

#### 资源调度和任务调度的流程

* 粗粒度资源申请(Spark）: 在Application执行之前，将所有的资源申请完毕，当资源申请成功后，才会进行任务的调度，当所有的task执行完成后，才会释放这部分资源。
* 细粒度资源申请（MapReduce）: Application执行之前不需要先去申请资源，而是直接执行，让job中的每一个task在执行前自己去申请资源，task执行完成就释放资源。

## Spark

#### Basic

* RDD(Resilient,Distributed,Dataset) 不可变的分布式对象集合
* DAG 有向无环图: 描述了RDD的依赖关系，这种关系也被称之为lineage，RDD的依赖关系使用Dependency维护。

  ![DAG](/img/in-post/post-interview/Spark-DAG.png)
* 宽依赖、窄依赖，DAGScheduler根据 wide dependency划分Stage
* RDDs 算子: Transformations and Actions, **Spark is lazy**
* Transformations:
  * Narrow Transformation: map, union, flatmap, filter...
  * Wide Transformation: join, reduceByKey, groupByKey, sortByKey...
* Action: reduce, count, countByKey, collect, first, take, foreach, OutputFunction like saveAsTextFile...

#### RDD,Dataset,Dataframe

***RDD***

弹性分布式数据集 Resilient Distributed Dataset 是分布式的Java对象的集合。

* 优点：
  1. 强大，内置很多函数操作，group，map，filter等，方便处理结构化或非结构化数据；
  2. 面向对象编程，直接存储的java对象，类型转化也安全；

* 缺点：
  1. 由于它基本和hadoop一样万能的，因此没有针对特殊场景的优化，比如对于结构化数据处理相对于sql来比非常麻烦；
  2. 默认采用的是java序列号方式，序列化结果比较大，而且数据存储在java堆内存中，导致gc比较频繁；

***DataFrame***

DataFrame是分布式的Row对象的集合。

* 优点：
  1. 结构化数据处理非常方便，支持Avro, CSV, Elastic search, Cassandra等kv数据，也支持HIVE tables, MySQL等传统数据表；
  2. 有针对性的优化，由于数据结构元信息spark已经保存，序列化时不需要带上元信息，减少了序列化大小，而且数据保存在堆外内存中，减少了gc次数；
  3. hive兼容，支持hql，udf等；
* 缺点：
  1. 编译时不能类型转化安全检查，运行时才能确定是否有问题
  2. 对于对象支持不友好，rdd内部数据直接以java对象存储，dataframe内存存储的是row对象而不能是自定义对象

***Dataset***

* DataFrame = Dataset[Row], Dataset每一个record存储的是一个强类型值而不是一个Row。
* DataFrame和Dataset API都是基于Spark SQL引擎构建的

#### event loop

#### distinct 原理

reduceByKey

#### 并行度设置

RDD：spark.default.parallelism
DataFrame: 
RDD.repartition：给RDD重新设置partition的数量 [repartitions 或者 coalesce]

#### 数据倾斜

**原理**: 在进行shuffle的时候，必须将各个节点上相同的key拉取到某个节点上的一个task来进行处理，此时如果某个key对应的数据量特别大的话，就会发生数据倾斜

**定位**: 通过Spark Web UI来查看当前运行的stage各个task分配的数据量，从而进一步确定是不是task分配的数据不均匀导致了数据倾斜。知道数据倾斜发生在哪一个stage之后，接着我们就需要根据stage划分原理，推算出来发生倾斜的那个stage对应代码中的哪一部分，这部分代码中肯定会有一个shuffle类算子。通过countByKey查看各个key的分布。

**解决**:

* 过滤少数导致倾斜的key
* 提高shuffle操作的并行度
* 局部聚合和全局聚合

## Flink

RDDs vs DataFrames and Datasets

#### [反压](http://wuchong.me/blog/2016/04/26/flink-internals-how-to-handle-backpressure/)

反压：短时负载高峰导致系统接收数据的速率远高于它处理数据的速率。

***Storm***
通过监控 Bolt 中的接收队列负载情况，如果超过高水位值就会将反压信息写到 Zookeeper ，Zookeeper 上的 watch 会通知该拓扑的所有 Worker 都进入反压状态，最后 Spout 停止发送 tuple。

***JStorm***
认为直接停止 Spout 的发送太过暴力，因此通过逐级降速来进行反压的，效果会较 Storm 更为稳定，但算法也更复杂。另外 JStorm 没有引入 Zookeeper 而是通过 TopologyMaster 来协调拓扑进入反压状态，这降低了 Zookeeper 的负载。

***[Spark](https://blog.51cto.com/14309075/2414995)***
需要反压的场景:

1. 首次启动Streaming应用，kafka保留了大量未消费历史消息，并且auto.offset.reset=latest，可以防止第一个batch接收大量消息、处理时间过长和内存溢出
2. 防止kafka producer突然生产大量消息，一个batch接收到大量数据，导致batch之间接收到的数据倾斜

原理：

1. 开启反压机制，即设置**spark.streaming.backpressure.enabled** 为true；
2. spark会在作业执行结束后，调用**RateController.onBatchCompleted()** 更新batch的元数据信息：batch处理结束时间、batch处理时间、调度延迟时间、batch接收到的消息量等；
3. Spark Streaming是先从broker里查询到每个分区的latestOffset，这样就可以得到每个分区的offset range，再用range和上一步预估的速率做对比计算就可以确定每个分区的处理的消息量；
   * 有效速率 = min（maxRatePerPartition，预估的速率）
   * 一个batch的每个分区每秒接收到的消息量 = batchDuration * 有效速率

* 若是基于Kafka Receiver的数据源，可以通过设置spark.streaming.receiver.maxRate来控制最大输入速率
* 若是基于Direct的数据源(如Kafka Direct Stream)，则可以通过设置spark.streaming.kafka.maxRatePerPartition来控制最大输入速率
* 参考：https://zhuanlan.zhihu.com/p/45954932

***Flink***
没有使用任何复杂的机制来解决反压问题！它利用自身作为纯数据流引擎的优势来优雅地响应反压问题。

Flink 在运行时主要由 operators 和 streams 两大组件构成。分布式阻塞队列 就是这些逻辑流，队列容量是通过缓冲池（LocalBufferPool）来实现的。

Flink 中的数据传输相当于已经提供了应对反压的机制。因此，Flink 所能获得的最大吞吐量由其 pipeline 中最慢的组件决定。

## [Redis](https://github.com/CyC2018/CS-Notes/blob/master/notes/Redis.md)

基于内存的高性能key-value数据库；采用单线程IO多路复用的设计；[支持多种数据结构](http://www.cleey.com/blog/single/id/808.html):String,Hash,List,Set,Zset有序集合,位图；可用作数据库、高速缓存、消息队列代理.

#### 优势

* 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)
* 支持丰富数据类型，支持string，list，set，sorted set，hash
* 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行
* 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后将会自动删除

#### 与Memcache相比

* 存储方式，Redis可以持久化其数据；
* 支持更为丰富的数据类型；
* 使用底层模型，客户端之间通信的应用协议不一样，redis速度更快

#### 定期删除+惰性删除策略

6种数据淘汰策略，保证redis中的数据都是热点数据

* voltile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
* volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
* volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰
* allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘
* allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
* no-enviction（驱逐）：禁止驱逐数据（写入报错）

#### [RDB & AOF 持久化](https://blog.csdn.net/ljheee/article/details/76284082)

* RDB（redis database）：指定时间间隔对内存中的数据集快照。适合备份、容灾恢复。
  * fork出子进程尽兴存储，不适合实时备份。
  * 若大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB更加的高效。
  * 风险是可能会丢两次持久之间的数据，量可能很大。
* AOF（append only file）：持久化记录redis执行过的所有写指令，以redis协议追加保存到文件末尾。
  * 可对AOP文件重写。
  * AOF 的默认策略为每秒钟 fsync 一次，如果硬盘IO慢，会阻塞父进程。
* 可同时使用。如果redis重启的话，则会优先采用AOF方式来进行数据恢复，这是因为AOF方式的数据恢复完整度更高。

#### [Redis 主从复制](https://blog.csdn.net/Stubborn_Cow/article/details/50442950)

通过使用 slaveof host port 命令来让一个服务器成为另一个服务器的从服务器。一个从服务器只能有一个主服务器，并且不支持主主复制。

![redis-SYNC](/img/in-post/post-interview/Redis-sync.png)

PS：为了保持保持数据一致性，在Master 发送完RDB文件后，还要定期的向slave发送PING命令，最后再发送变更命令。

* 在子进程保存RDB文件期间，把接受到的这些可能变更数据库“键空间”的命令保存下来，放到每个slave的回复列表中。当RDB文件发送完master会发送这些回复列表中的内容。
* 可以创建一个中间层来分担主服务器的复制工作。

#### Redis Hash

Redis hash 是一个string类型的field和value的映射表，适合用于存储对象（相比数据库的结构化数据，hash会占用更少的内存，稀疏）。

有[两种内部编码](https://juejin.im/post/5bc359ff5188255c7b16ab72#heading-17)：

* ZipList（压缩列表）
当哈希类型的元素个数小于hash-max-ziplist-entries配置（默认512个），同时所有值都小于hash-maxziplist-value配置（默认为64字节），Redis会使用ziplist做为哈希的内部实现。Ziplist可以使用更加紧凑的结构来实现多个元素的连续存储，所以在节省内存方面更加优秀。

* HashTable（哈希表）
当哈希类型无法满足ziplist要求时，redis会采用hashtable做为哈希的内部实现，因为此时ziplist的读写效率会下降。

#### rehash

redis支持多种数据结构，其中dict是使用频率相当高。

* 内部采用拉链法解决哈希冲突
* 扩容采用[渐进式 rehash](http://redisbook.com/preview/dict/incremental_rehashing.html)
  * dict的内部，维护了两张哈希表；
  * 在渐进式 rehash 进行期间， 字典的删除（delete）、查找（find）、更新（update）等操作会在两个哈希表上进行；
  * 在渐进式 rehash 执行期间， 新添加到字典的键值对一律会被保存到 ht[1] 里面， 而 ht[0] 则不再进行任何添加操作；
  * 将 rehash 键值对所需的计算工作均滩到对字典的每个添加、删除、查找和更新操作上， 从而避免了集中式 rehash 而带来的庞大计算量。

#### [hot key & big key](https://zhuanlan.zhihu.com/p/52393940)

* hot key出现造成集群访问量倾斜：
  1. 使用本地缓存
  2. 利用分片算法的特性，对key进行打散处理
* big key 造成集群数据量倾斜：
  * 对 big key 进行拆分

#### 常见性能优化

* Master最好不要做任何持久化工作，如RDB内存快照和AOF日志文件
* 如果数据比较重要，某个Slave开启AOF备份数据，策略设置为每秒同步一次
* 为了主从复制的速度和连接的稳定性，Master和Slave最好在同一个局域网内
* 尽量避免在压力很大的主库上增加从库
* 主从复制不要用图状结构，用单向链表结构更为稳定，即：Master <- Slave1 <- Slave2 <- Slave3...这样的结构方便解决单点故障问题，实现Slave对Master的替换。如果Master挂了，可以立刻启用Slave1做Master，其他不变。

