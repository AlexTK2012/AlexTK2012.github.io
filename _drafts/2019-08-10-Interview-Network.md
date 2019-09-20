---
layout:     post         
title:      "计算机网络"
date:       2019-08-10 12:00:00             # 时间
author:     "Alex"                          # 作者
header-img: "img/animate-bg/bg1-min.jpg"
catalog: true                               # 是否归档
tags:                                       # 标签，可多个
    - Interview
---

## TODO

https://github.com/CyC2018/CS-Notes/blob/master/notes/计算机网络%20-%20目录.md

## Ping

*输入 ping IP 后敲回车，发包前会发生什么:*

首先根据目的IP和路由表决定走哪个网卡，再根据网卡的子网掩码地址判断目的IP是否在子网内。如果不在则会通过arp缓存查询IP的网卡地址，不存在的话会通过广播询问目的IP的mac地址，得到后就开始发包了，同时mac地址也会被arp缓存起来。

## 缓存

https://github.com/CyC2018/CS-Notes/blob/master/notes/缓存.md

淘汰策略

* FIFO（First In First Out）: 先入先出，很好理解，就和队列一样，先进队列的先出队列。
* LRU（Least Recently Used）: 最近最少使用，意思就是最近读取的数据放在最前面，最早读取的数据放在最后面，如果这个时候有新的数据进来，那么最后面存储的数据淘汰。
* LFU（Least Frequently Used）: 最不常使用，意思就是对存储的数据都会有一个计数引用，然后队列按数据引用次数排序，引用数多的排在最前面，引用数少的排在后面。

[缓冲区溢出攻击](
https://www.cnblogs.com/fanzhidongyzby/archive/2013/08/10/3250405.html)

## TCP

|TCP/IP协议族|四层协议|
|--|--|
|应用层|HTTP、FTP、DNS|
|传输层|TCP、UDP|
|网络层|IP（处理在网络上流动的数据包，数据包是网络传输的最小数据单位）|
|数据链路层|网络（处理连接网络的硬件部分）|

上层协议数据通过**封装**转变为下层协议数据，加上自己的头部信息（链路层还会加上尾部信息）。

#### TCP&UDP

TCP协议提供面向连接、字节流和可靠的传输。TCP协议进行通信的双方必须先建立连接，然后才能开始传输数据。TCP连接是全双工的，也就是说双方的数据读写可以通过一个连接进行。

需要进行三次握手，四次挥手： https://blog.csdn.net/whuslei/article/details/6667471

## [HTTP 协议](https://www.runoob.com/http/http-messages.html)

HTTP协议是构建在TCP/IP协议之上的，是TCP/IP协议的一个子集

HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

请求方法：Get、Post、Head、OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT。

