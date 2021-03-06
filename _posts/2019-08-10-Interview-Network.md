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

## 协议体系

#### OSI七层协议

![TCP-IP](/img/in-post/post-interview/Network-TCPIP.png)

#### TCP/IP四层协议

|TCP/IP协议族|四层协议|
|--|--|
|应用层|HTTP、FTP、DNS|
|传输层|TCP、UDP|
|网络层|IP（处理在网络上流动的数据包，数据包是网络传输的最小数据单位）|
|数据链路层|网络（处理连接网络的硬件部分）|

上层协议数据通过**封装**转变为下层协议数据，加上自己的头部信息（链路层还会加上尾部信息）。

#### [TCP&UDP](https://blog.csdn.net/lzj2504476514/article/details/81454754)

***TCP***

TCP协议提供面向连接、字节流和可靠的传输。

TCP协议进行通信的双方必须先建立连接，然后才能开始传输数据。TCP连接是全双工的，也就是说双方的数据读写可以通过一个连接进行。

需要进行 [三次握手，四次挥手](https://blog.csdn.net/whuslei/article/details/6667471)。

[为什么要三次握手](https://blog.csdn.net/to_be_better/article/details/54885684)

三次握手：

1. client 发 SYN
2. server 回 SYN+ACK
3. client 发 ACK

四次挥手：

1. client 发 FIN， client 进入FIN-Wait-1 状态
2. server 回 ACK， server 进入Close-Wait 状态，client收到ACK后进入FIN-Wait-2 状态
3. server 发 FIN
4. client 回 ACK， client 进入Time-Wait状态，server 收到ACK后进入Close状态

***UDP（User Data Protocol，用户数据报协议）***

UDP是一个非连接的协议，传输数据之前源端和终端不建立连接，因此也就不需要维护连接状态，包括收发状态等，因此一台服务机可同时向多个客户机传输相同的消息。

UDP信息包的标题很短，只有8个字节，相对于TCP的20个字节信息包的额外开销很小。

***主要区别***

* TCP是可靠传输,UDP是不可靠传输;
* TCP面向连接,UDP无连接;
* TCP传输数据有序,UDP不保证数据的有序性;
* TCP不保存数据边界,UDP保留数据边界;
* TCP传输速度相对UDP较慢;
* TCP有流量控制和拥塞控制,UDP没有;
* TCP是重量级协议,UDP是轻量级协议;
* TCP首部较长20字节,UDP首部较短8字节;

#### [HTTP 协议](https://www.runoob.com/http/http-messages.html)

HTTP协议是构建在TCP/IP协议之上的，是TCP/IP协议的一个子集

HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

请求方法：Get、Post、Head、OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT。

***[HTTP 与 RPC协议](https://www.zhihu.com/question/41609070)***

#### [HTTP 报文](https://www.jianshu.com/p/8f61c95c078d)

HTTP 报文包括：起始行，首部字段，主体。

#### [SSL/TLS 协议](https://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)

SSL/TLS 握手是为了安全地协商出一份对称加密的秘钥，用处：

* 所有信息都是加密传播，第三方无法窃听。
* 具有校验机制，一旦被篡改，通信双方会立刻发现。
* 配备身份证书，防止身份被冒充。

[基于RSA 的握手](https://razeencheng.com/post/ssl-handshake-detail.html)

## Cookie和Session

***cookie***

位于用户的计算机上，用来维护用户计算机中的信息，直到用户删除。

***session***

session称为会话信息，位于web服务器上，主要负责访问者与网站之间的交互，当访问浏览器请求http地址时，将传递到web服务器上并与访问信息进行匹配， 当关闭网站时就表示会话已经结束，网站无法访问该信息了，所以它无法保存永久数据

session与cookie的区别:

1. Cookie以文本文件格式存储在浏览器中，而session存储在服务端它存储了限制数据量。它只允许4kb它没有在cookie中保存多个变量。
2. cookie的存储限制了数据量，只允许4KB，而session是无限量的
3. 可以轻松访问cookie值但是我们无法轻松访问会话值，因此它更安全
4. 设置cookie时间可以使cookie过期。但是使用session-destory（），我们将会销毁会话。

总结：如果我们需要经常登录一个站点时，最好用cookie来保存信息，要不然每次登陆都特别麻烦，如果对于需要安全性高的站点以及控制数据的能力时需要用会话效果更佳，当然我们也可以结合两者，使网站按照我们的想法进行运行

## 综合

当输入一个URL后：

* 网络
  * DNS，通过url 查询 IP地址;
  * 通过UDP 打包发送 or 通过TCP 三次握手，打包，通过TLS 加密传输
  * ARP 通过IP查询 MAC 地址;
* 服务器端
  * 负载均衡 Nginx
  * 业务处理，Spring MVC、Spring Security 鉴权
* 前后台Http 交互
  * http报文，gzip压缩
  * cookie 和 session
  * 长连接、短连接
* 前台
  * 页面解析、css、html、渲染
