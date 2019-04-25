---
layout:     post         
title:      "MySQL 常见问题"
subtitle:   "数据带来价值"  
date:       2019-04-24 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - MySQL
    - DataBase
---

采用jdbc来完成连接数据库的操作。

需要引入ojdbc14.jar。在操作前，首先得先得到数据库驱动类的对象，通过驱动对象拿到数据库连接对象。其中Class.forName(driverName)就是应用类反射机制，加载驱动程序的。

DriverManager 类是 JDBC 的管理层，作用于用户和驱动程序之间。它跟踪可用的驱动程序，并在数据库和相应驱动程序之间建立连接。一般只需要在类中直接使用方法 DriverManager.getConnection，即可建立与数据库的连接。

捕获SQL异常：SQL 是runtime运行时异常，可以不再DAO层捕捉异常，但需要在Service层捕捉异常，用于事务。

一个非常标准的Java连接Oracle数据库的示例代码：www.cnblogs.com/liuxianan/archive/2012/08/05/2624300.html

