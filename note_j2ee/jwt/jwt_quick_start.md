# JWT

*使用JWT直接支持跨域，并且不会被CSRF*



+ jwt存在放在http请求的header中还是cookie中

> JWT可以被放置在HTTP请求的两个常见位置：请求头（Header）和Cookie。
>
> 1. 放置在请求头（Header）中：
>    JWT通常被放置在请求头的Authorization字段中。具体来说，使用Bearer方案将JWT放置在Authorization字段的值中，格式如下：
>    
>    ```
>    Authorization: Bearer <JWT>
>    ```
>
>    这种方式是JWT在Web应用程序中最常见和推荐的用法。将JWT放置在请求头中可以更好地隔离身份验证和应用程序数据，并且可以更灵活地处理其他类型的身份验证。
>
> 2. 放置在Cookie中：
>    另一种常见的方式是将JWT存储在Cookie中。服务器可以将JWT作为名为"jwt"或其他自定义名称的Cookie发送给客户端。客户端在后续请求中会自动将该Cookie包含在HTTP头中发送给服务器。
>
>    ```
>    Set-Cookie: jwt=<JWT>; Path=/; Expires=<expiration_date>; HttpOnly; Secure
>    ```
>
>    这种方式适用于需要在每个请求中自动发送JWT的情况，而不需要在每个请求的Authorization头中显式添加它。
>
> 无论将JWT放置在请求头还是Cookie中，都需要采取适当的安全措施来保护JWT的机密性和完整性。例如，使用HTTPS协议来加密通信以防止JWT被窃取或篡改。此外，在将JWT存储在Cookie中时，应设置相关的安全标志，如HttpOnly和Secure，以防止跨站点脚本攻击（XSS）和跨站点请求伪造（CSRF）。











##### 参考

[jwt详解](https://blog.csdn.net/weixin_45070175/article/details/118559272?spm=1001.2014.3001.5506)