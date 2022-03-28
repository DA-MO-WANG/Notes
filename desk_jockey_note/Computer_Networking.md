#####Homework Problems and Questions—》Chapter 2—〉section 2.1

1.微信-HTTP; QQ-OICQ; IE-HTTP; outlook-POP3

2.网络架构是五层架构或者七层---数据视角；应用层架构是c/s 或p2p架构---服务视角

3.区分c/s，不是看数据，而是看一对request/response. 发出请求的是client-交流的初始方；给出响应的是server-等待被接触的

4.不同意，p2p还是有cs概念，但是不像c/s架构，存在一个中心化的server，而是每一个peer既是client又是server，在不同时刻。

5.ip + port number

6.如果要尽快处理一个远程客户端到服务端的一个事务，会选用tcp还是udp？

7.没有这样的应用，因为数据不丢失就是建立在时间成本上

8.考察udp/tcp提供的服务类型

9.考察tls

#####Concept Summary—》Chapter 2—〉section 2.1

先讲了一个网络应用架构和网络架构的区分：前者离人更近，高层次，后者物理更近，比较底层

引申出常见的网络应用架构，有两个，分别介绍：

​		c/s架构：依赖一个数据中心

​		p2p架构：minimal reliance; self- scalability

进程之间的通信：不同主机上的不同进程之间的通信

​		讲了下的client 和 server的概念

​		socket的概念：应用和网络之间的交界点--代码可以涉及的地方

选择运输服务

​		如何选择符合需要的运输协议？看你的需要和提供的服务的吻合程度：

​				  数据传输是否可靠：看能否提供完整的数据交付服务-能否容忍丢失的程度--数据的完整性

​				  