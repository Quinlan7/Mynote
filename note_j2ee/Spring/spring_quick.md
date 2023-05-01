[spring学习-快速入门](https://www.cnblogs.com/wmyskxz/p/8820371.html)

[java全栈知识体系](https://pdai.tech/md/spring/spring-x-framework-ioc.html#ioc-%E9%85%8D%E7%BD%AE%E7%9A%84%E4%B8%89%E7%A7%8D%E6%96%B9%E5%BC%8F)



## Spring依赖注入(DI)

Spring依赖注入是一种设计模式，它允许对象相互协作，以实现更大的目标。在依赖注入中，对象不会自己创建或管理它们的依赖关系。相反，这些依赖关系由第三方（通常是一个框架）创建和管理。Spring框架提供了一种依赖注入的实现方式，它可以将对象之间的依赖关系从代码中分离出来，并将其配置在XML文件或Java注解中。这样，当需要更改依赖关系时，只需修改配置文件或注解即可，而不必修改代码本身。

Spring提供了三种依赖注入方式：构造函数注入、Setter方法注入和字段注入。其中，构造函数注入和Setter方法注入是最常用的两种方式。构造函数注入是通过构造函数来传递依赖项的方式，而Setter方法注入是通过Setter方法来传递依赖项的方式。字段注入是一种不太常用的方式，它是通过直接设置字段值来传递依赖项的方式。





##### @configuration

[@Configuration是Spring框架中的一个注解，用于定义配置类。被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。@Configuration类也可以使用@Component注解进行标注，这样就可以通过类路径扫描自动侦测到它们，而不需要显式地使用BeanDefinitionReader加载它们。](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)[1](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)[2](https://blog.csdn.net/leoxyk/article/details/79800020)

[@Configuration类是一个特殊的@Component类，它除了允许通过@Bean方法定义bean之外，还提供了额外的功能](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)[1](https://spring.pleiades.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/Configuration.html)[。@Configuration类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。@Configuration类也可以使用@Component注解进行标注，这样就可以通过类路径扫描自动侦测到它们，而不需要显式地使用BeanDefinitionReader加载它们。](https://blog.csdn.net/leoxyk/article/details/79800020)[2](https://blog.csdn.net/leoxyk/article/details/79800020)

[@Configuration注解的作用是告诉Spring容器这个类是一个配置类，相当于以前Spring配置文件中的标签。@Configuration注解也可以被@ComponentScan注解所扫描到](https://blog.csdn.net/leoxyk/article/details/79800020)[2](https://blog.csdn.net/leoxyk/article/details/79800020)。





##### @Component

[@Component是Spring框架中的一个注解，它的作用是将一个类标识为Spring容器中的一个组件，让Spring容器自动扫描并管理这个类。使用@Component注解后，我们就可以使用@Autowired注解来自动装配这个类的对象了。@Component注解是Spring框架中最基本的注解之一，其他的一些注解如@Service、@Repository、@Controller等都是由@Component派生而来的，它们之间的区别在于语义上的不同。](https://www.baeldung.com/spring-component-annotation)[1](https://www.baeldung.com/spring-component-annotation)[2](https://www.javaguides.net/2018/11/spring-component-annotation-example.html)[3](https://qiita.com/KevinFQ/items/abc7369cb07eb4b9ae29)[4](https://blog.csdn.net/qq_42000661/article/details/107413696)



##### @Component  vs @configuration

[@Component和@Configuration注解都是Spring框架中的注解，用于标识一个类为Bean，但是它们有一些不同之处。@Component注解的范围最广，所有类都可以注解，但是@Configuration注解一般注解在这样的类上：这个类里面有@Value注解的成员变量和@Bean注解的方法，就是一个配置类。@Configuration中所有带@Bean注解的方法都会被动态代理（cglib），因此调用该方法返回的都是同一个实例。而@Component修饰的类不会被代理，每实例化一次就会创建一个新的对象。](https://zhidao.baidu.com/question/1308741575419218499.html)[1](https://zhidao.baidu.com/question/1308741575419218499.html)[2](https://blog.csdn.net/weixin_52850476/article/details/124410800)[3](https://blog.csdn.net/weixin_44119329/article/details/122247029)[4](https://blog.csdn.net/isea533/article/details/78072133)

在实际应用中，@Component注解可以用于标识任何类，而@Configuration注解则用于标识一个类为配置类。如果你需要在一个类中定义@Bean方法，那么这个类就应该被@Configuration注解标识。如果你只是想要标识一个普通的Bean，那么就可以使用@Component注解。



##### 关于注解扫描器

你可以在Spring中使用[context:component-scan](context:component-scan)元素来配置注解扫描器。这个元素可以用来指定要扫描的包，以及要排除的包。例如，下面的配置将会扫描com.example包及其子包，但是排除com.example.dao包及其子包：

```xml
<context:component-scan base-package="com.example">
    <context:exclude-filter type="regex" expression="com\.example\.dao\..*"/>
</context:component-scan>
```

你也可以使用JavaConfig来配置注解扫描器。例如，下面的配置将会扫描com.example包及其子包：

```java
@Configuration
@ComponentScan(basePackages = "com.example")
public class AppConfig {
}
```



+ 如果我们不配置注解扫描器，那么spring或者springboot会自动扫描吗

> 是的，Spring和Spring Boot都会自动扫描类路径下的所有类，但是只有被标记为@Component或其他注解的类才会被注册为Bean。如果你没有使用注解扫描器，那么你需要在XML配置文件中手动注册Bean。





##### 注解配置IOC

通过在类上加注解的方式，来声明一个类交给Spring管理，Spring会自动扫描带有@Component，@Controller，@Service，@Repository这四个注解的类，然后帮我们创建并管理，前提是需要先配置Spring的注解扫描器。

------

著作权归@pdai所有 原文链接：https://pdai.tech/md/spring/spring-x-framework-ioc.html





##### @Mapper

> @Mapper是MyBatis框架中的注解，用于标记一个接口，表示这个接口是一个Mapper。Mapper是一种数据访问层的设计模式，它负责与数据库进行交互，完成数据的增删改查等操作。在MyBatis中，我们可以使用@Mapper注解来标记一个接口，然后在XML文件中编写SQL语句，通过MyBatis框架自动实现这个接口。

+ @Mapper会被Spring自动扫描并注册bean吗

> 是的，@Mapper注解会被Spring自动扫描并注册bean。在Spring Boot中，我们可以使用@MapperScan注解来扫描Mapper接口所在的包，然后自动注册这些接口为Bean。例如：
>
> ```java
> @MapperScan("com.example.mapper")
> @SpringBootApplication
> public class Application {
>     public static void main(String[] args) {
>         SpringApplication.run(Application.class, args);
>     }
> }
> ```



+ 如果不配置@MapperScan注解，会被自动扫描吗

> 如果不配置@MapperScan注解，@Mapper接口不会被自动扫描并注册为Bean。如果你没有使用@MapperScan注解，你需要在每个Mapper接口上添加@Mapper注解，或者在XML文件中手动配置这些接口。例如：
>
> ```java
> @Mapper
> public interface UserMapper {
>     List<User> selectAll();
> }
> ```



+ 那么如果每个接口都配置了@Mapper就不用配置@MapperScan注解了吧

> 是的，如果每个接口都配置了@Mapper注解，就不需要在启动类上配置@MapperScan注解了。但是这样做比较麻烦，因为你需要在每个接口上都添加@Mapper注解。所以，使用@MapperScan注解可以更方便地扫描所有的Mapper接口并注册为Bean。





##### @Autowired

> 在Spring Boot中，@Autowired是一个用于进行依赖注入（Dependency Injection）的注解，它用于将Spring容器中的Bean自动注入到需要它的地方。
>
> 依赖注入是一种设计模式，用于解耦组件之间的依赖关系，从而实现更松散的耦合和更高的可测试性、可维护性。在Spring Boot中，@Autowired注解可以用于自动注入Spring容器中的Bean，从而避免了显式地在代码中创建和管理Bean实例。
>
> 具体而言，@Autowired注解可以用于以下场景：
>
> 1. 自动注入依赖：在需要使用某个Bean的地方，通过在该类的属性、构造方法或者方法参数上添加@Autowired注解，Spring容器会自动将对应类型的Bean实例注入到这些地方。
> 2. 自动注入配置值：@Autowired注解也可以用于注入Spring Boot应用程序的配置值，例如application.properties或application.yml中的配置项。通过在属性上添加@Value注解，可以实现将配置值自动注入到属性中。
> 3. 高级依赖注入：@Autowired注解还支持更高级的依赖注入方式，例如通过@Qualifier注解指定具体的Bean名称、通过@Primary注解指定首选的Bean、通过@Lazy注解实现延迟加载等。
>
> 使用@Autowired注解可以方便地在Spring Boot应用程序中进行依赖注入，简化了Bean的创建和管理，提高了代码的可维护性和可测试性。需要注意的是，使用@Autowired注解时，需要确保目标Bean已经在Spring容器中注册并且可以被自动注入，否则会导致注入失败或者出现运行时异常。

+ 使用在方法上

> 在Spring Boot中，@Autowired注解还可以用于方法上，它的作用是将Spring容器中的Bean自动注入到方法的参数中。
>
> 具体而言，当在方法上添加@Autowired注解时，Spring容器会尝试将与方法参数类型匹配的Bean实例注入到该参数中，从而实现对Bean的自动注入。这可以使得方法内部可以直接使用被注入的Bean，无需显式地从Spring容器中获取。
>
> 方法级别的@Autowired注解常用于以下场景：
>
> 1. 在配置类中使用：当在Spring Boot的配置类中的方法上添加@Autowired注解时，Spring容器会自动将与方法参数类型匹配的Bean注入到该方法参数中，从而可以在配置类中方便地使用其他Bean。
> 2. 在普通的Bean中使用：在普通的Bean类中，如果某个方法需要依赖其他Bean实例，可以通过@Autowired注解将这些依赖自动注入到方法参数中，从而方便地在方法中使用这些依赖。
>
> 需要注意的是，当在方法上使用@Autowired注解时，Spring容器会根据参数类型进行匹配，如果存在多个与参数类型匹配的Bean，可以使用@Qualifier注解或者通过参数名称与Bean名称进行匹配，以明确指定要注入的Bean实例。
>
> 以下是一个使用@Autowired注解在方法上的示例：
>
> ```java
> @Component
> public class FooService {
> 
>     private final BarService barService;
> 
>     @Autowired
>     public FooService(BarService barService) {
>         this.barService = barService;
>     }
> 
>     public void doSomething() {
>         // 使用自动注入的barService实例
>         // ...
>     }
> }
> ```
>
> 在上面的示例中，通过在构造方法上添加@Autowired注解，将BarService注入到FooService中的barService属性中，从而可以在FooService中使用自动注入的barService实例。

+ 使用在方法上时

+ ```java
  private static JedisPool jedisPool;  
  
  @Autowired    
  public void setJedisPool(JedisPool jedisPool) {        						JedisUtil.jedisPool = jedisPool;    
  }
  ```

+ 这段代码中的静态变量什么时候初始化

> 在这段代码中，静态变量`jedisPool`会在调用`setJedisPool`方法时进行初始化。
>
> `@Autowired`注解通常用于实现Spring的依赖注入功能，用于自动装配Spring容器中的Bean到类的属性上。在这段代码中，`setJedisPool`方法被标注了`@Autowired`注解，表示这个方法会在Spring容器启动时自动调用，并将一个`JedisPool`类型的Bean注入到`jedisPool`属性中。
>
> 当Spring容器启动时，会扫描并创建所有的Bean，包括`JedisPool`的Bean。当`JedisPool`的Bean创建完成后，Spring会自动调用`setJedisPool`方法，并将`JedisPool`的Bean作为参数传递给该方法，从而完成对`jedisPool`静态变量的初始化。
>
> 需要注意的是，静态变量的初始化时机取决于类的加载和首次使用。在这段代码中，静态变量`jedisPool`会在调用`setJedisPool`方法时进行初始化，而`setJedisPool`方法通常在Spring容器启动时被调用。因此，静态变量`jedisPool`会在Spring容器启动时进行初始化，以保证在项目运行过程中可以使用已经初始化的`JedisPool`实例。

[@Autowired注解作用在方法上](https://blog.csdn.net/qq_43842093/article/details/120477648)









##### @Resource

> @Resource注解是Java层面提供的注解，它在语义上被定义为通过其唯一的名称来标识特定的目标组件，其中声明的类型与匹配过程无关。如果没有指定名称，则默认名称从字段名称或setter方法派生。@Resource注解与@Autowired注解类似，也是用来进行依赖注入的。但是，@Autowired注解是Spring所提供的注解，它基于类型type进行依赖注入，而@Resource注解是Java层面所提供的注解，它基于类的名称进行依赖注入。





##### @Service

> @Service注解是Spring框架中用于标注服务层组件的注解，它通常用于将Java类标记为服务层组件，以便Spring容器可以识别和管理这些组件。@Service注解可以带有以下参数：
>
> 1. value：指定服务层组件的名称。可以通过value属性为服务层组件指定一个自定义的名称，如果不指定，默认会根据类名生成一个首字母小写的名称作为bean的名称。例如：@Service(value = "myService")。
>
> 2. name：与value属性功能相同，用于指定服务层组件的名称。例如：@Service(name = "myService")。
>
> 3. description：描述服务层组件的简要说明。可以通过description属性为服务层组件添加一个简要的描述信息。例如：@Service(description = "This is a sample service component.")。
>
> 4. autowire：指定服务层组件的自动装配模式。可以通过autowire属性指定服务层组件的自动装配模式，包括：`Autwired.AUTO`（默认值，表示自动装配所有依赖）、`Autwired.BY_TYPE`（根据类型自动装配）、`Autwired.BY_NAME`（根据名称自动装配）和`Autwired.NO`（不自动装配）。
>
> 5. proxyTargetClass：是否使用CGLIB代理。可以通过proxyTargetClass属性指定是否使用CGLIB（Code Generation Library）代理，默认为false，表示使用JDK动态代理。当需要代理的目标对象没有实现接口时，需要将proxyTargetClass属性设置为true，以使用CGLIB代理。
>
> 6. value和name属性的作用相同，都可以用于指定服务层组件的名称，二者可以互相替代使用。例如：@Service(value = "myService")和@Service(name = "myService")效果相同。
>
> 示例用法：
> ```java
> @Service(value = "myService")
> public class MyService {
>     // 服务层组件的实现代码
> }
> ```
>
> ```java
> @Service(name = "myService", description = "This is a sample service component.", autowire = Autowired.BY_TYPE, proxyTargetClass = true)
> public class MyService {
>     // 服务层组件的实现代码
> }
> ```
>
> 在使用@Service注解时，可以根据实际需求设置不同的参数来配置服务层组件的行为和属性。



+ @Service("ss")这是什么意思

> @Service("ss") 是 Spring 框架中使用 @Service 注解为服务层组件指定了一个自定义的名称 "ss"。这样在 Spring 容器中创建该服务层组件的实例时，会将其注册到容器中，并使用 "ss" 作为该实例的 bean 名称。
>
> @Service 注解通常用于标记服务层组件，表示该类负责业务逻辑的处理，例如数据处理、事务管理等。通过为 @Service 注解提供一个名称，可以自定义服务层组件在 Spring 容器中的 bean 名称，从而可以通过该名称在容器中获取对应的实例。
>
> 例如，下面的示例中，@Service 注解为 MyService 类指定了自定义的名称 "ss"：
>
> ```java
> @Service("ss")
> public class MyService {
>     // 服务层组件的实现代码
> }
> ```
>
> 这将在 Spring 容器中创建一个名为 "ss" 的 bean，表示 MyService 类的实例。可以通过该名称在容器中获取 MyService 实例，例如：
>
> ```java
> @Autowired
> @Qualifier("ss")
> private MyService myService;
> ```
>
> 这里通过 @Autowired 注解和 @Qualifier 注解指定了要注入的 bean 名称为 "ss"，从而将 Spring 容器中的 "ss" bean 注入到 myService 字段中。





##### @Qualifier

> @Qualifier 是 Spring 框架中用于解决依赖注入（DI）中多个同类型 bean 的歧义性问题的注解。当容器中存在多个同类型的 bean，并且需要注入到某个类的字段或方法参数中时，可以使用 @Qualifier 注解来明确指定要注入的 bean。
>
> @Qualifier 注解可以与 @Autowired 或 @Resource 注解一同使用，用于标识要注入的 bean 的名称或标识符，以便 Spring 容器能够正确选择要注入的 bean。
>
> 使用 @Qualifier 注解时，可以指定一个自定义的字符串值作为 bean 的名称或标识符，用于标识要注入的特定 bean。例如：
>
> ```java
> @Autowired
> @Qualifier("myBean")
> private MyInterface myInterface;
> ```
>
> 这里使用了 @Autowired 注解和 @Qualifier 注解，指定了要注入的 bean 的名称为 "myBean"，并将其注入到 myInterface 字段中。
>
> 需要注意的是，@Qualifier 注解一般与 @Autowired 或 @Resource 注解一同使用，用于解决多个同类型 bean 的歧义性问题。当容器中不存在与 @Qualifier 注解中指定的名称或标识符匹配的 bean 时，会抛出 NoSuchBeanDefinitionException 异常。因此，在使用 @Qualifier 注解时，需要确保容器中存在与指定的名称或标识符匹配的 bean。



##### @Bean

> 在Spring Boot中，@Bean注解是用于定义Bean的注解，它通常用于将一个方法标记为创建并注册一个Bean实例的方法。
>
> 具体而言，@Bean注解的作用有以下几个方面：
>
> 1. 创建Bean实例：通过在一个方法上添加@Bean注解，将该方法标记为创建并返回一个Bean实例的方法。这个方法可以包含一些自定义的逻辑，例如创建Bean实例、配置Bean的属性、初始化Bean等。
> 2. 注册Bean实例：被@Bean注解标记的方法会在Spring容器启动时自动被调用，生成一个Bean实例并注册到Spring容器中，从而可以在应用程序中通过@Autowired等方式进行自动注入和使用。
> 3. 自定义Bean的属性：@Bean注解还可以用于自定义Bean的属性，例如通过@Bean注解的initMethod和destroyMethod属性指定Bean的初始化方法和销毁方法，通过@Bean注解的name属性指定Bean的名称等。
> 4. 配置外部依赖：在一些场景下，Spring Boot应用程序需要与外部依赖进行集成，例如第三方库、自定义组件等。通过在配置类中定义带有@Bean注解的方法，可以将这些外部依赖注入到Spring容器中，并进行统一的配置和管理。
>
> 以下是一个使用@Bean注解的示例：
>
> ```java
> @Configuration
> public class AppConfig {
> 
>     @Bean
>     public FooService fooService() {
>         return new FooService();
>     }
> 
>     @Bean
>     public BarService barService() {
>         return new BarService(fooService());
>     }
> 
>     @Bean(initMethod = "init", destroyMethod = "cleanup")
>     public BazService bazService() {
>         return new BazService();
>     }
> }
> ```
>
> 在上面的示例中，通过在@Configuration注解标记的配置类中定义了三个带有@Bean注解的方法，分别用于创建FooService、BarService和BazService的实例，并注册到Spring容器中。这样，这些Bean实例就可以在应用程序中进行自动注入和使用了。同时，通过@Bean注解的initMethod和destroyMethod属性，还指定了BazService的初始化方法和销毁方法。











+ @Bean    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();        redisTemplate.setConnectionFactory(redisConnectionFactory);        // 配置序列化器等其他属性        return redisTemplate;    }这段代码中的RedisConnectionFactory 对象如何注入

> 在Spring Boot中，通过使用`@Bean`注解来创建`RedisConnectionFactory`的实例，并将其注册到Spring容器中，从而使其可以被其他组件自动注入。
>
> 在上面的示例中，`RedisConnectionFactory`的实例会通过构造函数注入到`redisTemplate`方法中。Spring会自动查找容器中是否存在`RedisConnectionFactory`的实例，如果存在，则会将其注入到`redisTemplate`方法中作为参数。
>
> 要确保`RedisConnectionFactory`实例被创建并注册到Spring容器中，可以在应用程序的配置类（如上面的示例中的`RedisConfig`类）中使用`@Bean`注解来创建`RedisConnectionFactory`的实例，并在其他组件中使用`@Autowired`注解来注入该实例。例如：
>
> ```java
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> import org.springframework.data.redis.connection.RedisConnectionFactory;
> import org.springframework.data.redis.core.RedisTemplate;
> 
> @Configuration
> public class RedisConfig {
> 
>     // ...其他配置...
> 
>     @Bean
>     public RedisConnectionFactory redisConnectionFactory() {
>         // 创建并配置RedisConnectionFactory实例
>         // 可以使用JedisConnectionFactory或LettuceConnectionFactory等
>         // 具体根据使用的Redis客户端选择
>         return new JedisConnectionFactory();
>     }
> 
>     @Bean
>     public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
>         RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
>         redisTemplate.setConnectionFactory(redisConnectionFactory);
>         // 配置序列化器等其他属性
>         return redisTemplate;
>     }
> }
> ```
>
> 上面的示例中，`redisConnectionFactory()`方法使用`@Bean`注解来创建`RedisConnectionFactory`的实例，并将其注册到Spring容器中。在`redisTemplate()`方法中，通过方法参数`RedisConnectionFactory`来注入该实例，从而实现了`RedisConnectionFactory`对象的注入。





> Spring Boot 在进行方法参数自动注入时，并不会自动调用类的构造方法进行对象的创建。Spring Boot 的依赖注入是通过依赖注入容器（如Spring容器）来管理和创建对象的，而不是通过调用类的构造方法来创建对象。
>
> 当使用方法参数自动注入时，Spring Boot 会尝试在容器中查找与方法参数类型匹配的Bean，并将其注入到方法参数中。如果找到了匹配的Bean，则直接将该Bean注入到方法参数中，而不会调用该类的构造方法。
>
> 这意味着，即使类的构造方法被定义为私有的或者没有无参构造方法，Spring Boot 依然可以成功进行方法参数自动注入，只要在容器中存在与方法参数类型匹配的Bean。
>
> 需要注意的是，Spring Boot 在进行方法参数自动注入时，会使用默认的构造方法（无参构造方法）进行对象的创建，因此如果需要使用带有参数的构造方法来创建对象，需要手动进行配置，例如使用`@Autowired`和`@Qualifier`等注解来指定使用特定的构造方法。





> 这句话的意思是，当 Spring Boot 进行方法参数的自动注入时，如果找不到相匹配的 Bean，Spring Boot 会尝试使用默认的构造方法（无参构造方法）来创建对象，并将该对象注入到方法参数中。
>
> 在进行方法参数自动注入时，Spring Boot 会根据参数类型去容器中查找相匹配的 Bean。如果找不到匹配的 Bean，Spring Boot 会尝试通过默认的构造方法来创建对象，然后将该对象注入到方法参数中。
>
> 需要注意的是，如果目标类没有提供默认的构造方法，或者默认的构造方法不可访问（如私有构造方法），则 Spring Boot 在进行方法参数自动注入时会抛出异常。因此，如果希望使用默认的构造方法进行对象的创建，需要确保目标类有默认的构造方法，并且该构造方法是可访问的。如果目标类没有默认的构造方法或者希望使用其他构造方法进行对象的创建，可以通过其他方式进行处理，如使用 `@Autowired` 注解的 `required` 属性设置为 `false`，或者使用 `@Nullable` 注解或者 `Optional` 类型作为方法参数。





##### @ConfigurationProperties(prefix = "jwt")这个注解是什么意思

> `@ConfigurationProperties(prefix = "jwt")` 是一个在 Spring 框架中用于绑定配置属性的注解。它主要用于将配置文件中的属性值绑定到 Java 类的字段上，从而方便地在应用程序中使用这些属性值。
>
> 具体而言，`@ConfigurationProperties` 注解用于将配置文件中以指定前缀（`prefix`）开头的属性值，绑定到被注解的类的字段上。在这里，`prefix` 参数指定了配置属性的前缀，例如 `"jwt"`，表示只绑定配置文件中以 `"jwt"` 作为前缀的属性。
>
> 例如，如果在配置文件（如 `application.properties` 或 `application.yml`）中有以下属性：
>
> ```properties
> jwt.secretKey=mySecretKey
> jwt.expirationTime=3600
> ```
>
> 可以使用 `@ConfigurationProperties` 注解将这些属性值绑定到一个 Java 类的字段上，如下所示：
>
> ```java
> @ConfigurationProperties(prefix = "jwt")
> public class JwtProperties {
>     private String secretKey;
>     private int expirationTime;
> 
>     // 省略构造函数、getter 和 setter 方法
> }
> ```
>
> 在这个例子中，`@ConfigurationProperties(prefix = "jwt")` 表示将配置文件中以 `"jwt"` 作为前缀的属性值绑定到 `JwtProperties` 类的字段上。例如，`jwt.secretKey` 的值将会被绑定到 `secretKey` 字段上，`jwt.expirationTime` 的值将会被绑定到 `expirationTime` 字段上。
>
> 一旦完成配置属性值的绑定，就可以在应用程序中直接使用 `JwtProperties` 类的实例来访问这些属性值，例如：
>
> ```java
> @Autowired
> private JwtProperties jwtProperties;
> 
> // 使用 jwtProperties 获取配置属性值
> String secretKey = jwtProperties.getSecretKey();
> int expirationTime = jwtProperties.getExpirationTime();
> ```
>
> 这样，就可以方便地在应用程序中使用配置文件中的属性值，并避免了硬编码属性值的问题。



##### JwtProperties这个类中的属性需要set方法吗？

需要！
