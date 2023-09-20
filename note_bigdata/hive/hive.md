# hive









### Hive CLI

Hive CLI使用HiveServer1。Hive CLI使用Thrift协议连接到远程Hiveserver1实例。

> HiveServer1 提供了一个 Thrift 服务，通过 Thrift 协议与 Hive CLI 进行通信。Hive CLI 将查询和命令发送到 HiveServer1，然后 HiveServer1 解析和执行这些请求，最后返回结果给 Hive CLI。
>
> 需要注意的是，HiveServer1 在较新的 Hive 版本中已经不再被推荐使用，而被 HiveServer2 取代。HiveServer2 提供了更多功能和性能优化，并且更加稳定。因此，如果您在使用较新版本的 Hive，建议使用 HiveServer2 作为远程访问 Hive 的方式，而不是 HiveServer1。不过，对于一些旧的 Hive 部署，可能仍然在使用 HiveServer1。

### Beeline

Beeline使用JDBC连接到远程HiveServer2实例。

[Hive CLI和Beeline的区别-Hive vs Beeline](https://blog.csdn.net/u013230189/article/details/108159520)

### HiveServer2

HiveServer2(HS2)是一个服务器接口，能使远程客户端执行Hive查询，并且可以检索结果。HiveServer2是HiveServer1的改进版，HiveServer1已经被废弃。HiveServer2可以支持多客户端并发和身份认证。旨在为开放API客户端（如JDBC和ODBC）提供更好的支持。



[[Hive]HiveServer2配置](https://developer.aliyun.com/article/632149)

### RPC

RPC：远程过程调用

由于HTTP在应用层中完成，整个通信的代价较高，远程过程调用中直接基于TCP进行远程调用，数据传输在传输层TCP层完成，更适合对效率要求比较高的场景，RPC主要依赖于客户端和服务端之间建立Socket链接进行，底层实现比REST更复杂。



[什么是RPC？](https://www.jianshu.com/p/7d6853140e13)

### Thrift

+ hiveServer2 使用的是 thrift 协议吗

> 是的，HiveServer2 使用 Thrift 协议来与客户端进行通信。HiveServer2 是 Apache Hive 的一个组件，它提供了一个远程接口，允许客户端应用程序通过网络连接到 Hive 集群，执行 SQL 查询和其他 Hive 相关操作。Thrift 是在这种通信中使用的协议。
>
> 客户端可以使用不同的编程语言编写，并通过 Thrift 生成的客户端库与 HiveServer2 通信。这使得不同编程语言的应用程序都能够与 Hive 集群进行交互，而不需要直接编写与 Hive 通信的底层代码。
>
> 总之，HiveServer2 使用 Thrift 协议来提供远程访问 Hive 集群的能力，这有助于实现跨语言的通信和互操作性。这是 Hive 在分布式环境中执行查询的一种方式。

`Thrift`是一个**轻量级**、**跨语言**的**远程服务调用**RPC框架，最初由`Facebook`开发，后面进入`Apache`开源项目。它通过自身的`IDL`**中间语言**, 并借助**代码生成引擎**生成各种主流语言的`RPC`**服务端**/**客户端**模板代码。

> 后面可以好好学学，可以用于代码中由 REST 切换为 gRPC

[Apache Thrift系列详解](https://juejin.cn/post/6844903622380093447)



### derby

- 元数据：Metastore

元数据包括：表名、表所属的数据库（默认是default）、表的拥有者、列/分区字段、表的类型（是否是外部表）、表的数据所在目录等；

默认存储在自带的derby数据库中，推荐使用MySQL存储Metastore

[Derby数据库简单介绍和使用方法](https://blog.csdn.net/kai_wei_zhang/article/details/7749568)



### 参考

[hive从入门到放弃](https://blog.csdn.net/weixin_43596734/category_11680264.html)

[hive系列文章](https://blog.csdn.net/ericleiy/category_11289261.html)