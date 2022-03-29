##Spring Framework Overview

> Spring makes it easy to create Java enterprise applications. It provides everything you need to embrace the Java language in an enterprise environment, with support for Groovy and Kotlin as alternative languages on the JVM, and with the flexibility to create many kinds of architectures depending on an application’s needs. As of Spring Framework 5.0, Spring requires JDK 8+ (Java SE 8+) and provides out-of-the-box support for JDK 9 already.

Spring让创造Java企业级应用变得很简单。它提供了你要在企业级开发中需要的一切东西。它支持很多种语言，支持根据应用需要创造各种各样的架构。

> Spring supports a wide range of application scenarios. In a large enterprise, applications often exist for a long time and have to run on a JDK and application server whose upgrade cycle is beyond developer control. Others may run as a single jar with the server embedded, possibly in a cloud environment. Yet others may be standalone applications (such as batch or integration workloads) that do not need a server.

Spring 支持很大范围的应用场景。在大规模企业级开发中，应用通常会运行很长一段时间，但JDK版本升级或者应用服务器升级通常让事情超出开发者的控制。另外一种情况，是应用以一个jar的形式嵌入到服务器上来运行，也存在在云环境中运行。还有一种情况，其他人可能运行一个独立过程，比如批处理或者整合负载，以一种不需要服务器的形式运行。

> Spring is open source. It has a large and active community that provides continuous feedback based on a diverse range of real-world use cases. This has helped Spring to successfully evolve over a very long time.

Spring本身是开源的，它本身拥有很多很活跃的社区，这些社区提供了以真实世界案例为基础的连续不断的反馈。



















##7. Task Execution and Scheduling-2

> The Spring Framework provides abstractions for the asynchronous execution and scheduling of tasks with the `TaskExecutor` and `TaskScheduler` interfaces, respectively. Spring also features implementations of those interfaces that support thread pools or delegation to CommonJ within an application server environment. Ultimately, the use of these implementations behind the common interfaces abstracts away the differences between Java SE 5, Java SE 6, and Java EE environments.

Spring框架通过设计 TaskExecutor 和 TaskScheduler 两个接口，来对异步执行和任务调度做出了抽象。Spring 也突出介绍了支持这些接口的线程池或代理模式的实现。最后，这些常规抽象接口的应用做到了与各种具体开发环境的隔离。

> Spring also features integration classes to support scheduling with the `Timer` (part of the JDK since 1.3) and the Quartz Scheduler ( [http://quartz-scheduler.org](http://quartz-scheduler.org/)). You can set up both of those schedulers by using a `FactoryBean` with optional references to `Timer` or `Trigger` instances, respectively. Furthermore, a convenience class for both the Quartz Scheduler and the `Timer` is available that lets you invoke a method of an existing target object (analogous to the normal `MethodInvokingFactoryBean` operation).

Spring 也重点设计了一些整合类来支持调度，比如用Timer 、Quartz Scheduler.  你可以通过使用Fa ctoryBean，传进可选择的引用-Quartz Scheduler 和 Timer 来创建这两个。进一步看，一个对Quartz 和Timer 都方便的类是能够让你调用一个存在着的目标对象的方法。

####7.1 对task执行过程taskExecutor的一种抽象-4

> Executors are the JDK name for the concept of thread pools. The “executor” naming is due to the fact that there is no guarantee that the underlying implementation is actually a pool. An executor may be single-threaded or even synchronous. Spring’s abstraction hides implementation details between the Java SE and Java EE environments.

Executors 是JDK中对应线程池的概念。这样命名“executor”是出于这样一种事实：我们并不能保证底层实现是一个池子。可能，executor 是一个单线程，甚至也可能是锁。Spring的抽象隐藏了实现细节，在SE和EE环境之间。

> Spring’s `TaskExecutor` interface is identical to the `java.util.concurrent.Executor` interface. In fact, originally, its primary reason for existence was to abstract away the need for Java 5 when using thread pools. The interface has a single method (`execute(Runnable task)`) that accepts a task for execution based on the semantics and configuration of the thread pool.

Spring的TaskExecutor 接口，是和concurrent包下的Executor接口是一样的。它存在的主要原因就是抽象出Java 5 时使用 线程池的需要。这个接口只有一个方法，这个方法用来接收一个任务，然后基于线程池的语义和配置来执行。

> The `TaskExecutor` was originally created to give other Spring components an abstraction for thread pooling where needed. Components such as the `ApplicationEventMulticaster`, JMS’s `AbstractMessageListenerContainer`, and Quartz integration all use the `TaskExecutor` abstraction to pool threads. However, if your beans need thread pooling behavior, you can also use this abstraction for your own needs.

TaskExecutor 被创造出来，来给Spring其他组件一种抽象，针对在线程池必需的地方。比如像ApplicationEventMulticaster、JMS’s AbstractMessageListenerContainer、整合的Quartz 都使用TaskExecutor 抽象来池化线程。当然，如果你的beans 需要池化的行为，你也可以基于你自己的需要使用这种抽象。

######7.1.1 task执行器类型-6

> Spring includes a number of pre-built implementations of `TaskExecutor`. In all likelihood, you should never need to implement your own. The variants that Spring provides are as follows:

Spring 自己就有很多预先实现的TaskExecutor。这么多可能性，自然你没必要去实现一个你自己的。Spring提供的各种类型如下：

- > SyncTaskExecutor`: This implementation does not execute invocations asynchronously. Instead, each invocation takes place in the calling thread. It is primarily used in situations where multi-threading is not necessary, such as in simple test cases.

SyncTaskExecutor:  这种实现根本没必要异步执行调用。相反，每一个调用都发生在调用线程的时候。它主要用在多线程不必要的地方，比如单例测试。

- > `SimpleAsyncTaskExecutor`: This implementation does not reuse any threads. Rather, it starts up a new thread for each invocation. However, it does support a concurrency limit that blocks any invocations that are over the limit until a slot has been freed up. If you are looking for true pooling, see `ThreadPoolTaskExecutor`, later in this list.

SimpleAsyncTaskExector: 这种实现根本没有重用任何线程。换句话说，它每次调用，都开始一个新的线程。但是它却是存在一种并发瓶颈，当slot 没有被释放时，调用就会被锁住。

- > `ConcurrentTaskExecutor`: This implementation is an adapter for a `java.util.concurrent.Executor` instance. There is an alternative (`ThreadPoolTaskExecutor`) that exposes the `Executor` configuration parameters as bean properties. There is rarely a need to use `ConcurrentTaskExecutor` directly. However, if the `ThreadPoolTaskExecutor` is not flexible enough for your needs, `ConcurrentTaskExecutor` is an alternative.

ConcurrentTaskExecutor:  这个实现是对concurrent包下的Executor实例的一个适配器。在bean 配置文件里，可以调节Executor参数来选择。很少见直接使用concurrentTaskExecutor的，如果ThreadPoolTaskExecutor 不能灵活适配你的需要，可以用ConcurrentTaskExecutor

> `ThreadPoolTaskExecutor`: This implementation is most commonly used. It exposes bean properties for configuring a `java.util.concurrent.ThreadPoolExecutor` and wraps it in a `TaskExecutor`. If you need to adapt to a different kind of java.util.concurrent.Executor`, we recommend that you use a `ConcurrentTaskExecutor` instead.

ThreadPoolTaskExecutor：这个实现是使用频率最高的，通过在bean 配置文件中，配置一个 ThreadPoolExecutor，然后在TaskExecutor里把这个括进去。如果你想要更多选择，ConcurrentTaskExecutor是一个选择。

> `WorkManagerTaskExecutor`: This implementation uses a CommonJ `WorkManager` as its backing service provider and is the central convenience class for setting up CommonJ-based thread pool integration on WebLogic or WebSphere within a Spring application context.

WorkManagerTaskExecutor：计时器和工作管理器 API (CommonJ)

> `DefaultManagedTaskExecutor`: This implementation uses a JNDI-obtained `ManagedExecutorService` in a JSR-236 compatible runtime environment (such as a Java EE 7+ application server), replacing a CommonJ WorkManager for that purpose.

DefaultManagerTaskexecutor：这个实现使用JNDI方式的ManagedExecutorService ，在JSR-236一致的运行时环境，替换 CommonJ WorkManager

######7.1.2 案例：使用一个task执行期

Spring的TaskExecutor 被通过一个简单的JavaBeans实现。 下面的例子里，我们通过使用ThreadPoolTaskExecutor定义一个bean，来异步打印一系列信息。

你自己根本没必要去从池里检索线程，然后执行，你要做的只是把一个要跑的任务放到队列里。然后Ta s kExecutor使用内部的规则来决定这个任务什么时候执行。

为了配置TaskExecutor使用的规则，我们可以在配置文件里暴露这个。

####7.2 对任务调度过程taskscheduler的一种抽象

Spring 3.0引入了TaskScheduler 和 调度任务的一系列方法，下面列出了接口中的一系列方法：

最简单的方法名字叫schedule(), 它只运行一个任务，和一个时间点。这造成了它只会在确定时间跑一次。其他方法都支持调度任务来重复执行。固定速率、固定间隔的方法以阶段性来执行，但是参数为Trigger的方法更加灵活。

###### 7.2.2 触发器trigger的实现









###### 7.2.3 调度器taskScheduler的实现

####7.3 注解支持-调度和异步执行

######7.3.1 授权调度注解

###### 7.3.2 @Scheduled 调度注解

######7.3.3 @Async 异步注解

######7.3.4 基于异步的限定执行

###### 7.3.5 基于异步的异常管理

#### 7.4 任务的命名空间

###### 7.4.1 调度人scheduler要素

###### 7.4.2 执行人要素

###### 7.4.3 调度任务scheduled-tasks要素

#### 7.5 使用Quartz 调度

###### 7.5.1 使用JobDetailFactoryBean

###### 7.5.2 使用MethodInvokingJobDetailFactoryBean

###### 7.5.3 通过使用Triggers 和 SchedulerFactoryBean 来布置任务



























