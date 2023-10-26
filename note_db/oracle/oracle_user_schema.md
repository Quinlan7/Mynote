# oracle特性

*oracle的基础知识*



[TOC]

### 用户、模式、表空间、表的关系

​		一个模式对应一个项目，对应一个用户；每个用户可以管理多个表空间，每个表空间由一个或多个物理文件(.dbf)组成，一个用户可以分配多个表空间，但只能有一个默认表空间，每张表可以存在于一个或多个表空间中（比如图中的表1）。

　　如果您有点好奇心，可能会问道，那多个用户可以共享一个表空间吗？答案是可以的
![image-20231013200155625](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310132002742.png)



### 数据库元数据

+ dba_segments这是什么表

> `DBA_SEGMENTS` 是 Oracle 数据字典视图之一，用于存储关于数据库中存储的段（例如表、索引、分区、LOB 等）的信息。它包含了关于这些段的元数据，如所在的表空间、所属用户（拥有者）、段的类型等信息。这个视图允许你查询数据库中的段以及它们所在的表空间。
>
> 以下是 `DBA_SEGMENTS` 视图中的一些常见列：
>
> - `OWNER`: 段的拥有者（用户）。
> - `SEGMENT_NAME`: 段的名称，可以是表、索引或其他对象的名称。
> - `SEGMENT_TYPE`: 段的类型，例如 TABLE、INDEX、LOBSEGMENT 等。
> - `TABLESPACE_NAME`: 段所在的表空间名称。
> - `BYTES`: 段的大小，以字节为单位。
> - `BLOCKS`: 段所占用的数据块数量。
>
> 通过查询 `DBA_SEGMENTS` 视图，你可以获取关于数据库中存储的段的详细信息，包括它们所在的表空间以及拥有者（用户）。这对于监视数据库的存储使用情况和管理段非常有用。需要注意的是，要查询 `DBA_SEGMENTS` 视图，通常需要具有适当的数据库权限，如 `SELECT_CATALOG_ROLE` 或 `SELECT ANY DICTIONARY` 权限。

+ 这个视图存储在哪里？

> `DBA_SEGMENTS` 视图是 Oracle 数据库的数据字典（或数据目录）的一部分，存储在数据库内部的系统表中。这些系统表包含了有关数据库对象、用户、权限、表空间等的元数据信息。数据字典是 Oracle 数据库的核心组成部分，用于记录和管理数据库的结构和配置。
>
> `DBA_SEGMENTS` 视图实际上是一个数据库视图，它是通过查询底层数据字典表来创建的。这些数据字典表包括 `SYS.DBA_SEGMENTS`，`SYS.DBA_USERS`，`SYS.DBA_TABLESPACES` 等，它们存储了关于数据库的元数据信息。这些表和视图通常只能由具有足够权限的用户（通常是具有 `SELECT_CATALOG_ROLE` 或 `SELECT ANY DICTIONARY` 权限的用户）进行查询。
>
> 你可以通过 SQL 查询来访问 `DBA_SEGMENTS` 视图，如前面提到的查询示例，以获取数据库中段的信息。这些视图和表存储在数据库的 `SYS` 用户模式中，对于普通用户而言通常是不可见的，但具有适当权限的用户可以查询这些视图来获取有关数据库的元数据信息。

##### 参考

[Oracle中表空间、段、区、块及方案详解](https://hongyitong.github.io/2016/09/05/Oracle%E4%B8%AD%E8%A1%A8%E7%A9%BA%E9%97%B4%E3%80%81%E6%AE%B5%E3%80%81%E5%8C%BA%E3%80%81%E5%9D%97%E5%8F%8A%E6%96%B9%E6%A1%88%E8%AF%A6%E8%A7%A3/)





### oralce 表空间扩容机制

> 在 Oracle 数据库中，表空间的自动扩展可以通过两种方式实现，具体取决于表空间的配置和设置。自动扩展可以增加数据文件的大小，也可以增加数据文件的数量。
>
> 1. **增加数据文件的大小**：在这种情况下，如果表空间的数据文件达到了其最大大小限制，Oracle 数据库会自动增加这些数据文件的大小，以容纳更多的数据。这通常是通过自动增加数据文件的大小来实现表空间的自动扩展。这需要确保表空间的数据文件的最大大小足够大，以容纳所需的数据。当数据库自动增加数据文件的大小时，这可能需要更多的磁盘空间。
>
> 2. **增加数据文件的数量**：在这种情况下，如果表空间的数据文件达到了其最大大小限制，Oracle 数据库会自动创建一个新的数据文件，并将新的数据块写入到新的数据文件中。这可以通过设置表空间的自动扩展属性来实现。这种方式下，不会增加单个数据文件的大小，而是增加数据文件的数量，从而实现表空间的自动扩展。
>
> 表空间的自动扩展配置取决于数据库管理员的需求和策略。通常，为了防止数据库空间不足的情况，建议配置表空间以进行自动扩展，不管是增加数据文件的大小还是数量，以确保数据库的持续正常运行。配置方法和具体参数设置可以在 Oracle 数据库的表空间属性中进行定义。



### oracle相关命令

+ 查询所有用户

```
SELECT username FROM dba_users;
```



+ 查询对应的空闲表空间的大小

```
-- 查询对应的空闲表空间的大小
SELECT tablespace_name, SUM(bytes) / (1024 * 1024) AS "Free Space (MB)"
FROM dba_free_space
GROUP BY tablespace_name;
```



+ 查看所有用户和它们所在的表空间

```
-- 查看所有用户和它们所在的表空间，可以执行以下查询：
SELECT DISTINCT d.tablespace_name, u.username
FROM dba_segments d
INNER JOIN dba_users u ON d.owner = u.username;
```





### oracle 表空间创建以及踩坑

navicat对于oracle分区信息的查看十分有限，且有bug



加入三条数据

![image-20231025151327806](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310251513862.png)

执行如下sql查看表和对应的表空间的关系

```sql
-- 查询表和对应的表空间
SELECT t.owner AS schema_name, t.table_name, s.tablespace_name, s.bytes/1024/1024 AS size_mb
FROM dba_tables t
INNER JOIN dba_segments s ON t.table_name = s.segment_name
WHERE t.owner = 'GSPASS';
```





可以看到我新加的三条数据，产生了对应的三个分区，并且对应的表空间均为我在创建表的时候指定的

![image-20231025151242363](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310251512569.png)

但是在navicat 的表设计中，看到表空间字段为空，并且如果在这里选择表空间并且保存的话，会报错

![image-20231025153640574](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310251536658.png)

在 navicat 右侧的表信息中也可以看到表空间为空，所以navicat对于分区表的显示是存在问题的，千万不要被误导

![image-20231025153717310](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310251537401.png)



##### 参考

[探秘Oracle表空间、用户、表之间的关系](https://blog.csdn.net/huyuyang6688/article/details/49282199)

[oracle表空间和数据文件](https://zhuanlan.zhihu.com/p/140929471)
