---
layout:     post         
title:      "MySQL 与 Oracle"
subtitle:   "记录常见区别～"  
date:       2019-04-25 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Database
    - MySQL
    - Oracle
---

> MySQL 与 Oracle 在细节处理上会有各种细微区别，持续记录～

## varchar2 字节长度

#### 常见问题场景

`mysql` 和 `oracle` 做数据同步。其中表的一个字段在 `mysql` 中设置为varchar(6),Oracle中为varchar2(6)。但mysql中能正常存放的数据同步到oracle中却报 ***ORA-12899: value too large for column*** 错误。

#### 原因分析

> 要看字符集编码方式

- MySQL中 `varchar(6)` 代表可以存放6个汉字，6个字母，或6个数字。
- Oracle中 `varchar2(6)` 代表可以中存放6个字节，即 Oracle中 `varchar2` 的长度代表字节数而不是字符数。
- MySQL中一个汉字占三个字节，Oracle中一个汉字占两个字节。

#### `varchar` or `varchar2`

> 主要区别在于处理 `empty string` 和 `NULL Value` 的方式不同.

1. `char`的长度是固定的，而`varchar2`的长度是可以变化的。
2. `varchar` 区分空川和NULL；正如ANSI标准规定的，Oracle保留VARCHAR以支持将来区分NULL和空字符串。
3. `varchar2` 把空串等同于null处理，
4. `varchar2` 字符要用几个字节存储，要看数据库使用的字符集，一般情况下，`varchar2` 把所有字符都占两字节处理，`varchar` 只对汉字和全角等字符占两字节，数字，英文字符等都是一个字节；

大部分情况下建议使用 `varchar2` 类型，可以保证更好的兼容性。

## 分页查询

#### MySQL

*LIMIT* 子句可以被用于强制 *SELECT* 语句返回指定的记录数。

```SQL
SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
```

- LIMIT 子句可以被用于强制 SELECT 语句返回指定的记录数。
- LIMIT 接受一个或两个数字参数。参数必须是一个整数常量。
  - 如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目
- 初始记录行的偏移量是 0(而不是 1)
- 为了与 PostgreSQL 兼容，MySQL 也支持句法： LIMIT # OFFSET #。
  
举例：

```SQL
// 为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1.
// 检索记录行 96-last.
mysql> SELECT * FROM table LIMIT 95,-1;

//如果只给定一个参数，它表示返回最大的记录行数目.
//检索前 5 个记录行
mysql> SELECT * FROM table LIMIT 5;

//换句话说，LIMIT n 等价于 LIMIT 0,n。
```

#### Oracle

> 那么oracle中有limit吗，答案是没有。oracle中可以通过 ***rownum*** 来实现这种查询。

###### Rownum

`rownum` 是个伪列，是随着结果集生成的，返回的第一行分配的是1，第二行是2等等，生成的结果是依次递加的，没有1就不会有2。

- 注意，不返回的就不算，第一条返回的结果的 `rownum` 为1。
- 我们说rownum不支持>, >=, =, between and，只支持<, <=等。虽说不支持，但并不会报错，只是返回的数据为空，这是因为根本不能满足这样的where条件。
- 实现 `rownum>n (n>0)` 的效果，`rownum` 本身是不行的，所以就可以先查询出一个结果集合。再从这个集合中查询，这时集合中的行号就是一个普通的字段了，可以做任何比较。即使用子查询方法来解决。

举例：

```SQL
1）查询表中的前8条记录
select * from area where rownum <= 8

2）查询第2到第8条记录，三种：

A: 先得到一个临时表有rownum列（一个伪列，类似rowid）,再在临时表中来查询。

select id,province,city,district
from (select id,province,city,district,rownum as num from area)
where num between 2 and 8;

B:使用集合减运算符minus，该操作返回在第一个select中出现而不在第二个select中出现的记录。

select * from area
where rownum <= 8 minus select * from area where rownum < 2;

C: 使用集合交运算符intersect。

select id,province,city,district
from (select id,province,city,district,rownum as num from area)
where num >=2
intersect
select * from area where rownum <= 8;
```

###### Reference

[oracle rownum使用与分页](https://blog.csdn.net/u012750578/article/details/14441211)

[Oracle ROWNUM用法和分页查询总结](https://blog.csdn.net/fw0124/article/details/42737671)