<h2>
##7. Task Execution and Scheduling-2

> The Spring Framework provides abstractions for the asynchronous execution and scheduling of tasks with the `TaskExecutor` and `TaskScheduler` interfaces, respectively. Spring also features implementations of those interfaces that support thread pools or delegation to CommonJ within an application server environment. Ultimately, the use of these implementations behind the common interfaces abstracts away the differences between Java SE 5, Java SE 6, and Java EE environments.

Spring框架通过设计 TaskExecutor 和 TaskScheduler 两个接口，来对异步执行和任务调度做出了抽象。Spring 也突出介绍了支持这些接口的线程池或代理模式的实现。最后，这些常规抽象接口的应用做到了与各种具体开发环境的隔离。

> Spring also features integration classes to support scheduling with the `Timer` (part of the JDK since 1.3) and the Quartz Scheduler ( [http://quartz-scheduler.org](http://quartz-scheduler.org/)). You can set up both of those schedulers by using a `FactoryBean` with optional references to `Timer` or `Trigger` instances, respectively. Furthermore, a convenience class for both the Quartz Scheduler and the `Timer` is available that lets you invoke a method of an existing target object (analogous to the normal `MethodInvokingFactoryBean` operation).

Spring 也重点设计了一些整合类来支持调度，比如用Timer 、Quartz Scheduler.  你可以通过使用Fa ctoryBean，传进可选择的引用-Quartz Scheduler 和 Timer 来创建这两个。进一步看，一个对Quartz 和Timer 都方便的类是能够让你调用一个存在着的目标对象的方法。

####7.1 对task执行过程taskExecutor的一种抽象-4

> Executors are the JDK name for the concept of thread pools. The “executor” naming is due to the fact that there is no guarantee that the underlying implementation is actually a pool. An executor may be single-threaded or even synchronous. Spring’s abstraction hides implementation details between the Java SE and Java EE environments.



######7.1.1 task执行器类型-6

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



























