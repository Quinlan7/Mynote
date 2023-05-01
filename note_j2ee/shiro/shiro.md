##### springboot项目中的shiro中的realm是什么

> Realm（领域）是 Apache Shiro 框架中的一个核心概念，用于处理认证（Authentication）和授权（Authorization）的逻辑。在 Spring Boot 项目中使用 Shiro 时，Realm 负责从数据源中获取用户的身份信息和权限信息。
>
> 具体来说，Realm 是一个接口，开发者需要自己实现该接口，提供具体的认证和授权逻辑。Realm 可以从不同的数据源中获取用户的身份信息和权限信息，例如数据库、LDAP、文件等。通过在 Shiro 配置中指定使用的 Realm，Shiro 可以根据配置的 Realm 来进行用户身份认证和权限授权的操作。
>
> 在 Shiro 中，Realm 的主要作用有两个：
>
> 1. 认证（Authentication）：Realm 负责验证用户的身份信息，例如用户名和密码是否匹配等。如果验证成功，Realm 将返回一个代表用户身份信息的 AuthenticationInfo 对象，用于后续的认证操作。
> 2. 授权（Authorization）：Realm 负责获取用户的权限信息，例如用户所拥有的角色、权限等。Shiro 使用这些权限信息来进行权限控制的判断，从而实现对用户访问资源的授权控制。
>
> 在 Spring Boot 项目中，可以通过配置 Shiro 的 Realm 实现对用户身份认证和权限授权的定制化，从而满足项目的安全需求。不同的 Realm 实现可以根据项目的具体情况，从不同的数据源中获取用户身份信息和权限信息，例如数据库、LDAP、文件等。







[Shiro安全框架【快速入门】就这一篇！](https://www.cnblogs.com/wmyskxz/p/10229148.html)