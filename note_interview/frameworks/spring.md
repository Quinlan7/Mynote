# Spring

[TOC]



## 一、Spring

### 1.1 IOC

#### 1.1.1 对于Spring IOC的理解

IoC（Inverse of Control:控制反转） 是⼀种设计思想。就是将原本在程序中手动创建对象的控制权，交由 IOC 容器来管理，从而实现对象创建和对象使用的解耦。

好处：实现对象创建的集中管理。业务代码专注于业务，不用关注对象的创建时候复杂的依赖关系。

**底层实现**

工厂+反射来实现

#### 1.1.2 单例Bean的线程安全问题

Spring中的单例Bean不是线程安全的。 

因为单例Bean，是全局只有一个Bean，所有线程共享。如果说单例Bean，是一个无状态的，也就是线程中的操作不会对Bean中的成员变量执行查询以外的操作，那么 这个单例Bean是线程安全的。比如Spring mvc 的 Controller、Service、Dao等，这些 Bean大多是无状态的，只关注于方法本身。 假如这个Bean是有状态的，也就是会对Bean中的成员变量进行写操作，那么可能就 存在线程安全的问题。

**解决方法**

1. 在 Bean 中尽量避免定义可变的成员变量。 
2. 在类中定义⼀个 ThreadLocal 成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐）。



#### 1.1.3 Bean的生命周期

![image-20240722220759240](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407222207439.png)

大致分为五个步骤：实例化，属性赋值，初始化，使用，销毁

1. 实例化：首先会通过一个叫做BeanDefinition获取bean的定义信息，调用==推断==构造函数，创建一个对象实例。（延伸:==推断构造方法==）
2. 属性赋值：将实例中相关的依赖进行注入，这里可能涉及到**循环依赖问题**（延伸：循环依赖问题）
3. 初始化：判断函数是否实现了初始化的接口或者实现了`initMethod`方法（具体名称），若是有则执行。
   1. 比如初始化前：会判断是否实现了对应的`XXawear`回调接口（`beanNameAware，BeanFactoryAware`等），实现了则执行，主要功能是将bean名称、bean的类加载器这些对象注入到我们的Bean中（如果我们的Bean需要用到这些对象）。
   2. BeanPostProcessor的before()方法回调
   3. 初始化接口（三种）（`initializingBean`等），执行对应的方法。
   4. BeanPostProcessor的after()方法回调，**这里会实现动态代理**（延伸：动态代理）
4. 使用
5. 最后一步销毁，销毁前也会判断是否实现了对应的销毁接口（`disposableBean`）或销毁的方法（des）。实现了则执行。

> Spring Bean的三种初始化方法
>
> + Bean 类实现 `InitializingBean` 接口，并覆盖 `afterPropertiesSet` 方法。
> + 使用 `@PostConstruct` 注解标记的方法将在 bean 初始化之后自动调用。
> + 在 Spring 的 XML 配置文件中，通过 `init-method` 属性指定初始化方法。



> **1. Spring Bean 实例化的基本流程**
>
> + 加载xml配置文件，解析获取配置中的每个的信息，封装成一个个的BeanDefinition对象;
> + 将BeanDefinition存储在一个名为beanDefinitionMap的Map中;
> + ApplicationContext底层遍历beanDefinitionMap，创建Bean实例对象;
> + 创建好的Bean实例对象，被存储到一个名为singletonObjects的Map中;
> + 当执行applicationContext.getBean(beanName)时，从singletonObjects去匹配Bean实例返回
>
> ![image-20240722171437615](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407221714733.png)
>
> **2. Spring 后处理器**
>
> Spring的后处理器是Spring对外开发的重要扩展点，允许我们介入到Bean的整个实例化流程中来，以达到动态注册 BeanDefinition，动态修改BeanDefinition，以及动态修改Bean的作用。Spring主要有两种后处理器： 
>
> + BeanFactoryPostProcessor：Bean工厂后处理器，在BeanDefinitionMap填充完毕，Bean实例化之前执行；
> + BeanPostProcessor：Bean后处理器，一般在Bean实例化之后，填充到单例池singletonObjects之前执行。
>
> 
>
> **2.1 Bean工厂后处理器 – BeanFactoryPostProcessor**
>
> BeanFactoryPostProcessor是一个接口规范，实现了该接口的类只要交由Spring容器管理的话，那么Spring就会回 调该接口的方法，用于对BeanDefinition注册和修改的功能。
>
> ![image-20240722173452103](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407221734236.png)
>
> 
>
> **Component实现原理**
>
> + 在BeanFactoryPostProcessor接口的实现中，扫描指定路径下的所有类，并且找到使用了@Component注解的所有类，返回他们的全类名
> + 然后在BeanFactoryPostProcessor中将这些类全部注册到BeanDefinition中，最终被Spring管理。 应用启动过程中就会根据BeanDefinition来创建 Bean 实例。
>
> 
>
> **2.2 Bean后处理器 – BeanPostProcessor**
>
> Bean被实例化后，到最终缓存到名为singletonObjects单例池之前，中间会经过Bean的初始化过程，例如：属性的 填充、初始方法init的执行等，其中有一个对外进行扩展的点BeanPostProcessor，我们称为Bean后处理。跟上面的 Bean工厂后处理器相似，它也是一个接口，实现了该接口并被容器管理的BeanPostProcessor，会在流程节点上被 Spring自动调用。
>
> **这里就是实现AOP的地方**
>
> 通过 BeanPostProcessor 我们可以对 初始化完成的Bean 做增强，通过对Bean动态代理，返回 Proxy 对象，到 Bean 工厂中。
>
> ![image-20240722193843031](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407221938122.png)
>
> ==动态代理==
>
> ![image-20240722193941979](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407221939058.png)
>
> 

#### 1.1.4 循环依赖

多个对象相互依赖，形成了一个环。A对象中依赖B，B对象又依赖A。循环依赖在spring中是允许存在，spring框架依据三级缓存已经解决了大部分的循环依赖。

**三层缓存机制**

三级缓存：

1. 一级缓存：缓存走完整个创建流程的单例对象`singletonObjects `
2. 二级缓存：用于存放早期暴露的Bean，用于保存实例化完成的 bean 实例`earlySingletonObjects`
3. 三级缓存：用于存放BeanFactory，可以生成代理对象或原始对象。`singletonFactories`

步骤：

1. 先实例A对象，同时会创建ObjectFactory对象存入三级缓存singletonFactories  
2. A在初始化的时候需要B对象，这个走B的创建的逻辑
3. B实例化完成，也会创建ObjectFactory对象存入三级缓存singletonFactories  
4. B需要注入A，通过三级缓存中获取ObjectFactory来生成一个A的对象同时存入二级缓存，这个是有两种情况，一个是可能是A的普通对象，另外一个是A的代理对象，都可以让ObjectFactory来生产对应的对象，这也是三级缓存的关键
5. B通过从通过二级缓存earlySingletonObjects  获得到A的对象后可以正常注入，B创建成功，存入一级缓存singletonObjects  
6. 回到A对象初始化，因为B对象已经创建完成，则可以直接注入B，A创建成功存入一次缓存singletonObjects 
7. 二级缓存中的临时对象A清除 

**为什么不能二层缓存**

如果只存在两层缓存的话；

1. 若是将A实例化后的对象放入二级缓存，那么最后注入B的依赖不是我们期望的A动态代理后对象。而是普通对象。
2. 若是将A实例化后，提前动态代理后再放入二级缓存，那么完全违背了spring最初bean生命周期创建过程（动态代理在初始化的后置处理器的方法中实现`postProcessAfterInitialization`）

**三层缓存不能解决的情况**

在构造方法中依赖其他类造成的循环依赖。（在初始化的时候就必须调用构造函数进行属性注入）

解决方法：可以使用@Lazy懒加载，什么时候需要对象再进行bean对象的创建



#### 1.1.5 @Autowired 和 @Resource 的区别是什么？

+ @Autowired 是 Spring 提供的注解， @Resource 是 JDK 提供的注解。 
+ Autowired 默认的注入方式为 byType （根据类型进行匹配）， @Resource 默认注入方式为 byName （根据名称进行匹配）。 
+ 当⼀个接口存在多个实现类的情况下， @Autowired 和 @Resource 都需要通过名称才能正确匹 配到对应的 Bean。 Autowired 可以通过 @Qualifier 注解来显示指定名称， @Resource 可以通 过 name 属性来显示指定名称。
+ 依赖注入的用法支持不同：**@Autowired 既支持构造方法注入，又支持属性注入和setter注入**，而**@Resource只支持属性注入和Setter注入**。



#### 1.1.6 Bean的作用域

+ singleton : IoC 容器中只有唯⼀的 bean 实例。Spring 中的 bean 默认都是单例的，是对单例设 计模式的应⽤。 
+ prototype : 每次获取都会创建⼀个新的 bean 实例。也就是说，连续 getBean() 两次，得到的 是不同的 Bean 实例。 
+ request （仅 Web 应⽤可⽤）: 每⼀次 HTTP 请求都会产⽣⼀个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。 
+ session （仅 Web 应⽤可⽤） : 每⼀次来⾃新 session 的 HTTP 请求都会产⽣⼀个新的 bean （会话 bean），该 bean 仅在当前 HTTP session 内有效。 
+ application/global-session （仅 Web 应⽤可⽤）： 每个 Web 应⽤在启动时创建⼀个 Bean （应⽤ Bean），，该 bean 仅在当前应⽤启动时间内有效。 
+ websocket （仅 Web 应⽤可⽤）：每⼀次 WebSocket 会话产⽣⼀个新的 bean。







### 1.2 AOP

#### 1.2.1 对于 Spring AOP 的理解

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，却为业务模块所共同调用的逻辑（例如事务处理、日志管理、权限控制等）封装起来，实现业务代码与通用功能解耦，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。 

Spring AOP 就是基于**动态代理**的，**如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 JDK Proxy，**去创建代理对象，而**对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 Cglib 生成⼀个被代理对象的子类来作为代理。**

#### 1.2.2 JDK 动态代理和 CGLIB 代理

Spring的AOP是通过动态代理来实现的，动态代理主要有两种方式JDK动态代理和 Cglib动态代理，这两种动态代理的使用和原理有些不同。 

**JDK 动态代理** 

1. Interface ：对于 JDK 动态代理，目标类需要实现一个Interface。 

2. **InvocationHandler** ：InvocationHandler是一个接口，可以通过实现这个接口，定义横切逻辑，再通过反射机制（invoke）调用目标类的代码，在此过程，可以包装逻辑，对目标方法进行前置后置处理。 

3. **Proxy** ：Proxy利用InvocationHandler动态创建一个实现了目标类实现的接口的实例，生成目标类的代理对象。 代理类与目标类的关系类似于兄弟，都实现了一个接口。

**CgLib 动态代理** 

1. 使用JDK创建代理有一大限制，它只能为接口创建代理实例，而CgLib 动态代理 就没有这个限制。 
2. CgLib 动态代理是使用字节码处理框架 ASM ，其原理是通过字节码技术为一个 类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，顺势织入切面逻辑。 
3. CgLib 创建的动态代理对象性能比 JDK 创建的动态代理对象的性能高不少，但是 CGLib 在创建代理对象时所花费的时间却比 JDK 多得多，所以对于单例的对象，因为无需频繁创建对象，用 CGLib 合适，反之，使用 JDK 方式要更为合适一些。同时，由于 CGLib 由于是采用动态创建子类的方法，对于 final 方法，无法进行代理。

#### 1.2.3 Spring AOP 和 AspectJ AOP 区别

**Spring AOP** 属于 **运行时增强** ，主要具有如下特点： 

1. 基于动态代理来实现，默认如果使用接口的，用 JDK 提供的动态代理实现，如果是方法则使用 CGLIB 实现 
3. 在性能上，由于 Spring AOP 是基于 动态代理 来实现的，在容器启动时需要生成代理实例，在方法调用上也会增加栈的深度，使得 Spring AOP 的性能不如 AspectJ 的那么好。 

**AspectJ** 

AspectJ 是一个易用的功能强大的 AOP 框架，属于 **编译时增强** ， 可以单独使用，也可以整合到其它框架中，是 AOP 编程的完全解决方案。AspectJ 需要用到单独的编译器 ajc。 AspectJ 属于静态织入，通过修改代码来实现，在实际运行之前就完成了织入，所 以说它生成的类是没有额外运行时开销的。

#### 1.2.5 自己项目中的 AOP

在我们所开发的所有项目中，都是使用aop来记录系统的操作日志的，还有统计接口访问情况。主要是为了我们系统出现错误的时候快速定位，以及记录不同部门对我们服务的访问情况。

主要思路是这样的，我们是自定义了日志注解，然后使用aop中的环绕通知，对使用我们这个注解的所有方法做增强，我们通过环绕通知记录了接口响应时间、访问接口的ip、请求时间等，获取到这些参数以后，保存到数据库。



### 1.3 事务

#### 1.3.1 Spring 事务类型

+ 编程式事务 ： 在代码中硬编码(不推荐使用) : 通过 TransactionTemplate 或者 TransactionManager 手动管理事务。
+ 声明式事务 ： 在 XML 配置文件中配置或者直接基于注解（推荐使用） : 实际是通过 AOP 实现 （基于 @Transactional 的全注解方式使用最多）

#### 1.3.2 Spring 事务传播行为

事务传播行为是为了解决业务层方法之间互相调用的事务问题。 

当事务方法被另⼀个事务方法调用时，必须指定事务应该如何传播。

Spring事务定义了7种传播机制： 

1. PROPAGATION_REQUIRED:默认的Spring事物传播级别，若当前存在事务，则加入该事务，若 不存在事务，则新建一个事务。 
2. PAOPAGATION_REQUIRE_NEW:若当前没有事务，则新建一个事务。若当前存在事务，则新建 一个事务，新老事务相互独立。外部事务抛出异常回滚不会影响内部事务的正常提交。
3. PROPAGATION_NESTED:如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务， 则新建一个事务，类似于REQUIRE_NEW。 
4. PROPAGATION_SUPPORTS:支持当前事务，若当前不存在事务，以非事务的方式执行。 
5. PROPAGATION_NOT_SUPPORTED:以非事务的方式执行，若当前存在事务，则把当前事务挂 起。 
6. PROPAGATION_MANDATORY:强制事务执行，若当前不存在事务，则抛出异常. 
7. PROPAGATION_NEVER:以非事务的方式执行，如果当前存在事务，则抛出异常。 Spring事务传播级别一般不需要定义，默认就是PROPAGATION_REQUIRED，除非在嵌套事务的情 况下需要重点了解。

#### 1.3.3 事务失效的情况

1. **@Transactional 应用在非 public 修饰的方法上**：TransactionInterceptor （事务拦截器）在目标方法执行前后进行拦截，DynamicAdvisedInterceptor（CglibAopProxy 的内部类）的 intercept 方法或 JdkDynamicAopProxy 的 invoke 方法会间接调用 AbstractFallbackTransactionAttributeSource的 computeTransactionAttribute 方法，获取Transactional 注解的事务配置信息。

   因为内部的computeTransactionAttribute方法，获取Transactional 注解的事务配置信息的时候，会检查目标方法是否为 public，不是public 则不会获取 Transactional 注解配置信息。

2. **@Transactional 注解属性 rollbackFor 设置错误**：如果方法抛出检查异常（如FileNotFound异常时），也会导致事务失效，因为 Transactional 注解的默认 rollbackFor 属性为运行时异常，所以无法捕获检查异常。我们只需要配置rollbackFor属性为Exception，这样别管是什么异常，都会回滚事务。

3. **同一个类中方法调用，导致@Transactional失效**：同一个类中，A方法调用B方法，A没有事务，B声明了事务，外部调用方法A的话，会导致B的事务失效。因为要想一个方法的事务生效，就要调用这个类的代理对象，可是当一个类中的方法直接调用同类中的另一个方法时，调用不会通过Spring生成的代理对象，而是通过 `this` 直接调用。这种情况下，Spring AOP无法拦截这个内部调用，事务管理逻辑就不会被触发。



### 1.4 Spring MVC

#### 1.4.1 SpringMVC的执行流程

![image-20240725010230199](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407250102451.png)

我不太了解视图的那部分流程，因为我开始学的时候就已经是前后端分离开发了，都是直接使用@ResponseBody 来返回 JSON 数据的形式：

1. 用户发送出请求到前端控制器DispatcherServlet，这是一个调度中心
2. DispatcherServlet 根据请求信息调用 HandlerMapping 。 HandlerMapping 根据 uri 去匹配查找能处理的 Handler （也就是我们平常说的 Controller 控制器） ，并会将请求涉及到的拦截器一起封装,返回给DispatcherServlet。

4. DispatcherServlet调用HandlerAdapter（处理器适配器）。

5. HandlerAdapter经过适配调用具体的处理器（Handler/Controller）。

6. 方法上添加了 @ResponseBody，就通过HttpMessageConverter 来返回结果并且转换为JSON



## 三、Spring Boot

### 3.1 优点

Spring Boot 以 **约定大于配置** 核心思想开展工作，相比Spring具有如下优势： 

1. Spring Boot 内嵌了如Tomcat，Jetty这样的web容器，也就是说可以直接跑起来，用不着再做部署工作了。 
2. Spring Boot 无需再像Spring一样使用繁琐的xml文件配置，可以自动配置(核心)Spring。
3. 通过Spring Boot Starters，可以快速整合常用依赖。

### 3.2 自动配置原理

自动装配就是把别人（官方）写好的配置类加载到spring容器，然后根据这个配置类生成一些项目需要的bean对象。

1. 启动类上`@springbootapplication`是个复合注解，里面包含一个`@enableAutoConfigration`注解，是实现自动化配置的核心注解。 
2. `@enableautoconfigration`注解也是一个复合注解，里面含有一个`@import`注解
3. @import导入了一个**`autoConfigrationImportSelector`**类，通过它的`selectImports`方法去读取该项目和该项目引用的Jar包的的classpath路径下**META-INF/spring.factories**文件中的所配置的类的全类名。 
4. 在这些配置类中所定义的Bean会根据条件注解所**指定的条件来决定**是否需要将其导入到Spring容器中。一般条件判断会有像`@ConditionalOnClass`这样的注解，判断是否有对应的class文件，如果有则加载该类，把这个配置类的所有的Bean放入spring容器中使用。


### 3.3 Spring Boot Starters

Spring Boot Starters 是一系列依赖关系的集合，因为它的存在，项目的依赖之间的关系对我们来说变的更加简单了。 

举个例子：在没有 Spring Boot Starters 之前，我们开发 REST 服务或 Web 应用程序时; 我们需要使用像 Spring MVC，Tomcat 和 Jackson 这样的库，这些依 赖我们需要手动一个一个添加。但是，有了 Spring Boot Starters 我们只需要⼀ 个只需添加一个spring-boot-starter-web⼀个依赖就可以了，这个依赖包含的子依赖中包含了我们开发 REST 服务需要的所有依赖。





## 四、Spring Cloud