##前言

分布式架构

EJB被Spring革命了

事实上，当EJB被接受时，EJB沦为被过度推崇(狂热的氛围利于推广)。从而让EJB的缺陷很快暴露出来：笨重、复杂、不易测试,Entity Bean的失败；人们开始探索新的技术

Spring，风格是一切从实际出发，强调基于POJO的轻量级推进快速开发。先进的技术理念：IoC、AOP

##Spring框架的由来

EJB只是一种特定场景下的解决方案之一，没有一种解决方案是普遍使用的。脱离具体场景来讨论任何解决方案都是脱离实际的表现。

更具灵活性的一种设计，只需要一个大气层，围绕基础的POJO，只提供各种服务列表，不以绑定的死板形式出现，而是以可插拔的形式，按用户需要逐步添加，这样就能保持轻量级、灵活度。服务映射成具体的东西就是模块。![image-20220415112930491](book_Spring揭秘.assets/image-20220415112930491.png)

一棵树这样的生命必须依赖强大的根基才能生长繁盛。Spring框架内的各个模块，从横向来看，同一水平线下各个模块之间相互独立；纵向来看，上层的模块需要依赖下层的模块才能正常工作。

> 正像Donald J. Trump在How To Get Rich一书中所说 的那样：“Before the dream lifts you into the clouds, make sure look hard at the facts on the ground.”
>
> //cloud->很美好的感觉；ground->hard-》cloud是建立在groud之上的，cloud是groud的一部分



# IoC的基本概念

全名Inversion of Control ，这里的control意思就是指bean层面的东西，在inversion之前是用户要什么自己new, 反转之后是容器基于POJO准备好各种bean服务，需要什么给你准备什么。类似好莱坞原则“Don't call us, we will call you”

如果是以前，被注入对象与被依赖对象之间是直接依赖关系------体现在new 操作上；这种关系变动时影响成本很高；如果是IoC，被注入对象和被依赖对象通过IoC Service Provider来打交道，在这里是全部没对关系都交给Spring容器管理。被注入对象需要什么，只需要和IoC Service Provider打声招呼(体现在@Autowired 和 @bean注解的一些操作)，它就会把需要依赖的对象注入到被注入对象中。这体现了IoC Service Provider为被注入对象服务的目的。尤其是大工程，开发者越来越享受框架提供的这种服务，把对象的管理托管给容器，而不是开发者本身，减轻了开发者的负担。这是为什么框架得到广泛接纳的原因。

###2.2 IoC通过什么方式把依赖对象注入到被注入对象中：多种形式

###2.3 IoC带来的好处

* 本质就是解耦，接触业务对象之间的依赖关系的一种对象绑定方式
* 有了IoC，就可以当新增功能时，对原有代码的侵入性很小，复用相同业务逻辑
* 方便测试，当我们测试时并不是所有业务组件都要测试，往往我们只对个别几个有测试需要，有了IoC，我们可以拆出来需要的组件来测试。这建立在耦合度很低的程度。过去用new 维护的硬耦合的方式，很难拆出子业务组件。



## IoC Service Provider

1. 几个角色
   * 业务对象、客户端对象、IoC service provider、被注入对象、依赖对象
   * 我们要提前准备好，等需要的时候就拿出来；而不是这些玩意都是我们自己人自己玩；把对象的构建关系从业务对象中剥离出来，对象之间的绑定关系，这些东西怎么识别？人类需要记忆，机器也需要一个记录，记录的方式不止一种：
     * 直接编码：把各种对象注册到容器类中，需要的时候再从容器类中获取
     * 配置文件
     * 注解形式：利用注解来打标签，借助标签暗示的信息来组装数据



#Spring IoC容器

1. IoC容器和IoC service provider 的关系

   IoC容器包含IoC service provider的功能，但不仅限于IoC service provider。它还包括支持各种支持比如AOP

2. Spring IoC容器的类型

   * BeanFactory：基础原生的情况，只提供基础的IoC服务支持，所以启动速度较快

     * 默认lazy-load：延迟初始化策略，只有要用到容器中某个受管对象时，才对该受管对象进行初始化和依赖注入操作

     

   * ApplicationContext：除了有BeanFactory提供的支持外，还有其他高级特性，而且要求全部初始化并绑定完成，需要更多系统资源，所以启动时间很长

     * 在BeanFactory基础上构建

3. BeanFactory的认识

   4.1 BeanFactory 对象注册与依赖绑定方式

   ![image-20220420165020297](book_Spring揭秘.assets/image-20220420165020297.png)

* 

  * 硬编码方式

    * BeanFactory 是总管理bean的总容器。DefaultListableBeanFactory是具体实现类；BeanDefinitionRegistry是抽象出来的管理注册与依赖的逻辑接口；BeanDefinition是保存被注入对象的各种信息
    * 注册逻辑就是设置kv对，k是bean的名字，v是beandefinition类

  * 外部xml配置方式

    * 有一个专门读取bean-xml格式的reader类，用来把xml内容加载到管理注册的类DefaultListableBeanFactory里

  * 基于注解方式

    * ```
      /ApplicationContext容器，加载触发器配置
      //xml  component-scan标注了要扫描的包范围
      //@Compoment 标注了哪些类需要被注册到容器里
      //@Autowired 标注了哪里需要被注入，以及注入什么对象
      ```

​	

#####4.4 容器功能实现的各个阶段

​		![image-20220421160605379](book_Spring揭秘.assets/image-20220421160605379.png)

*  容器启动阶段：根据图纸装配生产线
  * 加载对象生产设计图，比如xml图纸，容器利用BedefinitionReader工具类加载xml图纸，解析，把解析后的信息封装到 用来保留对象信息的BeanDefinition类中，最后把其注册到管理对象注册的BeanDefinitionRegistry上。
* Bean实例化阶段:当有人显式或隐式通过getBean请求对象时，就会触发实例化流程，容器会检测是否已经初始化，如果没有，就会根据注册了的对象的保存信息实例化该对象，并为其注入依赖
  * Bean实例化过程p74

