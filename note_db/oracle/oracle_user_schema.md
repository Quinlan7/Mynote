# oracle 表空间，分区表基础知识以及实操

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



### oracle 表空间，分区表创建实操以及踩坑

*navicat对于oracle分区信息的查看十分有限，且有bug*

#### 创建表空间

```sql
-- 创建表空间（默认就是永久的，没有配置的参数都是直接使用默认的就ok）
/*
    需要确定物理文件路径确实存在，否则会报错
    并且需要确定物理文件路径的权限是oracle用户
    如果是root用户，需要修改权限
    修改所有者：chown -R oracle:oinstall /mnt/sdd_tmp/oracle/gs
    修改权限：chmod -R 777 /mnt/sdd_tmp/oracle/gs
    */
/*创建表空间*/
create tablespace GTY_BILLING_TRANSACTION
/*表空间物理文件名称*/
datafile '/mnt/sdd_tmp/oracle/gs/gtybillingtransaction.dbf'
-- 大小 500M，每次 5M 自动增大，最大不限制(数据文件策略)
size 1024M autoextend on next 100M maxsize unlimited
--这一行指定了数据文件的数据块大小为8K字节。这是典型的块大小，但你可以根据需要进行调整。
BLOCKSIZE 8192
--这一行指定表空间采用本地管理，这意味着段的管理由Oracle自动处理。
EXTENT MANAGEMENT LOCAL
-- 这一行指定表空间中的段空间管理采用自动模式，Oracle将自动管理段的空间分配和释放。
SEGMENT SPACE MANAGEMENT AUTO
--这一行将表空间设置为在线状态，允许用户访问并向其中插入数据。
ONLINE;


-- 删除表空间
DROP TABLESPACE TOLL_ENTRY_DETAILS INCLUDING CONTENTS AND DATAFILES;


-- 查询表空间是否存在
SELECT tablespace_name FROM dba_tablespaces WHERE tablespace_name = 'TOLL_ENTRY_DETAILS';

-- 修改表的对应表空间
ALTER TABLE your_table_name
MOVE TABLESPACE new_tablespace_name;
```





#### 创建分区表

```sql
CREATE TABLE toll_entry_details
(
    id VARCHAR2(50),
    passid VARCHAR2(50),
    vehicletype NUMBER,
    enaxlecount NUMBER,
    lanetype NUMBER,
    vehicleclass NUMBER,
    enweight VARCHAR2(50),
    entime TIMESTAMP,
    vehicleid VARCHAR2(50),
    mediatype NUMBER,
    entolllaneid VARCHAR2(50)
)
TABLESPACE TOLL_ENTRY_DETAILS -- 指定共同的表空间
PARTITION BY RANGE (entime) -- 使用日期作为分区键
INTERVAL(NUMTODSINTERVAL(1, 'DAY')) -- 按天分区
(
  PARTITION initial_partition VALUES LESS THAN (TO_DATE('2023-01-01', 'YYYY-MM-DD'))
);


COMMENT ON TABLE toll_entry_details IS '收费站入口数据明细';
COMMENT ON COLUMN toll_entry_details.vehicletype IS '收费车型';
COMMENT ON COLUMN toll_entry_details.enaxlecount IS '入口轴数';
COMMENT ON COLUMN toll_entry_details.lanetype IS '车道类型';
COMMENT ON COLUMN toll_entry_details.vehicleclass IS '车种';
COMMENT ON COLUMN toll_entry_details.enweight IS '入口重量';
COMMENT ON COLUMN toll_entry_details.passid IS '通行标识ID';
COMMENT ON COLUMN toll_entry_details.id IS '入口处理编号';
COMMENT ON COLUMN toll_entry_details.entime IS '入口处理时间';
COMMENT ON COLUMN toll_entry_details.vehicleid IS '实际车辆车牌号码+颜色';
COMMENT ON COLUMN toll_entry_details.mediatype IS '通行介质类型';
COMMENT ON COLUMN toll_entry_details.entolllaneid IS '入口车道编号';


--删除表
DROP TABLE toll_entry_details;
```





#### 测试是否为分区表以及表空间是否生效

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



#### 查询分区表相关操作

```sql
-- 显示数据库所有分区表的信息：DBA_PART_TABLES
-- 显示当前用户可访问的所有分区表信息：ALL_PART_TABLES
-- 显示当前用户所有分区表的信息：USER_PART_TABLES

-- 显示当前用户所有分区表的分区列信息：USER_PART_KEY_COLUMNS
-- 显示当前用户可访问的所有分区表的分区列信息：ALL_PART_KEY_COLUMNS
-- 显示分区列 显示数据库所有分区表的分区列信息：DBA_PART_KEY_COLUMNS


-- 查询某张表是否分区
SELECT table_name, partitioned
FROM all_tables
WHERE table_name = 'TOLL_ENTRY_DETAILS';

-- 查询某张表的分区信息
SELECT * FROM ALL_PART_TABLES WHERE TABLE_NAME = 'TOLL_ENTRY_DETAILS';


-- 查询所有分区表 一个表对应n个分区就有n条记录
select * from all_tab_partitions where table_name='TOLL_ENTRY_DETAILS';
--2.3、指定分区查询数据
select * from tablename partition(partitionname);
--2.4、不指定分区查询数据
select * from tablename;
```







##### 参考

[探秘Oracle表空间、用户、表之间的关系](https://blog.csdn.net/huyuyang6688/article/details/49282199)

[oracle表空间和数据文件](https://zhuanlan.zhihu.com/p/140929471)
