# shiro jwt redis

*学习一下关于java的登录验证实现方式*

*前言：在微服务中我们一般采用的是无状态登录，而传统的session方式，在前后端分离的微服务架构下，如继续使用则必将要解决跨域sessionId问题、集群session共享问题等等。这显然是费力不讨好的，而整合[shiro](https://so.csdn.net/so/search?q=shiro&spm=1001.2101.3001.7020)，却很不恰巧的与我们的期望有所违背：
　　（1）shiro默认的拦截跳转都是跳转url页面，而前后端分离后，后端并无权干涉页面跳转。
　　（2）shiro默认使用的登录拦截校验机制恰恰就是使用的session。
　　这当然不是我们想要的，因此如需使用shiro，我们就需要对其进行改造，**那么要如何改造呢？**我们可以在整合shiro的基础上自定义登录校验，继续整合JWT，或者oauth2.0等，使其成为支持服务端无状态登录，即token登录。*

##### Spring Boot 实现登录验证都有什么方式

> Spring Boot 是一个用于构建 Java 应用程序的开发框架，提供了许多不同的方式来实现登录验证。以下是一些常见的方式：
>
> 1. 基于 Spring Security：Spring Security 是一个用于认证和授权的强大安全框架，可以轻松地集成到 Spring Boot 应用中。通过配置 Spring Security，可以实现基于用户名和密码的表单登录、基于角色的权限控制、记住我功能、单点登录（SSO）等功能。
>
> 2. 使用第三方身份验证服务：Spring Boot 提供了与许多第三方身份验证服务（如OAuth 2.0、OpenID Connect等）的集成支持，可以通过配置相关的客户端信息来实现登录验证。例如，可以使用 Spring Security OAuth 或者 Spring Security OIDC 来与第三方身份验证服务进行集成，从而实现基于第三方平台的登录验证。
>
> 3. 自定义登录验证逻辑：可以通过实现自定义的登录验证逻辑来实现登录验证。例如，可以编写一个实现了 Spring Security 的 `UserDetailsService` 接口的自定义类，从数据库或其他数据源中加载用户信息，并进行验证。
>
> 4. 使用 JSON Web Token（JWT）：JWT 是一种用于身份验证和授权的轻量级标准，可以用于实现无状态的认证机制。在 Spring Boot 中，可以使用第三方库（如 Spring Security JWT、jjwt 等）来生成和验证 JWT，并将其用于实现基于令牌的登录验证。
>
> 5. 结合 Spring Boot 提供的其他特性：Spring Boot 提供了很多用于处理身份验证和授权的特性，如基于注解的权限控制、过滤器、拦截器等。可以使用这些特性来实现自定义的登录验证逻辑。
>
> 这只是一些实现登录验证的方式，具体的选择取决于项目需求和开发团队的偏好。在实现登录验证时，务必考虑安全性和用户体验，并遵循最佳实践。



##### shiro

> Shiro 是一个用于身份认证、授权和会话管理的强大的 Java 安全框架。Shiro 提供了一套简单易用的 API，用于处理用户身份验证、权限授权和会话管理等安全相关的功能。它是一个灵活且功能丰富的安全解决方案，可以集成到各种类型的 Java 应用程序中，包括传统的 JavaEE 应用程序、Spring Boot 应用程序、Web 应用程序、RESTful 服务和其他类型的应用程序。
>
> Shiro 的主要功能包括：
>
> 1. 身份认证：Shiro 提供了多种身份认证方式，包括基于用户名和密码的表单登录、基于记住我功能的身份认证、基于证书的身份认证、基于多因素身份认证等。
> 2. 权限授权：Shiro 提供了丰富的授权功能，包括基于角色的授权、基于权限字符串的授权、自定义授权策略等。通过 Shiro 的授权功能，可以轻松实现细粒度的权限控制。
> 3. 会话管理：Shiro 提供了会话管理功能，可以管理用户的登录会话，并支持多种会话存储方式，如内存存储、Redis 存储、数据库存储等。Shiro 还支持集群环境下的会话管理，可以实现在多个应用服务器之间共享会话状态。
> 4. 密码加密：Shiro 提供了密码加密和解密的支持，包括多种加密算法，如MD5、SHA、SHA256、SHA512等，可以帮助开发者存储用户密码时进行安全加密。
> 5. Web 支持：Shiro 提供了对 Web 应用程序的特殊支持，包括基于过滤器的身份认证和授权，可以方便地集成到 Java Web 应用程序中。
> 6. 集成支持：Shiro 提供了与其他常见框架（如Spring、Spring Boot、Apache Shiro、Apache Cassandra等）的集成支持，可以方便地与这些框架一起使用，构建更加安全的应用程序。
>
> Shiro 是一个非常灵活和可扩展的安全框架，可以根据项目需求进行自定义配置，并提供了丰富的功能和良好的文档。它被广泛应用于 Java 应用程序的安全领域，是一个受欢迎的选择。



##### jwt

> JWT（JSON Web Token）是一种用于在网络应用之间传递信息的安全跨域身份验证机制。它是一种基于标准 JSON 格式的轻量级身份验证和授权协议，通过在用户和服务器之间传递被签名的令牌来验证用户身份并授权访问。
>
> JWT 由三部分组成：头部（Header）、载荷（Payload）和签名（Signature）。
>
> 1. 头部（Header）：头部通常由两部分组成，即令牌的类型（例如 "JWT"）和使用的签名算法（例如 "HS256" 表示 HMAC SHA-256，或 "RS256" 表示 RSA SHA-256）。
> 2. 载荷（Payload）：载荷是存放实际信息的部分，通常包含一些声明（Claim），例如用户的身份信息、权限等。载荷可以被编码成 JSON 格式，用于传递各种信息。
> 3. 签名（Signature）：签名用于验证发送方是否为可信任的源，并且确保消息在传递过程中没有被篡改。签名通常使用密钥与头部和载荷进行加密生成，接收方在验证时会使用相同的密钥进行解密并验证签名的有效性。
>
> JWT 的工作流程通常如下：
>
> 1. 用户通过认证方式（例如用户名和密码）进行身份验证，服务器验证成功后生成一个 JWT。
> 2. 服务器将生成的 JWT 返回给客户端，客户端将其保存，通常放在请求的 Authorization 头部中或者作为参数放在请求的 URL 中。
> 3. 客户端在后续的请求中携带 JWT，服务器通过验证 JWT 的签名来验证用户身份，并根据 JWT 中的声明进行权限授权。
>
> JWT 具有以下优点：
>
> 1. 简洁：JWT 使用 JSON 格式，具有较小的体积，便于传递和处理。
> 2. 自包含：JWT 包含了用户身份信息和授权信息，避免了服务器在验证时需要查询数据库或其他存储。
> 3. 可扩展：JWT 的载荷部分可以自定义声明，支持扩展功能。
> 4. 无状态：JWT 是无状态的，服务器不需要在后端保存用户的状态信息，使得服务端更加容易扩展和维护。
> 5. 安全：JWT 使用签名进行验证，可以确保消息的完整性和真实性，防止篡改和伪造。
>
> JWT 在很多应用场景中都得到了广泛应用，特别是在分布式系统、微服务架构和前后端分离的应用中，作为一种简单且安全的身份验证和授权机制。



##### shiro 和 spring Security

> Shiro（Apache Shiro）和Spring Security（Spring Boot Security）在功能上有一些相似之处，都是用于实现身份验证和授权的 Java 安全框架，但它们也有一些区别。
>
> Shiro 是一个独立的开源安全框架，提供了一套完整的身份验证、授权、会话管理和加密等安全功能，可以集成到任何 Java 应用中。Shiro 不依赖于任何其他框架，可以独立使用。
>
> Spring Security（或称为 Spring Boot Security）是在 Spring 框架的基础上集成了 Spring Security 框架的安全功能，提供了一种强大而灵活的安全解决方案，可以与 Spring Boot 框架无缝集成。Spring Security 提供了一些针对 Spring 环境的特性，例如可以使用 Spring 的依赖注入（DI）和面向切面编程（AOP）等特性来管理安全配置和使用。
>
> 虽然 Shiro 和 Spring Security 在功能上有很多相似之处，都可以用于实现身份验证和授权，但它们也有一些区别，例如：
>
> 1. 集成方式：Shiro 是一个独立的安全框架，可以在任何 Java 应用中集成和使用；而 Spring Security 是在 Spring 框架的基础上集成的安全框架，专门用于与 Spring 框架无缝集成。
>
> 2. 配置方式：Shiro 使用 ini、XML、注解等方式进行配置；而 Spring Security 使用基于 Java 配置和注解的方式进行配置。
>
> 3. 社区生态：Spring Security 是在 Spring 生态圈内广泛使用的安全框架，有着大量的社区支持和丰富的生态系统；而 Shiro 的社区相对较小，生态系统相对较简单。
>
> 4. 功能特性：Shiro 着重于简单和灵活，提供了一些独特的特性，如支持多种数据源、支持 Web 和非 Web 环境等；而 Spring Security 提供了一些与 Spring 框架集成的特性，如支持 Spring 的依赖注入（DI）和面向切面编程（AOP）等。
>
> 因此，具体选择使用 Shiro 还是 Spring Security 取决于项目需求、技术栈和团队偏好等因素。在使用时，应根据实际情况选择合适的安全框架，并按照官方文档和最佳实践进行配置和使用。





##### shiro jwt 结合

> Shiro 和 JWT 可以结合使用，以充分发挥它们各自的优势。Shiro 是一个功能丰富的 Java 安全框架，提供了身份验证、授权、会话管理等多种安全功能，并且有着广泛的社区支持和生态系统。而 JWT 是一种简单且无状态的身份验证和授权方案，通过 token 自包含的方式携带身份信息，适用于跨越不同应用或服务的身份验证和授权场景。
>
> 结合使用 Shiro 和 JWT 可以获得以下优势：
>
> 1. 无状态身份验证和授权：JWT 的无状态特性可以避免服务器端存储会话信息，从而减轻服务器的状态管理负担，并简化了跨应用或服务的身份验证和授权过程。
>
> 2. 灵活的身份验证和授权方式：Shiro 提供了丰富的身份验证和授权方式，可以根据项目需求进行灵活选择。结合 JWT，可以在需要时使用 JWT 进行身份验证和授权，也可以在其他场景下使用 Shiro 提供的其他身份验证和授权方式。
>
> 3. 可扩展性：Shiro 提供了丰富的扩展点和插件机制，可以根据需求进行扩展和定制。通过结合 JWT，可以实现自定义的身份验证和授权逻辑，以满足项目的特定需求。
>
> 4. 统一的身份验证和授权标准：JWT 是一种开放标准，具有广泛的应用和支持。结合 Shiro，可以将 JWT 作为一种统一的身份验证和授权标准，用于多个应用或服务，从而实现统一的身份验证和授权逻辑。
>
> 5. 弹性的架构设计：结合 Shiro 和 JWT，可以实现弹性的架构设计，使系统可以在不同的认证和授权需求下灵活运行，满足不同的安全要求和业务场景。
>
> 需要注意的是，结合使用 Shiro 和 JWT 时，需要合理设计和配置身份验证和授权逻辑，确保安全性和可靠性。例如，可以使用 Shiro 进行基于角色/权限的身份验证和授权，而使用 JWT 进行跨应用或服务的身份验证和授权。同时，需要妥善处理 JWT 的生成、传递、验证和过期等安全性相关的问题，以确保系统的安全性。



##### Spring Security jwt 结合

> 是的，Spring Security 可以与 JWT 结合使用，以实现身份验证和授权功能。Spring Security 是一个功能强大且广泛使用的安全框架，专门用于在 Java 应用程序中处理身份验证和授权的需求。而 JWT 是一种无状态的身份验证和授权方案，通过 token 自包含的方式携带身份信息，适用于跨应用或服务的身份验证和授权场景。
>
> 结合使用 Spring Security 和 JWT 可以获得以下优势：
>
> 1. 强大的身份验证和授权功能：Spring Security 提供了丰富的身份验证和授权功能，包括支持多种身份验证方式、基于角色和权限的授权、表达式语言等功能。结合 JWT，可以在身份验证过程中使用 JWT 进行身份验证，从而实现强大的身份验证和授权功能。
>
> 2. 无状态身份验证和授权：JWT 的无状态特性可以避免服务器端存储会话信息，从而减轻服务器的状态管理负担，并简化了跨应用或服务的身份验证和授权过程。这与 Spring Security 提供的传统的基于会话的身份验证和授权方式相比，更加灵活和适用于分布式环境。
>
> 3. 灵活的配置和扩展：Spring Security 提供了丰富的配置和扩展点，可以根据需求进行定制和扩展。结合 JWT，可以在 Spring Security 的配置中集成 JWT 相关的配置，例如设置 JWT 的签名密钥、Token 的有效期等，从而实现灵活的身份验证和授权逻辑。
>
> 4. 可与其他 Spring 生态系统集成：Spring Security 是 Spring 生态系统中的一部分，可以与其他 Spring 相关技术（如 Spring MVC、Spring Boot 等）无缝集成。结合 JWT，可以实现与其他 Spring 技术的集成，例如在 Spring Boot 应用中使用 JWT 进行身份验证和授权。
>
> 需要注意的是，结合使用 Spring Security 和 JWT 时，需要合理设计和配置身份验证和授权逻辑，确保安全性和可靠性。例如，需要妥善处理 JWT 的生成、传递、验证和过期等安全性相关的问题，以确保系统的安全性。同时，建议遵循最佳实践和安全建议，以保护应用程序免受安全攻击。





### shiro jwt redis 教程

##### response.setHeader("Access-Control-Expose-Headers", "Authorization");

> 这段Java代码设置了HTTP响应头的"Access-Control-Expose-Headers"字段的值为"Authorization"，用于解决跨域资源共享（CORS）的安全限制问题。
>
> 在Web开发中，当在一个域名的网页中通过AJAX请求访问另一个域名的资源时，浏览器会执行CORS安全限制，限制从不同域名下获取敏感信息。"Access-Control-Expose-Headers"字段用于定义服务器端响应中可以暴露给客户端的自定义HTTP头信息。
>
> 通过将"Access-Control-Expose-Headers"字段设置为"Authorization"，服务器端可以指示浏览器在接收到响应后，将"Authorization"头信息暴露给客户端，从而使得客户端可以在AJAX请求中获取到该头信息。这对于需要在请求头中携带身份验证令牌（如JWT令牌）的应用程序非常有用，因为浏览器默认情况下不会暴露自定义头信息，需要通过设置"Access-Control-Expose-Headers"字段来明确指示浏览器将其暴露给客户端。

##### Enumeration\<String> headerNames = request.getHeaderNames()；

> 这段Java代码用于获取HTTP请求中的所有头（header）的名称（name）。
>
> 在Java Servlet开发中，HttpServletRequest对象表示HTTP请求，可以通过该对象获取客户端发送的HTTP请求的各种信息，包括请求头。其中，`request.getHeaderNames()`方法返回一个Enumeration（枚举）类型的对象，该对象包含了所有请求头的名称。
>
> 上述代码将通过`request`对象调用`getHeaderNames()`方法，返回一个`Enumeration<String>`类型的对象，其中`String`表示请求头的名称。可以通过循环遍历该`Enumeration`对象，获取请求中的所有头的名称，例如：
>
> ```java
> Enumeration<String> headerNames = request.getHeaderNames();
> while (headerNames.hasMoreElements()) {
>     String headerName = headerNames.nextElement();
>     // 对每个请求头的名称进行处理
>     System.out.println("Header Name: " + headerName);
> }
> ```
>
> 这样可以逐个获取请求头的名称，并进行相应的处理，例如读取请求头的值，进行认证、授权、日志记录等操作。

如果直接获取Authorization，如果http请求头中没有，可能会空指针



##### String token = SecurityUtils.getSubject().getPrincipal().toString();

> 这段Java代码使用 Apache Shiro 框架来获取当前已认证的用户（主体）的身份信息。
>
> Apache Shiro 是一个用于身份验证（Authentication）、授权（Authorization）和会话管理（Session Management）的Java安全框架。在这段代码中，使用了 Shiro 的一些 API 来获取当前已认证的用户的身份信息。
>
> 解释如下：
>
> 1. `SecurityUtils` 是 Shiro 框架提供的工具类，用于获取安全相关的信息和执行安全操作。
> 2. `getSubject()` 是 `SecurityUtils` 类的方法，用于获取当前执行的主体（Subject）对象，代表了当前已认证的用户。
> 3. `getPrincipal()` 是 `Subject` 对象的方法，用于获取主体的身份信息，通常是用户的标识符，如用户名、用户ID等。
> 4. `toString()` 是将获取到的主体身份信息转换为字符串。
>
> 最终，这段代码将获取到的主体身份信息转换为字符串，并赋值给 `token` 变量，用于后续的处理，例如用于生成认证令牌、进行业务逻辑的处理等。需要注意的是，这段代码假定当前用户已经通过 Shiro 的认证流程成功认证，并且主体已经存在于当前的会话中。





















##### 参考

[springboot+shiro+jwt+redis+cache实现无状态token登录（完美好文章！！）](https://blog.csdn.net/hd243608836/article/details/115860131)
