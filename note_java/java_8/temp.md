## 方法引用和lambda

java 方法引用是什么



Java 方法引用是一种语法糖，它可以让你用方法的名字来指向一个方法，而不需要写出完整的 lambda 表达式。¹²

Java 方法引用有四种类型：¹²

- 构造器引用：Class::new
- 静态方法引用：Class::static_method
- 特定类的任意对象的方法引用：Class::method
- 特定对象的方法引用：instance::method

你想了解更多关于 Java 方法引用的什么呢？

源: 与必应的对话， 2023/4/16(1) Java 8 方法引用 | 菜鸟教程. https://bing.com/search?q=java+%e6%96%b9%e6%b3%95%e5%bc%95%e7%94%a8 访问时间 2023/4/16.
(2) Java 方法引用|极客教程 - geek-docs.com. https://geek-docs.com/java/java-examples/java-method-reference.html 访问时间 2023/4/16.
(3) Java 8 方法引用 | 菜鸟教程. https://www.runoob.com/java/java8-method-references.html 访问时间 2023/4/16.
(4) Java 8之方法引用 - 知乎. https://zhuanlan.zhihu.com/p/145694542 访问时间 2023/4/16.
(5) 秒懂Java之方法引用（method reference）详解 - CSDN博客. https://blog.csdn.net/ShuSheng0007/article/details/107562812 访问时间 2023/4/16.





什么叫语法糖





语法糖是一种计算机语言中的特殊语法，它可以让程序员更方便地使用语言，但是不影响语言的功能。¹²

语法糖的例子有很多，比如在 Python 中，你可以用 a, b = b, a 来交换两个变量的值，这就是一种语法糖，因为它等价于 t = a; a = b; b = t; 但是更简洁易读。³

你对语法糖还有什么疑问吗？

源: 与必应的对话， 2023/4/16(1) 语法糖（Syntactic sugar）/ 语法盐（syntactic salt .... https://bing.com/search?q=%e8%af%ad%e6%b3%95%e7%b3%96 访问时间 2023/4/16.
(2) 什么是语法糖？_语法糖是啥意思_野猪力量的博客-CSDN博客. https://blog.csdn.net/wofreeo/article/details/80679290 访问时间 2023/4/16.
(3) 牛鹭学院：Python语法糖，你知道吗？ - 知乎专栏. https://zhuanlan.zhihu.com/p/210501891 访问时间 2023/4/16.
(4) 语法糖（Syntactic sugar）/ 语法盐（syntactic salt .... https://blog.csdn.net/weixin_40539125/article/details/94340582 访问时间 2023/4/16.
(5) python 程序 中的语法糖有哪些？ - 知乎. https://www.zhihu.com/question/584179639 访问时间 2023/4/16.
(6) vue3基础之语法、核心API - 掘金. https://juejin.cn/post/7027438996888223758 访问时间 2023/4/16.





Java 中有哪些常用的语法糖？





Java 中有很多常用的语法糖，比如：¹²³

- 泛型：让你可以定义和使用参数化的类型，例如 List<String>。
- 变长参数：让你可以用一个数组来接收不定数量的参数，例如 public void print(String... args)。
- 条件编译：让你可以根据编译时的常量来决定是否编译某段代码，例如 if (false) { System.out.println("never"); }。
- 自动拆装箱：让你可以在基本类型和包装类型之间自动转换，例如 Integer i = 1; int j = i;。
- 内部类：让你可以在一个类中定义另一个类，例如 class Outer { class Inner { } }。

还有其他一些语法糖，比如 lambda 表达式、方法引用、枚举、断言、foreach 循环等。⁴⁵⁶

你对这些语法糖有什么看法吗？

源: 与必应的对话， 2023/4/16(1) Java语法糖详解_feifuzeng的博客-CSDN博客. https://blog.csdn.net/feifuzeng/article/details/113308160 访问时间 2023/4/16.
(2) Java 语法糖详解_热爱编程的大忽悠的博客-CSDN博客. https://blog.csdn.net/m0_53157173/article/details/126965454 访问时间 2023/4/16.
(3) Java 语法糖详解 - Joey先生 - 博客园. https://www.cnblogs.com/helloworld2048/p/10916453.html 访问时间 2023/4/16.
(4) 不了解这12个语法糖，别说你会Java！ - 知乎专栏. https://zhuanlan.zhihu.com/p/78454816 访问时间 2023/4/16.
(5) Java 语法糖详解 | JavaGuide(Java面试+学习指南). https://javaguide.cn/java/basis/syntactic-sugar.html 访问时间 2023/4/16.
(6) Java语法糖_ThinkWon的博客-CSDN博客. https://blog.csdn.net/ThinkWon/article/details/100103689 访问时间 2023/4/16.





java方法引用详细介绍一下





方法引用是 Java 8 中的一个新特性，它可以让你用方法的名字来指向一个方法，而不需要调用它。¹²

方法引用可以使代码更简洁易读，它可以替代一些只是调用一个已有方法的 lambda 表达式。³⁴

方法引用有四种形式：¹²

- 构造器引用：使用 Class::new 来引用一个类的构造器，例如 Person::new。
- 静态方法引用：使用 Class::static_method 来引用一个类的静态方法，例如 Math::abs。
- 特定类的任意对象的方法引用：使用 Class::method 来引用一个类的实例方法，例如 String::length。
- 特定对象的方法引用：使用 instance::method 来引用一个对象的实例方法，例如 System.out::println。

你想看一些方法引用的具体例子吗？

源: 与必应的对话， 2023/4/16(1) Java 8 方法引用 | 菜鸟教程. https://bing.com/search?q=java+%e6%96%b9%e6%b3%95%e5%bc%95%e7%94%a8 访问时间 2023/4/16.
(2) Java 方法引用|极客教程 - geek-docs.com. https://geek-docs.com/java/java-examples/java-method-reference.html 访问时间 2023/4/16.
(3) Java 8 方法引用 | 菜鸟教程. https://www.runoob.com/java/java8-method-references.html 访问时间 2023/4/16.
(4) 秒懂Java之方法引用（method reference）详解 - CSDN博客. https://blog.csdn.net/ShuSheng0007/article/details/107562812 访问时间 2023/4/16.
(5) Java 8之方法引用 - 知乎. https://zhuanlan.zhihu.com/p/145694542 访问时间 2023/4/16.





是的，请给我一些例子。







好的，我给你一些方法引用的例子。请看下面的代码：

```java
// 构造器引用
// 使用 Supplier 接口创建 Person 对象
Supplier<Person> supplier = Person::new;
Person person = supplier.get();

// 静态方法引用
// 使用 Function 接口对数字取绝对值
Function<Integer, Integer> function = Math::abs;
int result = function.apply(-10); // result = 10

// 特定类的任意对象的方法引用
// 使用 Predicate 接口判断字符串是否为空
Predicate<String> predicate = String::isEmpty;
boolean flag = predicate.test(""); // flag = true

// 特定对象的方法引用
// 使用 Consumer 接口打印字符串
Consumer<String> consumer = System.out::println;
consumer.accept("Hello"); // 输出 Hello
```

你觉得这些例子怎么样？





QueryWrapper继承自 AbstractWrapper ,自身的内部属性 entity 也用于生成 where 条件 及 LambdaQueryWrapper, 可以通过 new QueryWrapper().lambda() 方法获取，是什么意思





这是关于 MyBatis-Plus 的一个问题，它是一个 Java 框架，用于简化数据库操作。¹²

QueryWrapper 是一个条件构造器，用于生成 SQL 的 where 条件，它继承自 AbstractWrapper，这是一个抽象类，定义了一些通用的方法。¹²

QueryWrapper 的内部属性 entity 也可以用于生成 where 条件，它是一个实体类的对象，可以设置一些属性值，作为查询条件。¹²

LambdaQueryWrapper 是一个特殊的 QueryWrapper，它可以使用 lambda 表达式来构造查询条件，这样可以避免写错字段名的风险。¹³

你可以通过 new QueryWrapper().lambda() 方法获取一个 LambdaQueryWrapper 的对象，然后使用它来进行查询。¹³

你明白了吗？

源: 与必应的对话， 2023/4/16(1) Myabtis-Plus之QueryWrapper常用方法 - 余月七 - 博客园. https://www.cnblogs.com/yuyueq/p/14639016.html 访问时间 2023/4/16.
(2) 条件构造器 | MyBatis-Plus. https://baomidou.com/pages/10c804/ 访问时间 2023/4/16.
(3) 全网详解MyBatis-Plus LambdaQueryWrapper的使用说明以及 .... https://blog.csdn.net/lvoelife/article/details/129027594 访问时间 2023/4/16.





方法引用中的特定类的任意对象的方法引用，是一定要有具体对象才可以这么使用吧





不一定，特定类的任意对象的方法引用，是指你可以使用一个类的实例方法，而不需要指定具体的对象。

例如，你可以使用 String::length 来引用 String 类的 length 方法，这个方法是一个实例方法，它需要一个 String 对象来调用，但是你不需要指定具体是哪个 String 对象。

你可以把这种方法引用看作是一个函数，它接受一个 String 对象作为参数，返回它的长度。

你可以用这种方法引用来替代 lambda 表达式，例如 (String s) -> s.length()。

你理解了吗？





ServiceImpl中的getone方法







ServiceImpl 是 MyBatis-Plus 中的一个类，它实现了 IService 接口，提供了一些通用的数据库操作方法。¹²

getOne 是 ServiceImpl 中的一个方法，它用于查询一条数据，返回一个实体对象。¹³

getOne 方法接受两个参数，第一个参数是一个 QueryWrapper 对象，用于指定查询条件，第二个参数是一个布尔值，用于指定是否抛出异常。³

如果第二个参数为 true，那么当查询结果大于一条时，程序会抛出 TooManyResultsException 异常。³

如果第二个参数为 false，那么当查询结果大于一条时，程序会返回第一条数据。³

你想看一个 getOne 方法的示例代码吗？

源: 与必应的对话， 2023/4/16(1) 这些Mybatis-Plus使用技巧，太火了 - 知乎. https://zhuanlan.zhihu.com/p/279379694 访问时间 2023/4/16.
(2) MybatisPlus生成器ServiceImpl类详解_小小学渣的博客-CSDN博客. https://blog.csdn.net/weixin_45218371/article/details/124626491 访问时间 2023/4/16.
(3) MybatisPlus学习笔记（八）------通用service - 知乎. https://zhuanlan.zhihu.com/p/103580510 访问时间 2023/4/16.





User userTemp = userService.getOne(new QueryWrapper<User>().lambda().eq(User::getAccount,userQuery.getAccount()));这段代码怎么执行





这段代码的执行过程是这样的：

1. 创建一个 QueryWrapper 对象，用 lambda 表达式指定查询条件为 User 的 account 属性等于 userQuery 的 account 属性。
2. 调用 userService 的 getOne 方法，传入 QueryWrapper 对象，查询数据库中满足条件的数据。
3. 如果查询结果为空，返回 null。
4. 如果查询结果只有一条，返回这条数据对应的 User 对象。
5. 如果查询结果多于一条，抛出 TooManyResultsException 异常，因为没有指定第二个参数，默认为 true。
6. 将返回的 User 对象赋值给 userTemp 变量。

你理解了吗？







对于第一条我不太理解





第一条是指创建一个 QueryWrapper 对象，这是一个条件构造器，用于生成 SQL 的 where 条件。

lambda 表达式是一种简洁的函数表示方式，它可以用来指定查询条件的字段和值。

eq 是一个方法，它表示等于的关系，它接受两个参数，第一个参数是一个函数，用于获取 User 的 account 属性，第二个参数是 userQuery 的 account 属性的值。

这样，就相当于生成了一个 SQL 语句的一部分，如 where user.account = 'xxx'。

你明白了吗？





eq方法的第一个参数可以写成lambda表达式吗





是的，eq 方法的第一个参数可以写成 lambda 表达式，它是一个函数，它接受一个 User 对象作为参数，返回它的 account 属性。

例如，你可以写成 (User u) -> u.getAccount()，这样就和 User::getAccount 等价。

但是，使用 User::getAccount 的方式更简洁，也更容易阅读，所以建议你使用这种方式。

你理解了吗？



那这个函数是什么时候被调用的呢





这个函数是在 QueryWrapper 对象生成 SQL 语句的时候被调用的，它用于获取查询条件的字段名。

QueryWrapper 对象会遍历所有的条件方法，如 eq、gt、like 等，然后调用它们的第一个参数，传入一个实体对象，获取它们的属性名，然后拼接成 SQL 语句的 where 条件。

例如，如果你有一个 User 对象 user，它的 account 属性为 'admin'，那么当 QueryWrapper 对象调用 (User u) -> u.getAccount() 这个函数时，就会传入 user 对象，返回 'account' 这个字符串，然后拼接成 where account = 'admin' 这样的 SQL 语句。

你明白了吗？





eq语句的第一个参数是什么





eq 语句的第一个参数是一个函数，它接受一个实体对象作为参数，返回它的一个属性。

你可以使用 lambda 表达式或者方法引用来表示这个函数。

例如，如果你想查询 User 的 name 属性等于 'Tom' 的数据，你可以写成：

QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.eq((User u) -> u.getName(), "Tom");

或者

QueryWrapper<User> wrapper = new QueryWrapper<>();
wrapper.eq(User::getName, "Tom");

这两种写法都是等价的，它们都会生成 where name = 'Tom' 这样的 SQL 语句。

你理解了吗？





为什么这个函数是得到一个对象的name属性值，而生成的sql中对应的是name这个属性呢？





这是因为 QueryWrapper 对象会使用反射机制来获取这个函数的属性名。

反射机制是一种动态获取类的信息的技术，它可以让程序在运行时获取类的属性、方法、构造器等信息。

QueryWrapper 对象会通过反射机制获取这个函数的返回值类型，然后根据这个类型找到对应的属性名，然后拼接成 SQL 语句。

例如，如果这个函数的返回值类型是 String，那么 QueryWrapper 对象会通过反射机制找到 User 类中所有类型为 String 的属性，然后根据这些属性的顺序，选择第一个属性作为 SQL 语句的字段名。

如果 User 类中有多个类型为 String 的属性，那么你可以使用 @TableField 注解来指定哪个属性对应哪个字段名。

你明白了吗？















