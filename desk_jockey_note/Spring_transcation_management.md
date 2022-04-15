###1.4.6 使用 @Transactional

> n addition to the XML-based declarative approach to transaction configuration, you can use an annotation-based approach. Declaring transaction semantics directly in the Java source code puts the declarations much closer to the affected code. There is not much danger of undue coupling, because code that is meant to be used transactionally is almost always deployed that way anyway.

除了基于XML的事务配置，你也可以使用基于注解的方式。Java源代码中的事务语义与代码耦合太高，这里并没有过度联系的危险，因为被事务管理的代码可以以任何方式部署。

> The standard `javax.transaction.Transactional` annotation is also supported as a drop-in replacement to Spring’s own annotation. Please refer to JTA 1.2 documentation for more details.

标准库中的注解，javax.transaction.Transactional 也可以被支持作为一种对Spring自己的注解的替代。

> The ease-of-use afforded by the use of the `@Transactional` annotation is best illustrated with an example, which is explained in the text that follows. Consider the following class definition:

@Transactional 注解的简单使用在下面的例子中被更好地说明。

> Used at the class level as above, the annotation indicates a default for all methods of the declaring class (as well as its subclasses). Alternatively, each method can be annotated individually. See [Method visibility and `@Transactional`](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction-declarative-annotations-method-visibility) for further details on which methods Spring considers transactional. Note that a class-level annotation does not apply to ancestor classes up the class hierarchy; in such a scenario, inherited methods need to be locally redeclared in order to participate in a subclass-level annotation.

在Class层次上使用注解，表明了对这个class上所有的方法都应用了注解。当然，也可以单独在每个方法上加注解。当然，父类的事务注解，不意味在子类起作用。

> When a POJO class such as the one above is defined as a bean in a Spring context, you can make the bean instance transactional through an `@EnableTransactionManagement` annotation in a `@Configuration` class. See the [javadoc](https://docs.spring.io/spring-framework/docs/5.3.19/javadoc-api/org/springframework/transaction/annotation/EnableTransactionManagement.html) for full details.



#### 	事务配置

@Transactional 注解是元数据，明确接口、类、方法必须拥有事务语义。配置如下：kv

* value 指定使用的事务管理器；也可用transactionManager
* label ?
* 传播模式propagation
* isolation隔离level : 只用在特定的传播模式下-REQUIRED\REQUIRES_NEW
* 超时时间timeout : 秒粒度，只用在特定的传播模式下-REQUIRED\REQUIRES_NEW
* readonly: 读写互斥，只用在特定的传播模式下-REQUIRED\REQUIRES_NEW
* rollbackFor / rollbackForClassName：引起回滚的异常类型
* noRollbackFor / noRollbackForClassName：设置什么样的异常类不会引发回滚



#### 多个Transaction Managers下的@Transactional



#### 用户自定义注解

（什么场景下使用用户自定义注解）如果你发现你在很多不同方法上，重复使用相同配置的@Transactional，你可以使用Spring 元注解支持的自定义注解。

```java
//封装思想的体现，一次配置，下次使用时就可以少敲很多键盘
@Target({ElementType.METHOD,ElementType.TYPE})//ElementType枚举类，指定具体的注解作用的范围，这里是方法、类上
@Retention(RetentionPolicy.RUNTIME)//保留阶段配置，这里是在运行时保留
@Transactional(配置)//本次自定义封装的核心
public @interface OrderTx {}//定义了自定义注解的名字
```



###1.4.7 事务的传播模式

在Spring管理的事务中，意识到物理事务和逻辑事务上的不同，以及传播模式的配置是如何区分这种不同的。

PROPAGATION_REQUIRED 强制执行了一种物理事务，