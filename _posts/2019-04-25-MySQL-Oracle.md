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

## 数据类型差异

单独写在[这里了](/2019/04/29/Database-Datatype)

## 字符类型差异

#### 背景

MySQL和Oracle在它们支持的字符类型以及它们存储和检索字符类型值的方式上存在一些差异。

Oracle 官网解释 [Character Data Types](https://docs.oracle.com/cd/E12151_01/doc.150/e12155/oracle_mysql_compared.htm#i1026408)

#### 对比

> Before MySQL 5.0.3, `Varchar` 类型长度规范和 `Char` 相同。 后续只讨论 *MySQL 5.0.3* 之后的版本。

<style>
/*等于 th:nth-of-type(1)*/
table th:first-of-type {
    width: 20%; 
}
table th:nth-of-type(2){
    width: 40%;
}
table th:nth-of-type(3){
    width: 40%;
}
/*隔行变色*/
table tbody tr:nth-child(2n) {
    background: rgba(158,188,226,0.12);
}
</style>
| | MySQL | Oracle
| --- | ---- | ---
支持类型 | `Char`, `Varchar` | `CHAR`, `NCHAR`, `NVARCHAR2`, `VARCHAR2`
最短长度 | 从 MySQL 3.23 开始，`char` 可以被声明为0字节长度 | 所有字符类型可以声明的最小长度为1个字节
最大长度 | `Char` 最大长度为 255 字节,<br>`Varchar`最大长度为 65535 字节 | `CHAR` 和 `NCHAR` 允许的最大长度为 2,000 字节,<br>`NVARCHAR2` 和 `VARCHAR2` 允许的最大长度为 4,000 字节
char 填充 | 存储时用空格填充指定长度，但在检索值时删除尾随空格 | 存储时自动补足空格，并在检索时不删除尾随空格
varchar 填充 | 使用给定的字符数来存储值，在存储和检索值之前删除尾随空格 | 完全按给定的方式存储和检索值，包括尾随空格。
值超出指定长度 | 截断该值<br>*除非设置了STRICT SQL模式，否则不会生成错误* | 生成错误
处理empty string| `varchar` 区分 empty string 和 NULL 值 | `varchar2` 把空串等同于null处理.<br>正如ANSI标准规定的，Oracle保留VARCHAR以支持将来区分NULL和空字符串
字符集 | 每个字符类型（`CHAR`，`VARCHAR`和`TEXT`）列都有一个列字符集和排序规则。如果未在列定义中显式定义字符集或排序规则，则在指定时隐含表字符集或排序规则; 否则，选择数据库字符或排序规则。| `CHAR`,`VARCHAR2`类型的字符集由数据库字符集定义，而`NCHAR`,`NVARCHAR`类型的字符集定义为国家字符集
默认长度语义 | `Char`, `Varchar`的默认长度语义是 *字符 Character* 而不是 *字节 Byte* | `CHAR`, `NCHAR`的默认长度语义是 *字节 Byte*<br>`NVARCHAR2`, `VARCHAR2`的默认长度语义是 *字符 Character*

> 字符集
> GBK: 汉字和全角等字符占2个字节，数字，英文字符等都是1个字节.
> UTF-8: 汉字占3个字节，英文1个。

#### Tips

1. CHAR的长度是固定的，而VARCHAR2的长度是可以变化的。
   > 比如，存储字符串“abc"，对于CHAR (20)，表示你存储的字符将占20个字节(包括17个空字符)，而同样的VARCHAR2 (20)则只占用3个字节的长度，20只是最大值，当你存储的字符小于20时，按实际长度存储。
2. CHAR的效率比VARCHAR2的效率稍高。
3. 目前VARCHAR是VARCHAR2的同义词。

深入一点：[char varchar nchar nvarchar 四者的区别](https://www.cnblogs.com/sumsen/archive/2012/05/30/2525735.html)

#### 常见问题场景

`mysql` 和 `oracle` 做数据同步。其中表的一个字段在 `mysql` 中设置为varchar(6),Oracle中为varchar2(6)。但mysql中能正常存放的数据同步到oracle中却报 ***ORA-12899: value too large for column*** 错误。

> 分析:要考虑 **默认长度语义** 和 **字符集编码方式**

## Column Default Value

对于设置为不允许 NULL 值的列，`MySQL` 与 `Oracle` 处理其默认值的方式不同。

- `MySQL`中，对于不允许NULL值且在将数据插入表时没有为列提供数据的话, `MySQL`会去确定列的默认值。此默认值是列数据类型的隐式默认值。
  - 如果启用了 strict mode，则MySQL会生成错误，对于事务表，它会回滚insert语句。
- `Oracle`，当数据插入表中时，必须为不允许NULL值的所有列提供数据。 Oracle不会为具有NOT NULL约束的列生成默认值。

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

A: 先得到一个临时表有rownum列（一个伪列，类似rowid）,再在临时表中来查询。

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