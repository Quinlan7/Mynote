# hibernate

*Hibernate 是目前业界中主流的对象关系映射（ORM）框架*

+ hibernate与mybatis

> Hibernate和MyBatis都是Java平台上常用的对象关系映射（ORM）框架，用于在应用程序和数据库之间进行数据的持久化操作。它们有一些共同点，同时也有一些区别。
>
> **Hibernate**：
> - Hibernate是一个全面的ORM框架，它提供了将Java对象映射到关系数据库中的功能。它负责管理对象的生命周期、执行数据库操作以及处理对象之间的关系。
> - Hibernate使用基于注解或XML的映射方式，将Java对象与数据库表进行映射。它可以自动生成SQL语句并执行，开发者无需手动编写SQL。
> - Hibernate提供了一系列高级特性，如延迟加载、缓存、事务管理等。它具有强大的查询语言（HQL）和标准化的查询语言（Criteria API），方便开发者进行复杂的查询操作。
> - Hibernate通常适用于开发大型、复杂的应用程序，它可以提高开发效率，并且具有良好的扩展性。
>
> **MyBatis**：
> - MyBatis是一个半自动化的ORM框架，它提供了将SQL语句与Java方法进行映射的能力。开发者需要手动编写SQL语句，并通过XML或注解将SQL与Java方法关联起来。
> - MyBatis将Java对象与数据库表进行映射，开发者可以灵活地控制SQL语句的编写和执行过程。它不会自动生成SQL语句，因此开发者对SQL的控制更加直接。
> - MyBatis提供了简单易用的参数映射和结果集映射功能，支持动态SQL语句的构建，可以根据条件拼接SQL语句。
> - MyBatis相对于Hibernate而言更加轻量级，它更适用于对SQL语句有较高要求、需要灵活控制的场景。同时，MyBatis也可以与Hibernate集成使用，发挥各自的优势。
>
> 选择使用Hibernate还是MyBatis，取决于具体的应用需求和个人偏好。Hibernate适用于大型、复杂的应用程序，它提供了全面的ORM功能和高级特性。而MyBatis适用于对SQL语句控制要求较高的场景，更加灵活。在实际开发中，也可以根据项目需求，结合使用两个框架，发挥各自的优势。









##### 参考

[MyBatis和Hibernate的区别](http://c.biancheng.net/mybatis/mybatis-hibernate.html)