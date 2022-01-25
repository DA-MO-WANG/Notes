###### 参考资料

《深入理解Java虚拟机-3周志明》

#### 内存模型

​		1.Java内存模型和jvm内存模型不是一回事
​		2.计算机世界一切问题的起源在物理硬件上。要完成任务，不止是一个组件在工作，需要多个组件一起工作。而多个组件本身是有差异的，比如在运算速度上是有差异的。处理器运算速度最快。
​		不同速度的处理器和内存怎么发挥效率呢？
​				方案一：引入高速缓存（它的速度在处理器和内存之间）---？为什么有这个高速缓存能提升效率？因为它可以很快响应处理器，这样处理器就不会闲着
​				处理器处理逻辑后的寄存器数据---缓存数据----主内存数据

​				新的问题：多处理器的缓存对应同一块主内存：因为不同处理器若是处理逻辑不同，缓存数据结果也不同，那究竟把哪个回存到内存，就是一个问题？
​				内存模型--针对内存/缓存访问的过程-抽象	
​				线程----工作内存----主内存（每个线程，有独属于自己的工作内存，所有的运算都在自己的工作内存中实现，线程与其他线程的交互，要通过主内存）		
​				物理机上的内存模型、虚拟机上的内存模型，Java应用程序层次上 的内存模型，他们是互相嵌套的壳子
​				八种内存交互操作
​						发生地点：主内存
​								lock、unlock

​										   主内存---工作内存
​													read

​										   工作内存---工作内存
​													load

​											工作内存---执行引擎
​														use
​											执行引擎--工作内存
​														assign
​										  工作内存--主内存
​													store
​										  主内存---主内存
​													write
​						规则：
​								read、load 只能同时出现，顺序执行，可以非连续
​								初始化只能发生主内存

​								asign之后必须接着store和write
​								lock后会清空工作内存中此变量的值			



​					3.谈谈你对Jvm虚拟机内存模型的理解？
​							有五片内存区域
​							程序计数器 pcr: 用来记录下一条字节码的指令，属于线程私有
​							虚拟机栈：用来存储局部变量，里面有一个栈帧
​												栈帧在虚拟机栈中从入栈到出栈的过程就是一个方法从调用到结束的过程
​												属于线程私有													

​							本地方法栈：服务本地方法的调用过程，属于线程私有	
​							堆：存放对象实例，是受垃圾回收器来管理，它的扩展可以通过调节参数来实现，比如-xms. -xmx，属于线程共享
​							方法区：存放类加载时的一些信息，比如静态变量、常量，里面有一个运行时常量池，属于线程共享

​					

#### 垃圾回收算法

​		1.哪些区域的回收需要额外费心？
​				三个区域：程序计数器、虚拟机栈、本地方法栈，它们的内存分配情况，随线程而生而灭，一般存储的也只是一些关于方法的局部变量、临时变量
​									在编译期，当类结构确定，它们的内存也就就已知了。当当方法结束时，内存也就自然回收了。
​				2个区域：堆和方法区，接口对应哪个实现类，方法执行哪个分支，这些只能处于运行期才能确定。这种情况下的内存分配是动态的。

​		2.怎么判定一个对象是否死了？
​				方式一：引用计数算法——每个对象维护一个引用计数器，有一个地方引用这个对象，计数器值就加1，引用失效了，计数器值就减1，当计数器值为0 时，就意味着对象不再被使用
​						







#### 