# Annotation



##### @TableId(value = "id",type = IdType.ASSIGN_ID)

> `@TableId` 是 MyBatis-Plus 框架中的一个注解，用于标识实体类的主键字段，指定主键的生成策略。
>
> 在该注解的参数中，`value` 指定了数据库表中对应的列名，即主键字段的名称。`type` 指定了主键的生成策略，其中 `IdType.ASSIGN_ID` 是一种主键生成策略，表示使用数据库内部的自增ID来生成主键值。
>
> 具体来说，`IdType.ASSIGN_ID` 策略要求数据库中的主键字段是自增的，并且在插入数据时，MyBatis-Plus 会自动将数据库生成的主键值赋值给实体类的主键字段，从而实现主键的自动赋值。
>
> 使用 `@TableId` 注解配合 `IdType.ASSIGN_ID` 策略，可以简化在使用 MyBatis-Plus 进行数据库操作时对主键的处理，减少手动编写主键生成逻辑的工作量。注意，在使用 `IdType.ASSIGN_ID` 策略时，需要确保数据库表中的主键字段是自增的，否则会导致主键生成失败。

##### @TableId(type = IdType.ASSIGN_ID)这个注解中的type都有哪些值

> `@TableId` 注解中的 `type` 参数用于指定主键生成策略，`IdType` 是一个枚举类，定义了 MyBatis-Plus 框架支持的主键生成策略。以下是 `IdType` 中定义的主键生成策略类型：
>
> 1. `AUTO`：数据库自增主键，由数据库自动生成主键值。对应于数据库中的自增、自动增长等机制。
>
> ```java
> @TableId(type = IdType.AUTO)
> ```
>
> 2. `NONE`：无主键生成策略，不会生成主键值。适用于用户手动指定主键值。
>
> ```java
> @TableId(type = IdType.NONE)
> ```
>
> 3. `ID_WORKER`：雪花算法生成的分布式 ID，保证在分布式环境中的唯一性和有序性。
>
> ```java
> @TableId(type = IdType.ID_WORKER)
> ```
>
> 4. `ID_WORKER_STR`：字符串类型的雪花算法生成的分布式 ID。
>
> ```java
> @TableId(type = IdType.ID_WORKER_STR)
> ```
>
> 5. `UUID`：使用 Java 的 `java.util.UUID` 类生成的随机 UUID 作为主键值。
>
> ```java
> @TableId(type = IdType.UUID)
> ```
>
> 6. `ASSIGN_ID`：由数据库内部自动生成的主键值，例如 MySQL 的 `AUTO_INCREMENT`、Oracle 的 `IDENTITY` 等。
>
> ```java
> @TableId(type = IdType.ASSIGN_ID)
> ```
>
> 以上是 MyBatis-Plus 框架中支持的主键生成策略类型。可以根据具体的需求和数据库类型选择合适的主键生成策略来使用 `@TableId` 注解。

##### 使用uuid

> `@TableId(type = IdType.UUID)` 注解的作用是指定在使用 MyBatis-Plus 框架向数据库插入数据时，自动生成 UUID 作为主键值。但是需要注意的是，这只是一种生成主键值的策略，并不会直接自动向数据库插入数据。
>
> 在使用 `@TableId(type = IdType.UUID)` 注解后，当通过 MyBatis-Plus 提供的插入方法（例如 `insert()`、`insertBatch()` 等）插入新的实体对象时，MyBatis-Plus 会自动调用 Java 的 `java.util.UUID.randomUUID()` 方法生成一个随机的 UUID 值，并将其设置到实体对象的主键字段上，然后再将整个实体对象插入到数据库中。
>
> 例如，以下是一个使用 `@TableId(type = IdType.UUID)` 注解的实体类示例：
>
> ```java
> import com.baomidou.mybatisplus.annotation.IdType;
> import com.baomidou.mybatisplus.annotation.TableId;
> 
> public class MyEntity {
>     @TableId(type = IdType.UUID)
>     private String id;
>     // 其他字段和方法...
> }
> ```
>
> 在使用 MyBatis-Plus 提供的插入方法时，可以直接创建实体对象并设置其他字段的值，无需手动设置主键值，MyBatis-Plus 会在插入数据时自动生成 UUID 作为主键值，如下所示：
>
> ```java
> MyEntity entity = new MyEntity();
> entity.setName("foo");
> entity.setAge(18);
> // 不需要手动设置主键值
> myEntityMapper.insert(entity);
> ```
>
> 以上代码会生成一个新的 UUID 值，并将其设置到实体对象的 `id` 字段上，然后插入到数据库中。



[SpringBoot - MyBatis-Plus使用详解（主键策略、UUID、Sequence](https://www.cnblogs.com/DanielL916/p/16177526.html)]



##### @IdToName(Alias = "unitName")这个注解是什么意思

> @IdToName(Alias = "unitName") 这个注解通常是用于Java中的类、方法、属性等上面的注释，它的作用是将指定的别名（Alias）与该类、方法、属性等相关联。这样可以方便地在程序中使用这个别名来代替该类、方法、属性等的名称，从而增加代码的可读性和可维护性。
>
> 具体来说，如果一个Java类、方法、属性等上面使用了 @IdToName(Alias = "unitName") 这个注解，那么在程序中就可以使用 "unitName" 这个别名来代替该类、方法、属性等的名称。例如：
>
> ```
> @IdToName(Alias = "unitName")
> public class MyClassName {
>     // 类的定义
> }
> 
> public class MyClass {
>     @IdToName(Alias = "unitName")
>     private int myVariable;
>     
>     @IdToName(Alias = "unitName")
>     public void myMethod() {
>         // 方法的实现
>     }
> }
> ```
>
> 在程序中可以这样使用别名：
>
> ```
> MyClassName obj = new MyClassName(); // 使用类的别名
> MyClass obj2 = new MyClass();
> obj2.set("unitName", 10); // 使用属性的别名
> obj2.call("unitName"); // 使用方法的别名
> ```
>
> 这样就可以方便地使用别名来代替类、方法、属性等的名称了。