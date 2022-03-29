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

10. 握手本身没有传输data，而是交换的是某种控制信息
11. 前面这些协议对应的应用大多是文本型应用，这些应用对数据完整性很敏感的，需要的是可靠的运输层服务

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

​				  吞吐量：侧着数据交付的速率---确保吞吐量的服务-数据编码的速率和交付的速率

​				  时效性：发送方发送到socket到到达接收方socket之间的延迟
​				  安全性：加密服务、数据完整性校验



##### Concept Summary—》Chapter 2—〉HTTP-高频协议（重点）

1. HTTP的特点

   * stateless
     * http本身不存储客户的任何信息，也就是客户来几次都当第一次看

   * 长连接or短连接：区分长连接还是短连接：看给每一对req/resp分配一个TCP还是所有req/resp放在一个TCP
     * 短连接：特点-一对req/res就办对一个TCP的initiates和close这样的重操作（通过配置tcp可以选择串行连接还是并行连接）
       * shorcoming:时间角度-性能角度；空间角度：每一个TCP连接都要分配一个TCP buffer，TCP变量，服务端增加负担了，尤其是当高并发时
       * 现状：HTTP1.0默认是长连接，而且HTTP2 允许请求和应答交错了，优化了请求和应答的机制

2. 报文格式
3. Cookie: http本身无状态，但web 有需求去identify user
   * 用户第一次访问server, 
     * server会生成一个identifyID
     *  存到后端数据库
     * 把这个id 放到 resp message
   * client 收到resp
     * 读取到set-cookie，把这个值和host放到浏览器来管理的cookie文件里
     * 下次再次访问同一站点，就会在req中放入这个站点对应的标识userID
   * server收到req, 拿到req中的cookie，后续业务逻辑就会根据这这个id操作，包括一些相关表，围绕这个ID提供服务

##### Concept Summary—》Chapter 2—〉mail相关的协议



##### Concept Summary—》Chapter 2—〉DNS目录服务

​					

##### Concept Summary—》Chapter 2—〉p2p文件分发



##### Concept Summary—》Chapter 2—〉流视频



##### Concept Summary—》Chapter 2—〉socket编程



​					

​				  