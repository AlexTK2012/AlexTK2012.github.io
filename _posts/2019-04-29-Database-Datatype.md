---
layout:     post         
title:      "MySQL 与 Oracle 数据类型对比"
# subtitle:   "记录常见区别～"  
date:       2019-04-29 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Database
    - MySQL
    - Oracle
---

> For more information about Oracle data types, see [Oracle Database SQL Language Reference](https://docs.oracle.com/cd/B19306_01/server.102/b14200/toc.htm)

This section lists the difference between MySQL and Oracle data types. For some MySQL data types there is more than one alternative Oracle data type. The tables include information about the following:

## Numeric Types

When mapping MySQL data types to numeric data types in Oracle, the following conditions apply:

- If there is **no precision or scale** defined for the destination Oracle data type, precision and scale are taken from the **MySQL source data type**.
- If there is a precision or scale defined for the destination data type, these values are ***compared*** to the equivalent values of the source data type and **the maximum value is selected**.

MySQL | Size | Oracle
 ---- | ---- | ----
BIGINT | 8 Bytes | NUMBER (19,0)
BIT | approximately (M+7)/8 Bytes | RAW
DECIMAL(M,D) | M+2 bytes if D > 0, M+1 bytes if D = 0 (D+2, if M < D) | FLOAT(24), BINARY_FLOAT
DOUBLE | 8 Bytes | FLOAT(24), BINARY_FLOAT, BINARY_DOUBLE
DOUBLE PRECION | 8 Bytes | FLOAT(24), BINARY_DOUBLE
FLOAT(25<=X <=53) | 8 Bytes | FLOAT(24), BINARY_FLOAT
FLOAT(X<=24) | 4 Bytes | FLOAT, BINARY_FLOAT
INT | 4 Bytes | NUMBER (10,0)
INTEGER | 4 Bytes | NUMBER (10,0)
MEDIUMINT | 3 Bytes | NUMBER (7,0)
NUMERIC | M+2 bytes if D > 0, M+1 bytes if D = 0 (D+2, if M < D) | NUMBER
REAL | 8 Bytes | FLOAT(24), BINARY_FLOAT
SMALLINT | 2 Bytes | NUMBER(5,0)
TINYINT | 1 Byte | NUMBER(3,0)

## Date and Time Types

MySQL | Size | Oracle
 ---- | ---- | ----
DATE | 3 Bytes | DATE
DATETIME | 8 Bytes | DATE
TIMESTAMP | 4 Bytes | DATE
TIME | 3 Bytes | DATE
YEAR | 1 Byte | NUMBER

## String Types

When mapping MySQL data types to character data types in Oracle, the following conditions apply:

- If there is **no length** defined for the destination data type, the length is taken from the **source data type**.
- If there is a length defined for the destination data type, the **maximum value of the two lengths** is taken.

MySQL | Size | Oracle
 ---- | ---- | ----
BLOB | L + 2 Bytes whereas L<2^16 | RAW, BLOB
CHAR(m) | M Bytes, 0<=M<=255 | CHAR
ENUM (VALUE1, VALUE2, ...) | 1 or 2 Bytes depending on the number of enum. values (65535 values max) |  
LONGBLOB | L + 4 Bytes whereas L < 2 ^ 32 | RAW, BLOB
LONGTEXT | L + 4 Bytes whereas L < 2 ^ 32 | RAW, CLOB
MEDIUMBLOB | L + 3 Bytes whereas L < 2^ 24 | RAW, BLOB
MEDIUMTEXT | L + 3 Bytes whereas L < 2^ 24 | RAW, CLOB
SET (VALUE1, VALUE2, ...) | 1, 2, 3, 4 or 8 Bytes depending on the number of set members (64 members maximum) |  
TEXT | L + 2 Bytes whereas L<2 ^ 16 | VARCHAR2, CLOB
TINYBLOB | L + 1 Bytes whereas L<2 ^ 8 | RAW, BLOB
TINYTEXT | L + 1 Bytes whereas L<2 ^ 8 | VARCHAR2
VARCHAR(m) | L+1 Bytes whereas L<=M and0<=M<=255 before MySQL 5.0.3 (0 <= M <= 65535 in MySQL 5.0.3 and later; effective maximum length is 65,532 bytes) | VARCHAR2, CLOB