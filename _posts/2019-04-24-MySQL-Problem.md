---
layout:     post         
title:      "MySQL 常见问题"
subtitle:   "持续记录ing"  
date:       2019-04-24 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - MySQL
    - DataBase
---

查看 [MySQL 错误代码列表](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-error-sqlstates.html)

## 访问远程被拒绝

场景复现: 新增的数据库，Navicat 远程无法登陆，但远程主机中可以正常使用。

提示信息: `Error no. 1251: "Client does not support authentication..."`

常见原因: 默认用户未配置远程访问
>已确认Linux 对应3306 端口防火墙已打开
>/sbin/iptables -I INPUT -p tcp --dport 3306-j ACCEPT  #添加端口3306
>/etc/rc.d/init.d/iptables save #保存设置
>/etc/rc.d/init.d/iptables status #查看防火墙状态

解决:

```SQL
-- 连接远程主机
mysql -u root -ppassword
use mysql;
show tables;
```

![mysql-png](/img/in-post/post-database/mysql-login-1.png)

```SQL
-- 查看用户信息
select host,user,plugin,authentication_string from mysql.user;
```

![mysql-user](/img/in-post/post-database/mysql-user-1.png)

`Host` 列指定了允许用户登录所使用的IP， `localhost` (不同于127.0.0.1)表示本机使用通过 **UNIX socket** 连接，`%` 表示允许任何通过 **TCP/IP** 连接过来的主机访问，类似 `mysql -h 172.16.0.3`。

`caching_sha2_password` 是MySQL 8.0 的默认身份验证插件, 旧版本是 `mysql_native_password`。 [Why we change it](https://mysqlserverteam.com/mysql-8-0-4-new-default-authentication-plugin-caching_sha2_password/).

```SQL
-- 修改账号密码
ALTER USER 'root'@'%' IDENTIFIED BY "newpassword";

-- 修改IP限制
UPDATE USER SET host="%" WHERE user = "root";
flush privileges;
```

接下来就可以正常使用 Navicat 远程访问了。

## MyBatis动态sql

***order by 字段，在用动态sql时会出现问题***

需要了解 #{} 和 ${} 区别：

* #{} 将传入的数据都当成一个字符串，会对自动传入的数据加一个双引号。可以很大程度防止sql注入。
* ${} 将传入的数据直接显示生成在sql中。无法防止sql注入。
* ${} 方式一般用于传入数据库对象，例如传入表名。

