#Dynamo: 亚麻的高可用 KV存储平台

###摘要

> Reliability at massive scale is one of the biggest challenges we face at Amazon.com, one of the largest e-commerce operations in the world; even the slightest outage has significant financial consequences and impacts customer trust. The Amazon.com platform, which provides services for many web sites worldwide, is implemented on top of an infrastructure of tens of thousands of servers and network components located in many datacenters around the world. At this scale, small and large components fail continuously and the way persistent state is managed in the face of these failures drives the reliability and scalability of the software systems. This paper presents the design and implementation of Dynamo, a highly available key-value storage system that some of Amazon’s core services use to provide an “always-on” experience. To achieve this level of availability, Dynamo sacrifices consistency under certain failure scenarios. It makes extensive use of object versioning and application-assisted conflict resolution in a manner that provides a novel interface for developers to use

在大规模场景下的高可用性，是我们在Amazon面临的最大挑战。即使是哪怕很微小的运行宕机也会产生很明显的经济后果，影响到用户对我们的信任。Amazon平台，提供了海量网页服务，基于驻扎在世界各地的数以万计的服务器、网络组件组成的数据中心实现的。在这种规模下，大大小小的失败不断地在发生。正是这些失败，呼唤一种持续良好状态，促使软件系统对对可用性和可伸缩性的关注。这篇paper 描述了Dynamo的设计与实现，Dynamo是一种高可用的key-value存储系统。Amazon的很多核心服务借助这个系统实现了一种“总是在线”的状态。为了实现这种级别的可用性，Dynamo牺牲了某些失败场景下的持久性。他通过为开发人员提供一种新接口，充分解决了对象版本机制和应用程序辅助之间的矛盾。



### 基本术语

算法、管理、测量、性能、设计、可用性



### 1. 介绍

> Amazon runs a world-wide e-commerce platform that serves tens of millions customers at peak times using tens of thousands of servers located in many data centers around the world. There are strict operational requirements on Amazon’s platform in terms of performance, reliability and efficiency, and to support continuous growth the platform needs to be highly scalable. Reliability is one of the most important requirements because even the slightest outage has significant financial consequences and impacts customer trust. In addition, to support continuous growth, the platform needs to be highly scalable.

Amazon 运行着世界级别的电子交易平台。在高峰时段，这个平台服务着上亿顾客，通过成千上万台世界各地的数据中心的服务器。就性能、可用性、效率性而言，Amazon平台有一个严格使用指标。为了支持平台不断增长的需要，还要求具有可伸缩性。可用性是最重要的一个指标，因为即使再微小的宕机也会造成很严重的经济损失和用户口碑流失。其次，为了应对不断增长的需要，平台还需要高可伸缩性。

> One of the lessons our organization has learned from operating Amazon’s platform is that the reliability and scalability of a system is dependent on how its application state is managed. Amazon uses a highly decentralized, loosely coupled, service oriented architecture consisting of hundreds of services. In this environment there is a particular need for storage technologies that are always available. For example, customers should be able to view and add items to their shopping cart even if disks are failing, network routes are flapping, or data centers are being destroyed by tornados. Therefore, the service responsible for managing shopping carts requires that it can always write to and read from its data store, and that its data needs to be available across multiple data centers. 

我们从运作Amazon平台得到的教训之一，是一个系统的可靠性和可伸缩性取决于其应用状态是怎么被管理的。Amazon使用高度去中心化的、松散耦合的，面向服务的架构。这种体系架构由由上百个服务组成。在这种环境下，特别需要一种始终保证可用的存储技术。比如，即使磁盘故障，网络路由震荡，或者数据中心被龙卷风破坏，用户也能查看，并且添加货物到购物车。因此，管理购物车的服务需要一种存储技术，保证它能始终读写。而且数据也需要跨越多个数据中心可用。

> Dealing with failures in an infrastructure comprised of millions of components is our standard mode of operation; there are always a small but significant number of server and network components that are failing at any given time. As such Amazon’s software systems need to be constructed in a manner that treats failure handling as the normal case without impacting availability or performance. 

在一个由百万组件组成的基础设施中处理故障，是我们日常的运作模式。因此，



> To meet the reliability and scaling needs, Amazon has developed a number of storage technologies, of which the Amazon Simple Storage Service (also available outside of Amazon and known as Amazon S3), is probably the best known. This paper presents the design and implementation of Dynamo, another highly available and scalable distributed data store built for Amazon’s platform. Dynamo is used to manage the state of services that have very high reliability requirements and need tight control over the tradeoffs between availability, consistency, cost-effectiveness and performance. Amazon’s platform has a very diverse set of applications with different storage requirements. A select set of applications requires a storage technology that is flexible enough to let application designers configure their data store appropriately based on these tradeoffs to achieve high availability and guaranteed performance in the most cost effective manner. 





> There are many services on Amazon’s platform that only need primary-key access to a data store. For many services, such as those that provide best seller lists, shopping carts, customer preferences, session management, sales rank, and product catalog, the common pattern of using a relational database would lead to inefficiencies and limit scale and availability. Dynamo provides a simple primary-key only interface to meet the requirements of these applications. 





> Dynamo uses a synthesis of well known techniques to achieve scalability and availability: Data is partitioned and replicated using consistent hashing [10], and consistency is facilitated by object versioning [12]. The consistency among replicas during updates is maintained by a quorum-like technique and a decentralized replica synchronization protocol. 



