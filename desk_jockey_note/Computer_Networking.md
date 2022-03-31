Homework Problems and Questions—》Chapter 2—〉section 2.1

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



##### Concept Summary—》Chapter 2—〉<font color = #F000000>HTTP-高频协议</font>

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
   * server收到req, 拿到req中的cookie，后续业务逻辑就会根据这这个id操作，包括一些相关表，围绕这个ID提供服务。

4. web 缓存

   * 好的一面

     * 减少平均响应时间

       * https://www.zhihu.com/question/317549997

       * 考虑缓存命中率，40%立即得到响应，剩下60%走ordinary server ，平均下来2秒降到1.2秒

     * 减少整个链路的通信量，从而不用升级链路？

   * 新的问题：缓存不一致的问题，混存之后发生了改变

     * conditional Get请求：在报文里维护一个last modified字段和if - modified- since 字段，来去verify                                                                                                                                                                             

5. Http/2

   * http 1.1 遇到的问题：HOL-页面中前面的大对象，挡住了后面小对象的发送，从影响用户体验延迟--短板效应——》多个并行的TCP连接来解决，但是增大了带宽占用
   * http2 解决的方式：
     * framing机制：把各种message切成小帧，每个对象的帧轮流发送，然后收到了再组合
     * 同一个client 多个请求：优先级配置，按照优先级发送帧
     * server push : 不用等他请求这些对象，通过分析html, 就知道哪些对象是必需的，然后一股脑发送到client

6. http 3（草图）

   * 底层是QUIC , 包括了很多http 2的特性， HTTP 3是建立在 quic 基础上的

##### Concept Summary—》Chapter 2—〉mail相关的协议



##### Concept Summary—》Chapter 2—〉DNS目录服务

​					

##### Concept Summary—》Chapter 2—〉p2p文件分发



##### Concept Summary—》Chapter 2—〉流视频



##### Concept Summary—》Chapter 2—〉socket编程









## 3.2 Multiplexing混在一起用 和 Demultiplexing拆出来用

这一节的目的就是讨论运输层的复合和分解，也就是从网络层 主机到主机的传输扩展到两个主机上的进程到进程之间的传输。

为了更具体地讨论，我先从一个因特网环境下最简单基础的运输层服务开始。

在目的地主机上，运输层从下面的网络层收到很多segments。运输层的任务就是把这些segments中的data发送到对应的应用上。

然后举了一个例子：是怎么做到把segments中的数据发送到相应的应用上的？

一个进程拥有很多sockets，穿过这个门，来自网络中的数据来到了这个进程，同时也是穿过这个门来自进程的数据发到了网络中。因此，运输层并不是把数据直接发到进程，而是通过了一个中介-socket。但是在一段时间内，在接收主机上不止一个socket，每一个socket都有一个独一无二的标识符。这个标识符的类型依赖于是UDP socket还是TCP socket。

每一个运输层segments上，设计了一系列字段来实现这个目的。在接收端主机上，运输层检查了这些字段来确定接收的socket，然后把这个segment路由到这个socket。把运输层segment发送到正确的socket的任务被叫demultiplex，类似从一团中拿到一小个放到对应的一小个中。从不同的socket中收集数据块，在每一个数据块上嵌入头部信息，来创造一个segment，然后把这个segment传送到网络层，这个工作就叫multiplex

类比：从每个人手里收集mail就叫multiplex; 把mail分到每个人手里就叫demultiplex

######How it is actually done in a host？

segments 和 socket的故事：

* 存在这么一个socket
* segment上要有特定的字段设计，来实现特定发送的目的
  * 源端口号/目的端口号
  * 16位0-65535，0-1023公开协议使用的





### 无连接的(UDP)multiplex 和 demultiplex

每一个socket都得有一个端口号，只不过这个端口号可以自动分配；也可以手动绑定。通常UDP client自己的端口号，在socket创建时自动分配的；UDP server 手动绑定的。



​					

​				  