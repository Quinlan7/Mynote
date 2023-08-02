### Java连接Redis服务

*Java通过jedis连接redis服务*

[Redis——Java操作Redis、Jedis连接池、使用Redis缓存不常修改的数据](https://blog.csdn.net/m0_37989980/article/details/107447560)

##### jedis连接池

> Jedis连接池是一个用于管理Jedis客户端连接的Java对象池，用于提高Redis数据库连接的性能和效率。
>
> Jedis是Java语言中一个流行的用于与Redis数据库进行通信的客户端库。在使用Jedis客户端与Redis服务器进行交互时，每次与服务器建立连接都需要进行网络连接、认证等操作，这会导致较大的性能开销。为了减少这种性能开销，并提高与Redis服务器的通信效率，可以使用Jedis连接池来管理连接。
>
> Jedis连接池通常使用第三方库，例如Jedis Pool、commons-pool2等，这些库提供了连接池的实现，可以在应用程序中创建和维护一定数量的Jedis连接，并对连接进行管理，包括连接的创建、连接的复用、连接的回收、连接的定期验证等。
>
> 使用Jedis连接池的好处包括：
>
> 1. 节省连接的创建和销毁开销：连接池可以在应用程序启动时创建一定数量的连接，并在需要时从连接池中获取可用的连接，避免了频繁创建和销毁连接的性能开销。
> 2. 提高连接的复用率：连接池可以对连接进行复用，避免了每次请求都需要创建新的连接，从而提高了连接的复用率和性能。
> 3. 控制连接的数量：连接池可以设置最大连接数、最小连接数等参数，用于控制应用程序与Redis服务器之间的连接数量，防止连接过多导致性能下降。
> 4. 提供连接的管理和监控：连接池可以对连接进行管理和监控，包括连接的状态、连接的空闲时间、连接的验证等，从而提供更好的连接管理和监控功能。
> 5. 支持连接的自动恢复：连接池可以对异常的连接进行自动恢复，从而提高了应用程序对于Redis服务器连接的健壮性和可靠性。
>
> 使用Jedis连接池可以帮助开发者更好地管理和优化与Redis服务器之间的连接，提高应用程序的性能和稳定性。

[SpringBoot连接Redis操作](https://juejin.cn/post/7055259096282316836)

[springboot读取配置文件中的属性并实现自动注入](https://juejin.cn/post/7056048786401034277)

+ 为什么要自定义redisTemplate

[**自定义redisTemplate的方法**](https://juejin.cn/post/7063690856573698062)





##### springboot redistemplate 如何和我们的redis数据库连接上

> 在Spring Boot中，通过使用RedisTemplate可以与Redis数据库建立连接并进行操作。下面是一些在Spring Boot中使用RedisTemplate连接到Redis数据库的步骤：
>
> 1. 添加Redis依赖：在Spring Boot的项目中，首先需要在pom.xml文件中添加Redis的依赖。例如，可以添加以下依赖到pom.xml文件中：
>
> ```xml
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-data-redis</artifactId>
> </dependency>
> ```
>
> 2. 配置Redis连接信息：在Spring Boot的配置文件（例如application.properties或application.yml）中，配置Redis的连接信息，包括主机名、端口号、密码等。例如：
>
> ```properties
> spring.redis.host=127.0.0.1
> spring.redis.port=6379
> spring.redis.password=
> ```
>
> 3. 创建RedisTemplate实例：在Spring Boot的Java配置类中，可以通过@Bean注解创建一个RedisTemplate实例，并配置其属性，如下所示：
>
> ```java
> @Configuration
> public class RedisConfig {
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
> 在上面的示例中，通过@Bean注解创建了一个名为redisTemplate的RedisTemplate实例，并注入了一个RedisConnectionFactory实例，用于连接到Redis数据库。可以在此方法中对RedisTemplate进行配置，例如设置序列化器、连接池、事务等。
>
> 4. 使用RedisTemplate进行操作：通过@Autowired注解将RedisTemplate注入到需要使用Redis的组件中，然后可以使用RedisTemplate提供的方法进行对Redis数据库的操作，例如存储、读取、删除等。
>
> 以下是一个简单的示例，演示了如何在Spring Boot中使用RedisTemplate连接到Redis数据库并进行操作：
>
> ```java
> @Autowired
> private RedisTemplate<String, Object> redisTemplate;
> 
> public void setValue(String key, Object value) {
>     redisTemplate.opsForValue().set(key, value);
> }
> 
> public Object getValue(String key) {
>     return redisTemplate.opsForValue().get(key);
> }
> 
> public void deleteKey(String key) {
>     redisTemplate.delete(key);
> }
> ```
>
> 在上面的示例中，通过@Autowired注解将RedisTemplate注入到一个组件中，然后可以通过调用RedisTemplate的方法进行对Redis数据库的操作，例如通过opsForValue()方法获取字符串类型的操作对象，然后调用set()方法进行键值对的存储。





##### 如何设置RedisTemplate的连接池

> 在Spring Boot中，可以通过配置RedisConnectionFactory来设置RedisTemplate的连接池。以下是一个示例：
>
> ```java
> import org.springframework.beans.factory.annotation.Value;
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
> import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
> import org.springframework.data.redis.core.RedisTemplate;
> import redis.clients.jedis.JedisPoolConfig;
> 
> @Configuration
> public class RedisConfig {
> 
>  @Value("${spring.redis.host}")
>  private String host;
> 
>  @Value("${spring.redis.port}")
>  private int port;
> 
>  @Value("${spring.redis.password}")
>  private String password;
> 
>  @Value("${spring.redis.database}")
>  private int database;
> 
>  @Value("${spring.redis.pool.max-total}")
>  private int maxTotal;
> 
>  @Value("${spring.redis.pool.max-idle}")
>  private int maxIdle;
> 
>  @Value("${spring.redis.pool.min-idle}")
>  private int minIdle;
> 
>  @Bean
>  public JedisPoolConfig jedisPoolConfig() {
>      JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
>      jedisPoolConfig.setMaxTotal(maxTotal);
>      jedisPoolConfig.setMaxIdle(maxIdle);
>      jedisPoolConfig.setMinIdle(minIdle);
>      return jedisPoolConfig;
>  }
> 
>  @Bean
>  public JedisConnectionFactory jedisConnectionFactory(JedisPoolConfig jedisPoolConfig) {
>      RedisStandaloneConfiguration config = new RedisStandaloneConfiguration();
>      config.setHostName(host);
>      config.setPort(port);
>      config.setPassword(password);
>      config.setDatabase(database);
>      return new JedisConnectionFactory(config, jedisPoolConfig);
>  }
> 
>  @Bean
>  public RedisTemplate<String, Object> redisTemplate(JedisConnectionFactory jedisConnectionFactory) {
>      RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
>      redisTemplate.setConnectionFactory(jedisConnectionFactory);
>      // 配置序列化器等其他属性
>      return redisTemplate;
>  }
> }
> ```
>
> 在上面的示例中，使用了JedisConnectionFactory来配置连接池，并将其注入到RedisTemplate中。其中，通过JedisPoolConfig来设置连接池的属性，包括最大连接数（maxTotal）、最大空闲连接数（maxIdle）和最小空闲连接数（minIdle）等。同时，也可以通过RedisStandaloneConfiguration来配置Redis连接的基本信息，包括主机名（host）、端口号（port）、密码（password）和数据库索引（database）等。
>
> 注意：以上示例是使用Jedis作为Redis客户端的示例，如果使用Lettuce作为Redis客户端，可以替换JedisConnectionFactory为LettuceConnectionFactory，并使用LettucePoolConfig来配置连接池属性。





##### RedisTemplate 与 Spring Data Redis有什么关系

> `RedisTemplate`是Spring Data Redis模块中提供的一个类，用于简化Java程序与Redis进行交互的操作。它封装了底层的Redis连接和操作，并提供了一系列便捷的API，使得在Spring应用中使用Redis变得更加方便和高效。
>
> Spring Data Redis是Spring框架的一个模块，用于与Redis进行集成，提供了一套基于Spring的简化Redis操作的API。它通过封装底层的Java客户端库（如Jedis、Lettuce等），提供了统一的编程模型，支持各种常见的Redis操作，如设置、获取、删除、更新键值对、操作列表、集合、有序集合等，并且支持事务、数据序列化、缓存注解驱动等高级功能。
>
> `RedisTemplate`是Spring Data Redis模块中的一个关键类，它提供了一个操作Redis的入口，通过依赖注入的方式，可以在Spring应用中方便地使用`RedisTemplate`来进行Redis操作。`RedisTemplate`封装了连接池、序列化、错误处理等细节，提供了一系列常用的操作方法，如`opsForValue()`用于操作字符串类型的值，`opsForList()`用于操作列表类型，`opsForSet()`用于操作集合类型，以此类推。
>
> `RedisTemplate`可以通过配置来选择底层的Java客户端库，例如可以配置使用Jedis或者Lettuce作为底层的Redis连接库。此外，`RedisTemplate`还支持自定义的序列化和反序列化器，以便将Java对象与Redis中的数据进行转换。通过使用`RedisTemplate`，可以在Spring应用中轻松地进行Redis操作，同时充分发挥了Spring Data Redis提供的高级功能。





##### springboot redisConnectionFactory对象怎么自动装配

> 在Spring Boot中，`RedisConnectionFactory`对象可以通过以下步骤进行自动装配：
>
> 1. 添加Redis相关的依赖：在项目的`pom.xml`文件中添加Spring Data Redis的依赖，例如：
>
> ```xml
> <dependency>
>     <groupId>org.springframework.boot</groupId>
>     <artifactId>spring-boot-starter-data-redis</artifactId>
> </dependency>
> ```
>
> 2. 配置Redis连接信息：在项目的`application.properties`或`application.yml`配置文件中配置Redis的连接信息，包括主机、端口、密码等。例如：
>
> ```properties
> spring.redis.host=127.0.0.1
> spring.redis.port=6379
> spring.redis.password=your_password
> ```
>
> 3. 创建Redis连接工厂：在Java代码中使用`RedisConnectionFactory`进行Redis连接工厂的创建，并通过`@Autowired`注解将其自动注入到需要使用的类中。例如：
>
> ```java
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.data.redis.connection.RedisConnectionFactory;
> 
> // ...
> 
> @Autowired
> private RedisConnectionFactory redisConnectionFactory;
> ```
>
> 通过以上步骤，Spring Boot会自动根据配置文件中的Redis连接信息创建`RedisConnectionFactory`对象，并将其自动注入到使用`@Autowired`注解标记的字段中。这样，您就可以在项目中直接使用`redisConnectionFactory`对象来进行Redis操作，如创建`RedisTemplate`等。如果配置信息正确，并且相关的Redis依赖已添加，Spring Boot会自动创建并注入`RedisConnectionFactory`对象，简化了Redis连接的配置和管理。



##### 如果配置文件中存在redis的连接池配置，RedisConnectionFactory可以自动读取配置吗

> 是的，`RedisConnectionFactory`可以自动读取Spring Boot配置文件中的连接池配置。
>
> 在Spring Boot的配置文件（`application.properties`或`application.yml`）中，可以配置各种与Redis连接池相关的属性，例如最大连接数、最小空闲连接数、连接池大小等。这些属性可以通过以下方式进行配置：
>
> ```properties
> # 使用Jedis连接池的配置
> spring.redis.jedis.pool.max-active=100
> spring.redis.jedis.pool.max-idle=20
> spring.redis.jedis.pool.min-idle=5
> 
> # 使用Lettuce连接池的配置
> spring.redis.lettuce.pool.max-active=100
> spring.redis.lettuce.pool.max-idle=20
> spring.redis.lettuce.pool.min-idle=5
> ```
>
> 在Java代码中使用`RedisConnectionFactory`时，Spring Boot会根据配置文件中的连接池配置自动创建对应的连接池，并将其应用到`RedisConnectionFactory`中。例如，如果使用Jedis连接池，Spring Boot会根据`spring.redis.jedis.pool.*`属性的配置创建`JedisPool`，然后将其设置到`RedisConnectionFactory`中；如果使用Lettuce连接池，Spring Boot会根据`spring.redis.lettuce.pool.*`属性的配置创建`LettucePoolingClientConfiguration`，然后将其设置到`RedisConnectionFactory`中。这样，`RedisConnectionFactory`对象就可以自动根据配置文件中的连接池配置创建对应的连接池，无需手动编写连接池的配置和管理。







### Jedis                                                                     

> Jedis（发音为“jed-is”）是Java语言中一个常用的用于与Redis进行交互的客户端库。它提供了一组简单且易于使用的API，用于在Java应用程序中进行Redis的操作，包括数据的读取、写入、删除等。
>
> 以下是Jedis的一些特点和功能：
>
> 1. 简单易用：Jedis提供了简单且直观的API，方便Java开发者进行与Redis的交互。它支持常用的Redis操作，如字符串、哈希、列表、集合、有序集合等的操作。
>
> 2. 完全覆盖的Redis命令：Jedis支持Redis的所有命令，包括字符串操作、哈希操作、列表操作、集合操作、有序集合操作、发布/订阅、事务、管道等。
>
> 3. 高性能：Jedis采用了连接池的方式管理与Redis的连接，可以高效地与Redis服务器进行通信，提供较好的性能。
>
> 4. 支持多线程：Jedis实例是线程安全的，可以在多线程环境下并发使用。
>
> 5. 支持多种序列化方式：Jedis支持多种数据序列化方式，如Java的默认序列化、JSON序列化、二进制序列化等。
>
> 6. 内置事务支持：Jedis支持Redis的事务操作，可以通过multi()、exec()、discard()等方法进行事务操作。
>
> 7. 支持发布/订阅模式：Jedis支持Redis的发布/订阅模式，可以通过pubSub()方法进行发布消息和订阅消息。
>
> 8. 提供了丰富的扩展功能：Jedis提供了丰富的扩展功能，如连接池配置、连接超时设置、数据序列化配置等。
>
> 需要注意的是，Jedis是一个比较底层的Redis客户端库，对于一些高级特性如Redis集群、Sentinel、哨兵模式等可能需要额外的配置和处理。在使用Jedis时，应根据具体的业务需求和Redis服务器的配置进行相应的设置和处理。



### Lettuce介绍

> Lettuce 是一个基于 Java 的开源 Redis 客户端库，用于与 Redis 数据库进行交互。它提供了一组功能丰富、高性能且可扩展的 API，用于在 Java 应用程序中进行 Redis 操作，包括数据的读取、写入、删除等。
>
> 以下是 Lettuce 的一些特点和功能：
>
> 1. 异步和响应式：Lettuce 提供了异步和响应式的 API，可以在高并发环境下提供更好的性能和资源利用率。它支持使用 CompletableFuture、Reactive Streams、RxJava 等方式进行异步操作。
>
> 2. 高性能：Lettuce 使用 NIO（非阻塞 I/O）模型，支持高并发、高吞吐量的操作，可以与 Redis 服务器进行快速、高效的通信。
>
> 3. 完全覆盖的 Redis 命令：Lettuce 支持 Redis 的所有命令，包括字符串操作、哈希操作、列表操作、集合操作、有序集合操作、发布/订阅、事务、管道等。
>
> 4. 简单易用：Lettuce 提供了简单且直观的 API，方便 Java 开发者进行与 Redis 的交互。它支持多种数据序列化方式，如 Java 的默认序列化、JSON 序列化、二进制序列化等。
>
> 5. 支持多线程：Lettuce 实例是线程安全的，可以在多线程环境下并发使用。
>
> 6. 支持连接池和连接状态监控：Lettuce 提供了连接池的功能，可以对连接进行管理和复用，并支持连接状态监控和自动重连。
>
> 7. 支持 Redis 集群和 Sentinel：Lettuce 支持 Redis 集群和 Sentinel（哨兵）模式，可以通过配置连接参数来实现对 Redis 集群和 Sentinel 的操作。
>
> 8. 提供了丰富的扩展功能：Lettuce 提供了丰富的扩展功能，如连接池配置、连接超时设置、数据序列化配置、SSL 加密支持等。
>
> 需要注意的是，Lettuce 是一个现代化的 Redis 客户端，提供了异步和响应式的特性，适合在高并发、高性能的应用场景中使用。在使用 Lettuce 时，应根据具体的业务需求和 Redis 服务器的配置进行相应的设置和处理。





### RedisTemplate

这个是Spring提供的封装了Jedis和Lettuce的工具，封装了底层的这两个连接工具，使用RedisTemplate时可以选择这两个中的任意一个进行连接。并且可以设置序列化方式。RedisConnectionFactory对象可以自动读取配置文件中的redis相关配置，进行自动装配。





### Redis缓存

##### 作用

在高并发的业务场景下，数据库大多数情况都是用户并发访问最薄弱的环节。所以，就需要使用redis做一个缓冲操作，让请求先访问到redis，而不是直接访问Mysql等数据库。这样可以大大缓解数据库的压力。

> Redis 缓存是一种将数据存储在 Redis 数据库中，以便在需要时能够快速访问和检索数据的技术。它可以将一部分或全部数据存储在 Redis 数据库中，从而减轻后端数据库的负载，提高系统性能。
>
> Redis 缓存通常用于加速对经常访问、计算成本较高的数据的读取操作，如数据库查询、API 调用等。通过将这些数据存储在 Redis 数据库中，并设置适当的缓存策略，可以在需要时快速从 Redis 缓存中获取数据，而不必每次都从后端数据库或其他数据源中读取，从而显著减少了响应时间和资源消耗。
>
> Redis 缓存支持多种数据类型，包括字符串、哈希、列表、集合、有序集合等。它还提供了丰富的缓存功能，如过期时间、缓存穿透处理、缓存更新策略等，以满足不同场景下的缓存需求。
>
> 使用 Redis 缓存可以带来很多优势，如提高系统性能、降低后端数据库负载、减少响应时间、提高用户体验等。但需要注意的是，缓存的使用需要慎重考虑，确保缓存数据的一致性和安全性，并根据业务需求合理设置缓存策略，避免缓存数据过期或者失效导致的数据不一致或错误。

[Redis实现缓存](https://tobebetterjavaer.com/redis/redis-springboot.html#%E7%BC%96%E7%A8%8B%E5%96%B5%E6%95%B4%E5%90%88-redis)





##### Redis缓存只是针对使用缓存注解的Redis方法而言的

*其实redis本身就是一个高速缓存数据库，他存储的数据就是作为缓存的，但是这是针对我们显示调用redis存储的数据而言的。我们显示配置的redis缓存管理配置，是针对方法缓存而言的，都是使用注解的缓存方法*

> Redis是一款内存高速缓存数据库。Redis全称为：**Remote Dictionary Server**（远程数据服务），使用C语言编写，Redis是一个key-value存储系统（键值存储系统），支持丰富的数据类型，如：String、list、set、zset、hash。





##### RedisTemplate自动配置了redis缓存吗

> 在Spring Boot中，RedisTemplate并不会自动配置Redis缓存。RedisTemplate是Spring提供的用于操作Redis数据库的工具类，它提供了一系列的方法用于对Redis进行数据的存储、查询、删除等操作。
>
> 然而，要在Spring Boot中使用Redis作为缓存，需要进行额外的配置。Spring Boot提供了自动配置的方式，可以通过在应用程序的配置文件（如application.properties或application.yml）中进行相应的配置，来启用Redis作为缓存。
>
> 以下是一个示例的Spring Boot配置文件中使用Redis作为缓存的配置：
>
> ```properties
> # Redis配置
> spring.redis.host=127.0.0.1 # Redis服务器的主机名或IP地址
> spring.redis.port=6379 # Redis服务器的端口号
> spring.redis.password= # Redis服务器的连接密码
> spring.redis.database=0 # 使用的Redis数据库索引，默认为0
> 
> # 启用Redis作为缓存
> spring.cache.type=redis
> 
> # 其他缓存相关配置
> spring.cache.cache-names=myCache # 缓存名称
> spring.cache.redis.time-to-live=60000 # 缓存过期时间，单位毫秒
> ```
>
> 在以上配置中，通过设置spring.cache.type为redis，启用了Redis作为缓存。可以通过其他缓存相关的配置项来设置缓存的一些属性，例如缓存的名称、缓存的过期时间等。
>
> 需要注意的是，虽然RedisTemplate是用于操作Redis数据库的工具类，但它并不会自动配置Redis缓存。要在Spring Boot中启用Redis作为缓存，需要进行额外的配置，并通过相应的配置项来设置缓存的相关属性。





##### redis缓存如何使用呢

> 在Spring Boot中使用Redis作为缓存，可以通过以下步骤进行：
>
> 步骤 1：添加Redis依赖
> 在项目的pom.xml文件中添加Spring Boot对Redis的依赖，例如：
>
> ```xml
> <dependency>
>  <groupId>org.springframework.boot</groupId>
>  <artifactId>spring-boot-starter-data-redis</artifactId>
> </dependency>
> ```
>
> 步骤 2：配置Redis连接信息
> 在应用程序的配置文件（如application.properties或application.yml）中配置Redis的连接信息，例如：
>
> ```properties
> # Redis配置
> spring.redis.host=127.0.0.1 # Redis服务器的主机名或IP地址
> spring.redis.port=6379 # Redis服务器的端口号
> spring.redis.password= # Redis服务器的连接密码
> spring.redis.database=0 # 使用的Redis数据库索引，默认为0
> ```
>
> 步骤 3：使用Redis作为缓存
> 在需要使用缓存的地方，可以通过Spring Boot提供的缓存注解来使用Redis作为缓存。例如，可以使用`@Cacheable`注解来启用缓存，并指定缓存的名称、缓存的键、缓存的值等。例如：
>
> ```java
> @Service
> public class MyService {
> 
>  @Cacheable(value = "myCache", key = "#key")
>  public String getData(String key) {
>      // 从数据库或其他数据源获取数据
>      // ...
>  }
> 
> }
> ```
>
> 在上面的示例中，`@Cacheable`注解指定了缓存的名称为"myCache"，缓存的键为传入的`key`参数。当调用`getData()`方法时，如果缓存中已经存在对应的键值对，那么缓存中的值将被直接返回，而不会执行方法体中的逻辑。如果缓存中不存在对应的键值对，那么方法体中的逻辑将被执行，并将结果存入缓存中，以便下次使用。
>
> 除了`@Cacheable`注解外，还有其他一些缓存注解可用于不同的缓存操作，例如`@CachePut`用于更新缓存，`@CacheEvict`用于删除缓存，`@Caching`用于组合多个缓存操作等。
>
> 需要注意的是，使用Redis作为缓存时，缓存的键值对将存储在Redis中，因此在使用缓存时需要考虑缓存的有效期、缓存的命名规则等。此外，Spring Boot还提供了对缓存的高级配置，例如配置缓存的过期时间、配置缓存的序列化方式等，可以根据需要进行相应的配置。