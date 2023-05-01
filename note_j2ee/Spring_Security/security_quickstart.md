# Spring Security Quick Start

##### @PreAuthorize

> `@PreAuthorize` 注解是 Spring Security 框架中的一个注解，用于在方法级别进行访问控制。它定义了在方法被调用之前对用户的权限进行检查的行为。在 Spring Security 中，`@PreAuthorize` 注解是在 `org.springframework.security.access.prepost` 包中定义的。
>
> 具体来说，`@PreAuthorize` 注解的定义位于 `org.springframework.security.access.prepost.PreAuthorize` 类中。该类是 Spring Security 提供的预授权注解之一，用于在方法调用之前进行权限检查，允许或拒绝用户访问方法。`@PreAuthorize` 注解可以在方法级别使用，用于对方法的访问权限进行控制，可以结合 Spring Security 提供的表达式语言，如 SpEL（Spring Expression Language）来定义权限规则。
>
> 以下是 `@PreAuthorize` 注解的定义示例：
>
> ```java
> package org.springframework.security.access.prepost;
> 
> @Target({ElementType.METHOD, ElementType.TYPE})
> @Retention(RetentionPolicy.RUNTIME)
> @Inherited
> @Documented
> @Repeatable(PreAuthorizeContainer.class)
> public @interface PreAuthorize {
>     String value();
> }
> ```
>
> 需要注意的是，`@PreAuthorize` 注解通常与其他 Spring Security 注解一同使用，如 `@Secured`、`@RolesAllowed` 等，用于在应用程序中实现细粒度的访问控制和权限管理。







##### @PreAuthorize("@ss.hasPermi('system:user:list')")

> `@PreAuthorize("@ss.hasPermi('system:user:list')")` 是一个基于 Spring Security 框架的注解，用于在方法调用之前进行权限检查。具体而言，这个注解使用了 Spring Security 提供的表达式语言（SpEL）来定义权限规则。
>
> 解析该注解的意思如下：
>
> - `@PreAuthorize`：这是一个在方法调用之前进行权限检查的注解，用于对方法的访问权限进行控制。
> - `"@ss.hasPermi('system:user:list')"`：这是一个 SpEL 表达式，用于定义权限规则。在这个例子中，它调用了名为 `hasPermi` 的方法，并传递了 `'system:user:list'` 作为参数。`hasPermi` 方法可能是自定义的，用于检查当前用户是否具有执行 `'system:user:list'` 操作的权限。
>
> 这种用法通常用于在方法调用之前对用户的权限进行检查，确保只有具有相应权限的用户才能调用该方法。在这个例子中，`@PreAuthorize` 注解使用了自定义的权限检查逻辑 `hasPermi`，该逻辑可能是基于业务需求自行实现的，用于检查用户是否具有执行 `'system:user:list'` 操作的权限。如果用户没有足够的权限，将会在方法调用之前抛出 Spring Security 异常，从而阻止方法的执行。





##### SpEL（Spring Expression Language）



##### 禁用Spring secruity

> `spring.security.enabled=false` 是在 Spring Boot 项目中用于禁用 Spring Security 的配置属性。Spring Security 是一个用于在 Java 应用中处理认证和授权的框架，用于保护应用程序的安全性。
>
> 通过将 `spring.security.enabled` 设置为 `false`，您可以在 Spring Boot 项目中禁用 Spring Security 功能，从而不会应用任何与认证和授权相关的安全配置。这对于不需要认证和授权的简单项目或者测试环境中可能是有用的。
>
> 在禁用 Spring Security 后，您的应用程序将不再进行任何安全验证，包括用户认证、角色授权、访问控制等。这意味着任何用户都可以访问应用程序的所有资源，包括受限的资源，因此在生产环境中使用时需谨慎。
>
> 需要注意的是，禁用 Spring Security 可能会导致您的应用程序在安全性方面存在风险。在实际项目中，通常建议根据应用程序的安全需求配置和使用 Spring Security，以确保应用程序的安全性和防护。





