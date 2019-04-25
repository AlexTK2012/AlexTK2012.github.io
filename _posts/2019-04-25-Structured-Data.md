---
layout:     post         
title:      "结构化数据"
subtitle:   "结构化数据与非结构化数据的区别"  
date:       2019-04-25 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Data Structure
---

## 概述

Structured data is far easier for Big Data programs to digest, while the myriad formats of unstructured data creates a greater challenge. Yet both types of data play a key role in effective data analysis.

相对于结构化数据（即行数据，存储在数据库里，可以用二维表结构来逻辑表达实现的数据）而言，不方便用数据库二维逻辑表来表现的数据即称为非结构化数据，包括所有格式的办公文档、文本、图片、XML、HTML、各类报表、图像和音频/视频信息等等。

## Structured data

Structured data usually resides in relational databases (RDBMS-Relational database management system). Data may be human- or machine-generated as long as the data is created within an RDBMS structure.

结构化数据是数据的数据库(即行数据,存储在数据库里,可以用二维表结构来逻辑表达实现的数据)。

![structured-data](/img/in-post/post-data-structure/structured-data.jpg)

我们可以清楚的看到能够形式化存储在数据库中，每一个列都有具体的含义。

#### Structured Data Examples

The most common examples of structured data are numbers, names, dates, addresses, and transactional information .

## Unstructured data

Unstructured data is essentially everything else. Unstructured data has internal structure but is not structured via pre-defined data models or schema. It may be textual or non-textual, and human- or machine-generated. It may also be stored within a non-relational database like NoSQL.

![unstructured-data](/img/in-post/post-data-structure/unstructured-data.jpg)

> 图片来自于吴恩达老师deeplearning课程slides

非结构数据与结构化数据相比较而言，更难让计算机理解。


#### Unstructured Data Examples

The most common examples of unstructured data are survey responses, social media comments, blog comments, email responses, and phone call transcriptions.

## Difference

![differ](/img/in-post/post-data-structure/structured-unstructured.jpg)

Structured data is objective facts and numbers that most analytics software can collect, making it easy to export, store, and organize in typical databases like Excel, Google Sheets, and SQL. You can also easily examine structured data with standard data analysis methods and tools like regression analysis and pivot tables.

On the contrary, unstructured data is usually subjective opinions and judgments of your brand in the form of text, which most analytics software can’t collect, making it difficult to export, store, and organize in typical databases. You also can’t examine unstructured data analysis methods and tools. Most of the time, you must store unstructured data in Word documents or NoSQL databases and manually analyze it or use the analysis tools in a NoSQL database to examine this type of data.

## Reference

[Structured vs. Unstructured Data](https://www.datamation.com/big-data/structured-vs-unstructured-data.html)

[Structured vs unstructured data](https://learn.g2crowd.com/structured-vs-unstructured-data)

可以先理解下`定性与定量数据` [qualitative vs quantitative data](https://learn.g2crowd.com/qualitative-vs-quantitative-data)

[结构化数据与非结构化数据的区别](https://zhuanlan.zhihu.com/p/29856645)