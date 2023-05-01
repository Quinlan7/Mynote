# Listerner

*监听器*

### 简介

> 在Java EE（现在被称为Jakarta EE）中，监听器（Listener）是一种用于监听Web应用程序中事件的组件。监听器可以监听各种事件，例如Web应用程序的启动和关闭、HTTP请求和响应的创建和销毁、Session的创建和销毁等。
>
> 监听器通过实现Java EE规范中定义的相应监听器接口来实现。以下是一些常见的Java EE监听器接口：
>
> 1. ServletContextListener（javax.servlet.ServletContextListener）：用于监听Web应用程序的启动和关闭事件。可以在Web应用程序启动时执行一些初始化操作，或在Web应用程序关闭时执行一些清理操作。
> 2. ServletRequestListener（javax.servlet.ServletRequestListener）：用于监听HTTP请求和响应的创建和销毁事件。可以在请求创建时执行一些预处理操作，或在请求销毁时执行一些后处理操作。
> 3. HttpSessionListener（javax.servlet.http.HttpSessionListener）：用于监听Session的创建和销毁事件。可以在Session创建时执行一些初始化操作，或在Session销毁时执行一些清理操作。
> 4. ServletContextAttributeListener（javax.servlet.ServletContextAttributeListener）：用于监听ServletContext属性的变化事件。可以在ServletContext属性被添加、删除或修改时执行相应的操作。
> 5. ServletRequestAttributeListener（javax.servlet.ServletRequestAttributeListener）：用于监听HTTP请求属性的变化事件。可以在请求属性被添加、删除或修改时执行相应的操作。
> 6. HttpSessionAttributeListener（javax.servlet.http.HttpSessionAttributeListener）：用于监听Session属性的变化事件。可以在Session属性被添加、删除或修改时执行相应的操作。
>
> 通过监听器，开发人员可以在Web应用程序的不同阶段执行一些自定义的逻辑，例如在应用程序启动时初始化一些资源、在请求到达时进行身份验证、在Session创建时执行一些初始化操作等。监听器是一种强大的机制，可以在Web应用程序中实现更加灵活和复杂的逻辑。





### SpringBoot 中的监听器

> 是的，Spring Boot框架中使用了监听器（Listener）。
>
> 在Spring Boot中，监听器是一种事件处理机制，用于在应用程序运行期间监听和响应特定的事件。Spring Boot提供了一些内置的监听器，也支持自定义监听器。
>
> Spring Boot内置了一些监听器，例如：
>
> 1. ApplicationStartedEvent：应用程序启动时触发的事件。
> 2. ApplicationEnvironmentPreparedEvent：应用程序环境准备时触发的事件，可以在此事件中对环境进行配置。
> 3. ApplicationPreparedEvent：应用程序准备时触发的事件，可以在此事件中进行应用程序的初始化操作。
> 4. ApplicationReadyEvent：应用程序准备完成时触发的事件，表示应用程序已经完全启动并可以接受请求。
> 5. ApplicationFailedEvent：应用程序启动失败时触发的事件。
>
> 除了内置的监听器，Spring Boot还支持自定义监听器，通过实现ApplicationListener接口或使用@EventListener注解来定义自己的监听器，然后在Spring Boot应用程序中注册这些监听器。
>
> 监听器可以用于处理各种场景，例如在应用程序启动时进行初始化操作、在应用程序关闭时进行资源清理、在特定事件发生时触发某些逻辑等。监听器可以与其他Spring Boot的功能模块（如过滤器、拦截器、控制器等）一起使用，以实现全面的应用程序事件处理和业务逻辑。



+ 这些监听器的具体作用

> 这些监听器在Spring Boot应用程序中可以用于不同的场景和目的，具体用途如下：
>
> 1. ApplicationStartedEvent：应用程序启动时触发的事件，可以用于在应用程序启动后执行一些初始化操作，例如加载配置、初始化数据库连接池、启动定时任务等。
>
> 2. ApplicationEnvironmentPreparedEvent：应用程序环境准备时触发的事件，可以用于在应用程序启动前对环境进行配置，例如设置系统属性、加载外部配置文件、设置环境变量等。
>
> 3. ApplicationPreparedEvent：应用程序准备时触发的事件，可以用于在应用程序启动前进行应用程序的初始化操作，例如初始化缓存、初始化数据源、注册自定义Bean等。
>
> 4. ApplicationReadyEvent：应用程序准备完成时触发的事件，表示应用程序已经完全启动并可以接受请求，可以用于执行应用程序启动后的一些操作，例如加载缓存数据、初始化后台任务、发送通知等。
>
> 5. ApplicationFailedEvent：应用程序启动失败时触发的事件，可以用于处理应用程序启动过程中的错误情况，例如记录错误日志、发送报警通知等。
>
> 这些监听器提供了一种在Spring Boot应用程序中监听和响应不同事件的方式，通过在监听器中编写相应的逻辑，可以实现在不同的应用程序生命周期阶段执行自定义的业务逻辑，例如初始化、配置、资源加载、错误处理等。监听器在Spring Boot中具有灵活性和可扩展性，可以根据实际需求进行定制化的处理。