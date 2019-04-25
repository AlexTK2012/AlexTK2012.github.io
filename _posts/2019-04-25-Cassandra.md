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

## CQL Shell 常用命令

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

#### Describe

此命令描述 Cassandra 及其对象的当前集群，可简写为 `desc`

Command | Usage
:-----  | :-----
desc cluster | 提供有关集群的信息
desc Keyspaces | 列出集群中的所有键空间
desc tables | 列出了键空间中的所有表
desc table `table_name` <br>(table 可省略) | 提供表的描述
desc types | 列出所有用户定义的数据类型

## Reference

[W3CSchool cassandra教程](https://www.w3cschool.cn/cassandra/)

[Cassandra 的使用者](https://www.zhihu.com/question/19592244)
