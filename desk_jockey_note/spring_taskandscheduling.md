<h2>
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

######7.1.2 案例：使用一个task执行期

####7.2 对任务调度过程taskscheduler的一种抽象

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



























