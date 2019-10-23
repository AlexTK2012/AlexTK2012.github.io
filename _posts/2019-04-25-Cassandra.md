---
layout:     post         
title:      "Cassandra"
subtitle:   "基本使用"  
date:       2019-04-25 18:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Database
    - Cassandra
---

## 简介

[Cassandra](https://cassandra.apache.org) 是一个来自 Apache 的大规模、高可扩展的分布式开源 `NoSQL` 数据库，可用于管理大量的结构化数据。它提供了高可用性、高性能，没有单点故障。完美适用于跨数据中心／云端的结构化数据、半结构化数据和非结构化数据。

## 特点

- **弹性可扩展性** - `Cassandra`允许添加更多的硬件以适应更多的客户和更多的数据根据要求。
- **始终基于架构** - `Cassandra`没有单点故障，它可以连续用于不能承担故障的关键业务应用程序。
- **快速线性性能** - `Cassandra`是线性可扩展性的，即它为你增加集群中的节点数量增加你的吞吐量。因此，保持一个快速的响应时间。
- **灵活的数据存储** - `Cassandra`适应所有可能的数据格式，包括：结构化，半结构化和非结构化。它可以根据您的需要动态地适应变化的数据结构。
- **便捷的数据分发** - `Cassandra`通过在多个数据中心之间复制数据，可以灵活地在需要时分发数据。
- **事务支持** - `Cassandra`支持属性，如原子性，一致性，隔离和持久性（ACID）。
- **快速写入** - `Cassandra`被设计为在廉价的商品硬件上运行。 它执行快速写入，并可以存储数百TB的数据，而不牺牲读取效率。

## 架构

`Cassandra` 的设计目的是处理跨多个节点的大数据工作负载，而没有任何单点故障。 `Cassandra` 在其节点之间具有对等分布式系统，并且数据分布在集群中的所有节点之间。

- 集群中的所有节点都扮演相同的角色。 每个节点是独立的，并且同时互连到其他节点。
- 集群中的每个节点都可以接受读取和写入请求，无论数据实际位于集群中的何处。
- 当节点关闭时，可以从网络中的其他节点提供读/写请求。

## CQL

**CQL (Cassandra Query Language)** 是用于 `Cassandra` 的查询语言，可类比用于关系型数据库的SQL，注意，虽然CQL 和 SQL 看起来比较相似，但二者内部原理完全不同。

> 测试环境可以通过 Docker 直接搭建
> docker run --name mycassandra -p 3000:9042 -d cassandra:latest

## CQL 命令介绍

#### Shell 命令

下面给出了Cqlsh记录的shell命令。这些是用于执行任务的命令，如显示帮助主题，退出cqlsh，描述等。

- **HELP** -显示所有cqlsh命令的帮助主题。
- **CAPTURE** -捕获命令的输出并将其添加到文件。
- **CONSISTENCY** -显示当前一致性级别，或设置新的一致性级别。
- **COPY** -将数据复制到Cassandra并从Cassandra复制数据。
- **DESCRIBE** -描述Cassandra及其对象的当前集群。
- **EXPAND** -纵向扩展查询的输出。
- **EXIT** -使用此命令，可以终止cqlsh。
- **PAGING** -启用或禁用查询分页。
- **SHOW** -显示当前cqlsh会话的详细信息，如Cassandra版本，主机或数据类型假设。
- **SOURCE** -执行包含CQL语句的文件。
- **TRACING** -启用或禁用请求跟踪。

#### CQL数据定义命令

- **CREATE KEYSPACE** -在Cassandra中创建KeySpace。
- **USE** -连接到已创建的KeySpace。
- **ALTER KEYSPACE** -更改KeySpace的属性。
- **DROP KEYSPACE** -删除KeySpace。
- **CREATE TABLE** -在KeySpace中创建表。
- **ER TABLE** -修改表的列属性。
- **DROP TABLE** -删除表。
- **NCATE** -从表中删除所有数据。
- **CREATE INDEX** -在表的单个列上定义新索引。
- **DROP INDEX** -删除命名索引。

#### CQL数据操作指令

- **INSERT** -在表中添加行的列。
- **UPDATE** -更新行的列。
- **DELETE** -从表中删除数据。
- **BATCH** -一次执行多个DML语句。

#### CQL字句

- **SELECT** -此子句从表中读取数据
- **WHERE** -where子句与select一起使用以读取特定数据。
- **ORDERBY** -orderby 子句与 select 一起使用，以特定顺序读取特定数据。

## 常用场景

#### Select

CQL没有连接查询和子查询，因此select语句只适用于单个表。

- `LIMIT` 选项限制查询返回的行数，而 `PER PARTITIONLIMIT` 选项限制查询为给定分区返回的行数，两种类型的限制可以在同一语句中使用。
- `GROUP BY` 允许将一组列中共享相同值的所有选定行压缩为单个行，只能在分区键级别或在集群列级别对行进行分组。因此，`GROUP BY` 选项只接受主键顺序中的 **主键列名** 作为参数。
- `ORDER BY` 允许选择返回结果的顺序，ASC用于升序，DESC用于降序，默认ASC。它使用列名称列表以及列的顺序作为参数。
- `ALLOW FILTERING` 允许显式地允许（一些）需要过滤的查询。
  假设建表:

  ```SQL
    create table test(c1 int,c2 int, c3 int,c4 int, primary key(c1,c2,c3));
    -- c1 是分区键(partition key)
    -- c2、c3 是排序键(clustering key)
    -- c4 是普通列
  ```

  需要加上的场景:
  - 缺少 partition key 的等值过滤条件.
    `select * from test where c2=2;`
  - 对普通列值过滤.
    `select * from test where c1=1 and c2=1 and c3=1 and c4>10;`
  - 范围查询后跟精确查询
    `select * from test where c1=1 and c2>20 and c3=10;`
- 。。。

#### Describe

此命令描述 Cassandra 及其对象的当前集群，可简写为 `desc`，不区分大小写。

Command | Usage
:-----  | :-----
desc cluster | 提供有关集群的信息
desc Kkyspaces | 列出集群中的所有键空间
desc tables | 列出了键空间中的所有表
desc table `table_name` <br>(table 可省略) | 提供表的描述
desc types | 列出所有用户定义的数据类型

#### 实例

Assume we have 2 tables called Inspections and Restaurants.

```SQL
-- List 10 restaurants.
select * from Restaurants limit 10;

-- List all the restaurant names.
select name from Restaurants;

-- Name and borough of restaurant id 41569764.
select name,borough from Restaurants where id=41569764;

-- Dates and grades of the inspections of restaurant id 41569764.
select inspectiondate,grade from inspections where idrestaurant=41569764;

-- Names of the restaurants offering French cuisine.
select name from restaurants where cuisinetype='French';

-- Names of the restaurants from the BROOKLYN borough.
select name from restaurants where borough='BROOKLYN' Allow Filtering;

-- Grades and scores of inspections for restaurant id 41569764 with a score of at least 10.
select grade,score from inspections
where idrestaurant=41569764 and score > 10 allow filtering;

-- Grades (non null) of inspections with a score greater than 30.
-- CQL 没有 NOT 或 != 操作
select grade from inspections where score>30 and grade>'' allow filtering;

-- Number of results for the previous query.
select count(grade) from inspections where score > 30 allow filtering;
```

## Cassandra and Big Data – Conclusions

Cassandra is considered today as one of the most powerful NoSQL databases in a Big Data environment. When a real-world project requires working on very large volumes of data, the challenge is to write the data quickly. And on this point, Cassandra has demonstrated its superiority. As we saw before, the scalability of Cassandra makes it particularly suitable for an environment where data are distributed on multiple servers. Thanks to Cassandra's architecture, distribution requires lightweight management, balanced over all nodes.

One would think that putting a Cassandra cluster in production is done with a few simple steps/clicks. In reality, this may be much more delicate. Indeed, Cassandra offers a very open data modeling, which gives access to a lot of possibilities, and allows above all to do anything. Unlike the relational data, with Cassandra, we cannot just store documents. It is indeed necessary to have a detailed knowledge of the data that will be stored, the way in which it will be queried, its distribution on the different nodes. The design of the Cassandra data model therefore requires special attention, as poorly performing modeling in production with PBs of data will give catastrophic results.

Cassandra also makes it possible not to constrain the number of key / value pairs in the documents. When a document has a lot of values, we are talking about a “wide row”. Wide rows are an advantage in terms of modeling, but the more values a document has, the heavier it is. We must therefore consider this tradeoff. Remember also that in Cassandra, being a NoSQL database, the concept of joins does not exist.

The similarities with the relational model and particularly SQL make the initial learning curve easy, particularly to those who have a lot of experience with SQL. On the other hand, they can lead users to underestimate this extremely powerful database. Cassandra offers high performance, provided that the model of the data is adequate for the workload.
 
## Reference

[W3CSchool cassandra教程](https://www.w3cschool.cn/cassandra/)

[Cassandra 的使用者](https://www.zhihu.com/question/19592244)

[数据类型](https://blog.csdn.net/vbirdbest/article/details/77802031)