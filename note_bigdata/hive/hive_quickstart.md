# hive



+ 删除文件(hdfs)

```
[root@glnode01 ~]# hdfs dfs -rmr /tjgl/tmp/origindb/origin_gty_billing_transaction/origin_gty_billing_transaction__ce076ba7_e970_49e2_8ceb_39a5d98419a0
```

+ 展示分区信息

```
0: jdbc:hive2://172.16.4.71:10001> 
show partitions origin_gty_billing_transaction;
```

+ 展示表创建信息

```
0: jdbc:hive2://172.16.4.71:10001> show create table origin_cpcts_exotu;
```

+ 删除表

```
0: jdbc:hive2://172.16.4.71:10001> drop table origin_etcts_entrypassdata;
```

+ 删除数据库

```
0: jdbc:hive2://172.16.4.71:10001> drop database origindata cascade;
```

​	



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





### 直接配置hive永久动态分区

+ 配置

> 在Hive中，您可以使用动态分区来将数据加载到分区表中，而无需预先创建分区目录。这使得数据加载和管理更加灵活。下面是如何在Hive中配置和使用动态分区的步骤：
>
> 1. **启用动态分区功能**：
>
>    在Hive中，动态分区功能默认是禁用的，您需要在Hive的配置中启用它。为此，您可以在Hive的配置文件中（通常是`hive-site.xml`）添加以下配置：
>
>    ```xml
>    <property>
>        <name>hive.exec.dynamic.partition</name>
>        <value>true</value>
>    </property>
>    <property>
>        <name>hive.exec.dynamic.partition.mode</name>
>        <value>nonstrict</value>
>    </property>
>    ```
>
>    `hive.exec.dynamic.partition` 设置为 `true` 启用了动态分区功能，而 `hive.exec.dynamic.partition.mode` 设置为 `nonstrict` 允许动态分区的非严格模式，即允许动态分区的字段可以为空。
>
> 动态分区使得在Hive中加载数据到分区表变得更加便捷，无需手动创建分区目录，Hive会自动管理分区的创建。请根据您的需求配置动态分区并执行适当的数据加载操作。

+ 查看

> 要查看您Hive中动态分区的设置，您可以使用以下步骤：
>
> 1. 打开Hive命令行界面，可以在终端中运行以下命令：
>    
>    ```bash
>    hive
>    ```
>
>    这会打开Hive的命令行界面，允许您与Hive进行交互。
>
> 2. 在Hive命令行界面中，您可以使用以下命令查看动态分区设置：
>
>    ```sql
>    SHOW CONF "hive.exec.dynamic.partition";
>    SHOW CONF "hive.exec.dynamic.partition.mode";
>    ```
>
>    第一个命令用于查看是否启用了动态分区，第二个命令用于查看动态分区的模式（strict或nonstrict）。
>
>    例如，如果您要查看 `hive.exec.dynamic.partition` 的设置，可以运行：
>
>    ```sql
>    SHOW CONF "hive.exec.dynamic.partition";
>    ```
>
>    这将显示配置的值，如果它是 `true`，表示启用了动态分区。同样，您可以运行以下命令来查看 `hive.exec.dynamic.partition.mode` 的设置：
>
>    ```sql
>    SHOW CONF "hive.exec.dynamic.partition.mode";
>    ```
>
>    这将显示动态分区模式的设置，通常应该是 `nonstrict`，以允许动态分区的字段可以为空。
>
> 3. 确认设置后，您可以使用 `exit;` 命令退出Hive命令行界面。
>
> 通过上述步骤，您可以在Hive命令行界面中查看动态分区的设置。如果您在 `hive-site.xml` 中更改了这些设置，并且希望确认是否生效，上述步骤可以帮助您验证配置。



### 详细说一下 hive命令行 和 hiveserver2

> Hive命令行（Hive CLI）和HiveServer2都是与Apache Hive相关的两个不同的组件，用于执行Hive查询和操作，但它们在性质、用途和工作方式上有一些区别。
>
> **Hive命令行 (Hive CLI)：**
>
> 1. **性质**：Hive命令行是一个命令行工具，通常用于在本地终端中直接与Hive交互。
>
> 2. **用途**：Hive命令行主要用于交互式的数据查询、开发和管理任务。您可以使用Hive命令行执行Hive查询、创建表、加载数据、查看数据等操作。
>
> 3. **工作方式**：Hive命令行通过JDBC连接到Hive Metastore（或者直接到HiveServer2，取决于配置），并允许用户通过命令行界面输入Hive查询。
>
> 4. **特点**：Hive命令行通常支持一组内置命令，例如`SHOW TABLES`、`DESCRIBE TABLE`、`SELECT`等，以及一些特定于Hive的命令。
>
> 5. **适用场景**：Hive命令行适用于交互式查询和开发任务，以及一些基本的Hive管理操作。它通常在本地或Hive服务器所在的节点上使用。
>
> **HiveServer2：**
>
> 1. **性质**：HiveServer2是Hive的一个独立的服务器进程，用于提供远程客户端访问Hive的服务。
>
> 2. **用途**：HiveServer2主要用于支持远程客户端（如Beeline、JDBC客户端等）连接到Hive，并执行Hive查询和操作。它使得多个客户端可以同时访问Hive，支持并发查询。
>
> 3. **工作方式**：HiveServer2通过Thrift或HTTP等协议提供对Hive的JDBC/ODBC连接。客户端通过这些连接发送SQL查询，并接收查询结果。
>
> 4. **特点**：HiveServer2主要提供SQL接口，不支持Hive命令行的一些特定命令。它通常与外部客户端工具一起使用，如Beeline、ODBC驱动程序、JDBC驱动程序等。
>
> 5. **适用场景**：HiveServer2适用于生产环境中，特别是当需要多个用户或应用程序同时访问Hive时。它提供了稳定、可扩展的Hive查询服务，并支持远程客户端连接。
>
> 总结：Hive命令行适用于本地交互式开发和一些简单的管理任务，而HiveServer2用于提供远程客户端访问Hive的服务，以支持生产环境中的多用户查询和应用程序集成。您可以根据具体的使用情景选择使用其中之一或两者结合使用。



### hive默认存储元数据的derby是什么

> Apache Hive 是一个数据仓库工具，用于在大数据环境中查询和分析数据。在 Hive 中，Derby 是默认的元数据存储数据库。
>
> **Derby** 是一个嵌入式关系型数据库，通常用于小型应用程序或测试环境。在 Hive 中，Derby 用于存储元数据信息，如表的结构、分区信息、表的位置等。这些元数据对于 Hive 的运行和管理非常重要，因为它们用于查询优化、元数据检索和表的管理等任务。
>
> Derby 作为默认的元数据存储数据库，使得 Hive 在安装和配置时更加简单，因为用户不需要额外配置外部数据库。但是，对于大规模的生产环境，通常会考虑将元数据存储迁移到更强大和可扩展的数据库系统，如MySQL、PostgreSQL或Oracle等，以提高性能和可用性。这通常是通过配置 Hive 的元数据存储为外部数据库来实现的。
>
> 总之，Derby 是 Hive 默认的元数据存储数据库，用于存储与表和数据库相关的元数据信息。但在生产环境中，通常会考虑将其替换为更适合大规模使用的外部数据库。



##### 参考

[hive中内部表和外部表有什么区别？一般什么时候使用内部表什么时候使用外部表？](https://blog.csdn.net/qq_37189286/article/details/110919547)

[hive从入门到放弃](https://blog.csdn.net/weixin_43596734/category_11680264.html)

[hive系列文章](https://blog.csdn.net/ericleiy/category_11289261.html)