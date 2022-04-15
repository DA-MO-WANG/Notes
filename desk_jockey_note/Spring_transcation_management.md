###1.4.6 使用 @Transactional

> n addition to the XML-based declarative approach to transaction configuration, you can use an annotation-based approach. Declaring transaction semantics directly in the Java source code puts the declarations much closer to the affected code. There is not much danger of undue coupling, because code that is meant to be used transactionally is almost always deployed that way anyway.

除了基于XML的事务配置，你也可以使用基于注解的方式。Java源代码中的事务语义与代码耦合太高，这里并没有过度联系的危险，因为被事务管理的代码可以以任何方式部署。

> The standard `javax.transaction.Transactional` annotation is also supported as a drop-in replacement to Spring’s own annotation. Please refer to JTA 1.2 documentation for more details.