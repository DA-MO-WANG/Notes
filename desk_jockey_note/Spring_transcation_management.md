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

@Transactional 注解是元数据，明确接口、类、方法必须拥有事务语义。配置如下：

* 传播模式propagation
* 隔离level
* 事务类型
* 超时时间timeout
* 触发回滚的异常

