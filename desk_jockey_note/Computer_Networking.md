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

UDP socket 是二元组来标定的——(目的地ip,目的地端口号)



### 面向连接的(TCP)multiplex 和 demultiplex

tcp server 有一个**welcoming socket**,等待一个连接建立的请求

连接建立的请求，通常只是一个tcp segments, 它不包含 application data

TCP socket 是四元组标定——(源端口号、源ip、目的地端口号、目的地ip) ; tcp segments 也包含这四个字段

##### web servers 和 TCP

在结束讨论之前，还有一点话要说：web server 是如何使用端口号的？我们先假定有一台主机上运行着一个web server，比如说Apache 服务器，在80端口。当客户端比如浏览器，发送很多segments给服务器，所有segments都朝向80端口。特别的是初始化连接建立的segment和携带HTTP请求信息的segment都将到达80端口。正如我们描述的那样，server能区分出来自不同客户端的segments。

图3.5展示web server 为每一个连接都派生出一个新的进程。正如3.5所示，这些进程中的每一个都拥有自己的connection socket 用来接收Http request 和 发出Http response。然而我们还要提到，在connection socket和进程之间不总是一一对应的。事实上，今天的高性能web server仅仅使用一个进程，当一个新的client connect到来时，就会创建出一个携带一个新的connection socket的新的线程。对于这样的服务器，在给定时间里，就会存在在同一个process上附带着很多connection socket。

如果client 和server 使用 长连接HTTP，在长连接期间，client 和 server 通过相同的server socker交换HTTP 报文，然而，如果client 和 server 使用短连接HTTP，那么伴随每一个请求/响应对，一个新的TCP连接被创建和销毁过程。这种频繁的创建和销毁会极大的影响一个繁忙的server。

现在我们已经讨论了运输层的复用和分用服务，让我们换一下来讨论因特网运输层协议-UDP。我们会看到UDP并没有给网络层添加多少多余的东西，除了复用和分用外。



##### 不面向连接的运输层：UDP

###### how it works, what is does

为了更好地讨论UDP，假定你现在很有兴趣设计一个没有虚饰的，皮包骨头的运输层协议，你有什么想法吗？你可能首先考虑到使用一个空洞的运输层协议。尤其是在发送端，你可能想到去从应用进程里取得message, 然后直接把message 传到 network layer。在接收端，你可能想到直接从network layer里获取message，然后把message直接传到applicaition layer。但是在上一节中我们了解到我们不得不做一些额外的事情。至少，运输层得提供multiplex 和 demultiplex 服务，为了把数据从network layer 传到application layer中恰当的进程里。

除了复用、分用功能，还有一些轻量级的错误检查，它再也没有往ip上增加什么东西了。如果应用开发者选择UDP而不是TCP，这个应用几乎是直接和IP对话，UDP直接从应用进程中拿到数据，然后附加上源和目的地端口号字段来为实现复用分用服务，还有2个小字段，然后把这个结果字段传给网络层。网络层把运输层segments注入到IP数据报里，然后把这个部分发到接收端主机。如果segment到达接收端主机，UDP就会使用目的端口号来把segment的数据发到相应的用户进程里。注意！在发送一个segment之前，在运输层的接收方和发送方之间没有握手。基于这个原因，UDP被认为是不面向连接的。

为什么一些应用开发者会选择基于UDP而不是选择TCP，TCP不是能提供可靠的数据传输吗？

* 没有congestion-control machanism的好处：UDP就是简单的把application layer的data发送到UDP，UDP把数据打包到UDP segment中，然后把这个segment发送到网络层。另一方面，TCP有一个拥塞控制机制，会在源端到目的地的链路变得非常拥挤时，就挤掉一些TCP发送端。TCP会一直重发，直到收到目的地的ack。但是很多应用接受不了延迟，能够忍受一些数据丢失。
* 没有establish connection的好处：TCP像一个绅士，在发送之前有三次握手；UDP没有这些正式准备礼仪工作，就是简单粗暴扔数据。这样就没有连接建立的延迟
* 不用保持connection state的好处：connection state 的maintain 是通过一些参数实现的，比如收发缓冲、拥塞控制参数、串序、ack number, tcp 需要维护这些parameters; UDP 是不需要追踪这些参数
* header 部分small：TCP 头部20字节那么大；UDP头部只有8字节



在讨论UDP segment结构之前，我们谈到即使使用UDP，一个应用也是能拥有可靠的数据传输。只要这种可靠性建立在应用自身。我们谈到QUIC协议在UDP之上实现了可靠性。但是这带来一种困难，应用开发者忙于调试很长时间（？？？），即使这样，直接在应用层上构建可靠性，会让应用能吃上自己的蛋糕。应用进程能可靠地交流，而不受TCP拥塞控制机制对传输速率的限制。

#### UDP segment 结构

application data 占据了 UDP segment中的data field。UDP header 部分只有4个字段，每一个占2个字节。source 和 dest port 用来实现把data 传送到相应的进程里；length 字段表示UDP header + data 部分的字节数量大小，因为会变化，所以要记录。checksum用来核查segment是否有错误。事实上，checksum也会在IP header里用到。

##### UDP checksum

这个字段提供了error侦查。bits是否改变了

UDP的错误检查机制：checksum存储的是所有字段的位串和的反值，然后传输到接收端，就回把这些值加起来（包括checksum），会得到全1串，这样容易检测是否混进0去了

系统设计的原则：在高层次提供它的成本相比，低层次实现一个功能是多余的

UDP虽然提供了检测，但不意味着它会修复，它只会扔掉错误的字段



#### 3.4 可靠性数据传输的原则

数据可靠传输的问题，不只是运输层的问题，也是链路层，还有应用层。

给上层实体提供的服务抽象是数据传输的可靠channel。在这个channel里，转发的数据没有发生任何损坏或者丢失，一切按着sent的顺序传输。这也是TCP提供的服务模型。

一种可靠传输协议的关键部分就是实现这种服务模型。这个任务很难，因为实际来讲下面的层次是不可靠的。比如TCP就建立在不可靠的IP层次之上。更一般的说，两个可靠交流的端点之下的层次由单一的物理链接组成。我们只是向上提供一种抽象--一种可靠的抽象。实际开发就得考虑如何在不可靠的现实下如何开发出可靠的抽象。

考虑到底层的channel 模型越来越复杂，我们将不断迭代开发sender和receiver 之间的可靠传输协议。比如我们设想，当底层channel发生数据篡改丢失时，这个时候我们希望协议机制能做什么。这里我们假设分组是按照发送次序进行交付，底层channel不会对分组重排序。rdt-send函数代表的是可靠运输层提供给上层的接口，方便上层把数据交付给send。rdt-send 和deliver-data是一一对应的，一个是发送端调用下层提供的服务，一个是接受端调用下层提供的交付服务。至于这个下层对发送和接受怎么实现的，它可定是类似调用更下层提供的服务--udt-send和rdt-recv()，这个是对底层channel的发送接收的一个抽象。



##### 构造一个可靠数据传输协议

有限状态机概念，指定了下描述规则：横线上方代表的是引起变迁的事件，下方是事件发生时所采取的操作。如果上方没有事件，下方没有动作，就用特殊符号表示。

rdt 1.0 ：在发送端，rdt-send从上层拿到数据，打包成packet，然后传到channel；接收端，rdt-recv从channel中拿到数据，然后通过deliever-data把数据交付给上层

rdt 2.0：

###### 考虑增加bit error的情况：rdt 2.0

一个更真实的底层channel模型是一种分组bit会出现bits错误的模型。这种bit error通常会出现在网络中的物理实体组件传输packet、扩展、缓存时。我们仍然假设被传输的packet接收的顺序与发送的顺序一致。

在开发基于这样一个channel的用于更真实交流的协议时，首先要考虑的是人们怎么处理这种情况。比如人们打电话要说一段话时，在一个典型的场景里，信息接收者会说ok，在他听到、理解、记录每一句话后。如果信息接收者听到一段模糊的话，你就会被要求重复这段话。这样的信息协议使用的是肯定-否定机制。这样的信息控制机制，允许接收者让发送者知道什么样的信息被正确发送了。什么样的信息被错误发送，然后需要重复。在计算机网络配置中，一个可靠的数据传输协议，基于这样的重传机制，被叫做ARQ。

从本质来看，ARQ协议需要额外的三个部分来处理bit error这种情况：

* 错误侦测，当错误发生时，得让接收方知道；比如UDP中的checksum字段就是这个设计作用
* 接收者反馈，让发送端掌握接收情况，比如有ack、nak，比如一个bit来 标识
* 重传机制

图3.1 展示了rdt 2.0 的FSM存在。

rdt 2.0 发送端有两个状态。最左边的状态，发送端等待着来自上层的要传输的数据。当rdt_send事件发送时，发送端就会创造一个packet包裹着要被发送的数据。









（现实中的网络体系，根本不是一下子非常完美的，而是随着时间，不断打补丁，但是我们在了解时，很多修改的痕迹被刻意遮掩了，只留下流畅的没有出错的部分，所以看起来就像一生下来很完美一样，怎么可能呢，人又不是神，即使那些所谓聪明的人也一样，他们却是能想出一些出奇的点子，但这不意味着他们像违反逻辑的超自然存在）



195

就跟画画一样，先画出模子，然后逐步填充细节，按照画的意图

整个过程就像一个数据怎么从数据产生的地方，流到遥远的另一个主机上，中间经历了哪些关卡，为了度过关卡，得在通关文碟上加什么东西。基于逻辑的设计。

（我越来越喜欢翻译了！！！！:smile）

（有一个想法：根本就是一个过程为了便于理解，来拆分出七个或5个部分，实际上是为了完成一个目的的一个大过程）

（翻译不是目的，而是记录上一次看到的东西，然后下一次在这个基础上再做功，这样让大脑层次化递进）



​				  