#Dynamo: 亚麻的高可用 KV存储平台

###摘要

> Reliability at massive scale is one of the biggest challenges we face at Amazon.com, one of the largest e-commerce operations in the world; even the slightest outage has significant financial consequences and impacts customer trust. The Amazon.com platform, which provides services for many web sites worldwide, is implemented on top of an infrastructure of tens of thousands of servers and network components located in many datacenters around the world. At this scale, small and large components fail continuously and the way persistent state is managed in the face of these failures drives the reliability and scalability of the software systems. This paper presents the design and implementation of Dynamo, a highly available key-value storage system that some of Amazon’s core services use to provide an “always-on” experience. To achieve this level of availability, Dynamo sacrifices consistency under certain failure scenarios. It makes extensive use of object versioning and application-assisted conflict resolution in a manner that provides a novel interface for developers to use

在大规模场景下的高可用性，是我们在Amazon面临的最大挑战。即使是哪怕很微小的运行宕机也会产生很明显的经济后果，影响到用户对我们的信任。Amazon平台，提供了海量网页服务，基于驻扎在世界各地的数以万计的服务器、网络组件组成的数据中心实现的。在这种规模下，大大小小的失败不断地在发生。正是这些失败，呼唤一种持续良好状态，促使软件系统对对可用性和可伸缩性的关注。这篇paper 描述了Dynamo的设计与实现，Dynamo是一种高可用的key-value存储系统。Amazon的很多核心服务借助这个系统实现了一种“总是在线”的状态。为了实现这种级别的可用性，Dynamo牺牲了某些失败场景下的持久性。他通过为开发人员提供一种新接口，充分解决了对象版本机制和应用程序辅助之间的矛盾。



### 基本术语

算法、管理、测量、性能、设计、可用性



### 1. 介绍

> Amazon runs a world-wide e-commerce platform that serves tens of millions customers at peak times using tens of thousands of servers located in many data centers around the world. There are strict operational requirements on Amazon’s platform in terms of performance, reliability and efficiency, and to support continuous growth the platform needs to be highly scalable. Reliability is one of the most important requirements because even the slightest outage has significant financial consequences and impacts customer trust. In addition, to support continuous growth, the platform needs to be highly scalable.

Amazon 运行着世界级别的电子交易平台。在高峰时段，这个平台服务着上亿顾客，通过成千上万台世界各地的数据中心的服务器。就性能、可用性、效率性而言，Amazon平台有一个严格使用指标。为了支持平台不断增长的需要，还要求具有可伸缩性。可用性是最重要的一个指标，因为即使再微小的宕机也会造成很严重的经济损失和用户口碑流失。其次，为了应对不断增长的需要，平台还需要高可伸缩性。