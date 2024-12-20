# Mysql

[TOC]

## 一、数据库基础



### Mysql 引擎

#### 介绍

1. `MyISAM`: B 树它的每个节点都是 Key.value 的二元组，它的 key 都是从左到右递增的排序，value 中存储数据。这种模式在读取数据方面的性能很高，因为有单独的索引文件，Myisam 的存 储文件有三个.frm 是表的结构文件，.MYD 是数据文件，.MYI 是索引文件。不过 Myisam 也 有些缺点它只支持表级锁，不支持行级锁也不支持事务，外键等，所以一般用于大数据存储
2. `InnoDB`: 这是 MySQL 5.6 以及之后的默认引擎。B+树。
3. `Memory`: 这是一个特殊的引擎，该引擎存取的数据，全部放在内存中，不会落入磁盘。因此当数据库宕机或重启后，数据就会丢失。自从 Redis 兴起之后，memory 引擎也式微了。





## 二、MySQL 基础

### 2.1 mysql 数据类型

#### 2.1.1 char 和 varchar 的区别

CHAR 和 VARCHAR 是最常用到的字符串类型，两者的主要区别在于：**CHAR 是定长字符串，VARCHAR 是变长字符串。**

CHAR 在存储时会在右边填充空格以达到指定的长度，检索时会去掉空格；VARCHAR 在存储时需要使用 1 或 2 个额外字节记录字符串的长度，检索时不需要处理。

CHAR 更适合存储长度较短或者长度都差不多的字符串，例如 Bcrypt 算法、MD5 算法加密后的密码、身份证号码。VARCHAR 类型适合存储长度不确定或者差异较大的字符串，例如用户昵称、文章标题等。

#### 2.1.2 DECIMAL 和 FLOAT/DOUBLE 的区别是什么？

DECIMAL 和 FLOAT 的区别是：**DECIMAL 是定点数，FLOAT/DOUBLE 是浮点数。DECIMAL 可以存储精确的小数值，FLOAT/DOUBLE 只能存储近似的小数值。**

DECIMAL 用于存储具有精度要求的小数，例如与货币相关的数据，可以避免浮点数带来的精度损失。

在 Java 中，MySQL 的 DECIMAL 类型对应的是 Java 类 `java.math.BigDecimal`。

#### 2.1.3 blob和text有什么区别？

+ blob用于存储二进制数据（图片，音视频），而text用于存储大字符串。 
+ blob没有字符集，text有一个字符集，并且根据字符集的校对规则对值进行排序 和比较

不推荐使用这两种类型

- 检索效率较低。
- 不能直接创建索引，需要指定前缀长度。
- 可能会消耗大量的网络和 IO 带宽。
- 可能导致表上的 DML 操作变慢。

#### 2.1.4 DATETIME和TIMESTAMP的异同？

DATETIME 类型没有时区信息，TIMESTAMP 和时区有关。

TIMESTAMP 只需要使用 4 个字节的存储空间，但是 DATETIME 需要耗费 8 个字节的存储空间。但是，这样同样造成了一个问题，Timestamp 表示的时间范围更小。

- DATETIME：1000-01-01 00:00:00 ~ 9999-12-31 23:59:59
- Timestamp：1970-01-01 00:00:01 ~ 2037-12-31 23:59:59

#### 2.1.5 drop、delete 与 truncate 区别？

**用法不同** 

+ drop: 删除数据和表结构。 
+ truncate (清空数据) : 删除表中的所有数据。 
+ delete（删除数据） : 使用where语句，删除符合条件的数据，如果不加 where 子句和 truncate 作用类似。 

**属于不同的数据库语言** 

+ truncate 和 drop 属于 DDL(数据定义语言)语句，操作立即生效，原数据不放到 rollback segment 中，不能回滚，操作不触发 trigger。

+ delete 语句是 DML (数据库操作语言)语句，这个操作会放到 rollback segement 中，事务提交之后才生效。

#### 2.1.6 UNION与UNION ALL的区别？

+ 如果使用UNION ALL，不会合并重复的记录行 

+ 效率 UNION ALL 高于 UNION





### 2.2 mysql 的基础架构（一条sql的执行流程）

![img](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202407181638067.png)

+ 先检查该语句 是否有权限 ，如果没有权限，直接返回错误信息，如果有权限会 先查询缓存 (MySQL8.0 版本以前)。 
+ 如果没有缓存，分析器进行语法分析 ，判断 sql 语句是否有语法错误。
+ 语法解析之后，优化器会对查询的语句进行优化，确定执行的方案。 
+ 完成查询优化后，执行器按照生成的执行计划 调用数据库引擎接口 ，返回执行结果。

### 2.3 mysql 存储引擎

#### 2.3.1 InnoDB 和 MyISAM 的区别

1. **事务支持**：MyISAM不提供事务支持；InnoDB 提供事务支持，实现了 SQL 标准定义了四个隔离级别，具有提交(commit)和回滚(rollback) 事务的能力。并且，InnoDB 默认使⽤的 REPEATABLE-READ（可重读）隔离级别是可以解决幻读 问题发生的（基于 MVCC 和 Next-Key Lock）
2. **最小锁粒度**：MyISAM只支持表级锁，更新时会锁住整张表，导致其它查询和更 新都会被阻塞。InnoDB支持行级锁。 
3. **索引类型**：数据结构都是B+树，但是MyISAM的索引为非聚簇索引，InnoDB的索引是聚簇索引。
4. **主键必需**：MyISAM允许没有任何索引和主键的表存在；InnoDB如果没有设定 主键或者非空唯一索引，就会自动生成一个6字节的主键(用户不可见)，数据是主 索引的一部分，附加索引保存的是主索引的值。 
5. **是否支持数据库异常崩溃后的安全恢复** ：MyISAM 不支持，而 InnoDB 支持。 使用 InnoDB 的数据库在异常崩溃后，数据库重新启动的时候会保证数据库恢复到崩溃前的状态。这个恢复的过程依赖于 redo log 。
6. **外键支持**：MyISAM不支持外键；InnoDB支持外键。

#### 2.3.2 MyISAM 和 InnoDB 如何选择？

选 InnoDB





## 三、MySQL 索引

### 3.1 对于mysql索引的理解

索引是一种用于快速查询和检索数据的数据结构，其本质可以看成是一种排序好的数据结构。

索引的优点主要有以下几条：

1. 通过创建唯一索引，可以保证数据库表中每一行数据的唯一性。
2. 可以大大加快数据的查询速度，这也是创建索引的主要原因。

增加索引也有许多不利的方面，主要表现在如下几个方面：

1. 创建索引和维护索引要耗费时间，并且随着数据量的增加所耗费的时间也会增加。
2. 索引需要占磁盘空间。

### 3.2 索引的数据结构

MySQL的默认的存储引擎InnoDB采用的B+树的数据结构来存储索引，选择B+树而不是B树的主要的原因是：

+ 数据不放在非叶子节点上，全部放在叶子节点上，非叶子结点可以放更多的索引信息，所以在磁盘IO操作时，每次读写的数据块可以更有效地使用，减少磁盘读写次数。
+ 叶子节点连成了一个循环链表 ，方便范围查询。

### 3.3 常见的索引

#### 3.3.1 主键索引

数据表的主键列使用的就是主键索引。

在 MySQL 的 InnoDB 的表中，当没有显示的指定表的主键时，InnoDB 会自动先检查表中是否有唯一索引且不允许存在 null 值的字段，如果有，则选择该字段为默认的主键，否则 InnoDB 将会自动创建一个 6Byte 的自增主键。

#### 3.3.2 二级索引

二级索引（Secondary Index）的叶子节点存储的数据是主键的值，也就是说，通过二级索引可以定位主键的位置，二级索引又称为辅助索引/非主键索引。

#### 3.3.3 聚簇索引与非聚簇索引

**聚簇索引:**（Clustered Index）即索引结构和数据一起存放的索引，并不是一种单独的索引类型。InnoDB 中的主键索引就属于聚簇索引。索引(B+树)的每个非叶子节点存储索引，叶子节点存储索引和索引对应的数据。

**非聚簇索引:**(Non-Clustered Index)即索引结构和数据分开存放的索引，并不是一种单独的索引类型。二级索引就属于非聚簇索引。MySQL 的 MyISAM 引擎，不管主键还是非主键，使用的都是非聚簇索引。

非聚簇索引的叶子节点并不一定存放数据的指针，因为二级索引的叶子节点就存放的是主键，根据主键再回表查数据。

#### 3.3.4 覆盖索引

如果一个索引包含（或者说覆盖）所有需要查询的字段的值，我们就称之为 **覆盖索引（Covering Index）** 。如果不是覆盖索引，那么就需要回表查询。

在 InnoDB 存储引擎中，非主键索引的叶子节点包含的是主键的值。这意味着，当使用非主键索引进行查询时，数据库会先找到对应的主键值，然后再通过主键索引来定位和检索完整的行数据。这个过程被称为“回表”。

**覆盖索引即需要查询的字段正好是索引的字段，那么直接根据该索引，就可以查到数据了，而无需回表查询。**



### 3.4 索引下推

索引条件下推优化 （Index Condition Pushdown (ICP) ） 是MySQL5.6添加 的，用于优化数据查询。 

+ 不使用索引条件下推优化时存储引擎通过索引检索到数据，然后返回给MySQL Server，MySQL Server进行剩余的条件判断。 
+ 当使用索引条件下推优化时，如果存在某些被索引的列的判断条件时，MySQL Server将这一部分判断条件 下推 给存储引擎，然后由存储引擎通过判断索引是否 符合MySQL Server传递的条件，只有当索引符合条件时才会将数据检索出来返回 给MySQL服务器。

索引下推优化可以减少存储引擎回表的次数，也可以减少MySQL服务器和存储引擎传输的数据量。

**什么时候会触发索引下推？**

1. 查询类型：只有使用了非聚簇索引的查询才可能触发ICP。这是因为在聚簇索引中，索引与数据行是紧密耦合的，所以没有额外的检索步骤。

2. WHERE 子句的结构：当查询的WHERE子句中的一部分可以使用索引进行过滤，而另一部分则不能时，ICP最有可能被触发。在这种情况下，可以使用索引的部分首先进行过滤，然后应用余下的条件。

   例如，考虑一个包含`first_name`和`last_name`的复合索引。以下查询可能会触发ICP：

   ```
   SELECT * FROM users WHERE first_name = 'John' AND last_name LIKE 'Smi%';
   ```

   在这个例子中，`first_name`的等值比较可以直接用索引进行过滤，而`last_name`的LIKE操作可能会触发ICP，这样只有满足`first_name`条件的索引条目才会进一步检查`last_name`。

### 3.5 索引失效的原因

+ or语句：若是or条件中有一个没有索引，那么就不走索引。
+ 如果字段类型是字符串，where时一定用引号括起来，否则会因为隐式类型转换，索引失效 
+ 模糊查询，如果%号在前面也会导致索引失效。 
+ 联合索引，在使用的时候没有遵循最左匹配法则，导致失效；
+ 在索引列上使用mysql的内置函数或者运算，索引失效。；
+ ==还有一种比较复杂的情况：就是使用联合索引的时候，where语句有 and 的两个并列条件，第一个条件使用了范围查询，第二个条件会索引失效，但是在MySQL5.6 之后，有了索引下推，解决了这个问题，索引下推，把where里的第二个条件放到存储引擎去判断，减少回表次数。==

通常情况下，想要判断出这条sql是否有索引失效的情况，可以使用explain执行计划来分析

### 3.6 创建索引的原则

+ 索引应该建在查询应用频繁的字段； 
+ 索引的个数应该适量，索引需要占用空间，更新时候也需要维护； 
+ 区分度低的字段，例如性别，不要建索引； 
+ 频繁更新的值，不要作为索引； 
+ 尽可能创建组合索引，而不是单列索引。 



## 四、MySQL 日志

### 4.1 常见的日志文件

mysql 的 **server 层**日志文件：

+ 慢查询日志 （slow query log）：慢查询日志是用来记录执行时间超过 long_query_time 这个变量定义的时长的查询语句。通过慢查询日志，可以查找出 哪些查询语句的执行效率很低，以便进行优化。
+ 二进制日志 （bin log）：关于二进制日志，它记录了数据库所有执行的DDL和 DML语句（除了数据查询语句select、show等），以事件形式记录并保存在二进 制文件中。

**InnoDB 引擎**日志文件：

+ 重做日志 （redo log）：用于实现持久性，在服务器或者数据库宕机的情况下，恢复已经写入到**缓冲池**，但是没有写入到磁盘的数据。
+ 回滚日志 （undo log）：用于实现事务的一致性和原子性，如果事务执行失败或调用 了rollback，导致事务需要回滚，就可以利用undo log中的信息将数据回滚到修改 之前的样子。

### 4.2 binlog 和 redo log 的区别

+ 日志的目的不同：binlog用于数据备份，主从同步（例如阿里的canal就是使用的binlog来做数据库增量日志解析），或者数据迁移；redo log 用于故障恢复。
+ 层级不同：binlog属于server层，所以bin log会记录所有与数据库有关的日志记录，包括InnoDB、MyISAM等存储引擎 的日志，而redo log属于 InnoDB 引擎的，只记InnoDB存储引擎的日志。 
+ 记录的内容不同：binlog记录逻辑日志，即一个事务的具体操作内容。而redo log记录的是关于每个磁盘上的页（Page）的更改的物理情况。 
+ 写入的时间不同：bin log仅在事务提交前进行提交，也就是只写磁盘一次。而在 事务进行的过程中，却可以不断有redo log buffer的内容被写入redo log中。 
+ 写入的方式也不相同：redo log是循环写入和擦除，bin log是追加写入，不会覆 盖已经写的文件。

### 4.3 为什么需要redo log？（为什么不直接每次把修改的数据页写入磁盘？）

+ 大小问题：一个数据页是16KB，而一条redo log 的日志记录可能就几十Byte
+ redolog 是在日志文件中顺序写，而数据页是随机写，因为数据页可能在硬盘中的任意位置，所以性能很差。

### 4.4 redo log 的两阶段提交

**目的：**

是为了保证 意外情况下的 redo log 和 binlog 的数据一致。

**原因：**

主要就是两种情况下都会有数据不一致的情况发生：

- 若是先写入redo log，后写入binlog：在两次写之间发生宕机，则redo log更新成功，binlog没有更新成功，那么用redolog恢复主节点数据，主节点有新的数据，用binlog恢复从节点数据，而binlog中并没有新的数据，导致从节点数据会和主节点不一致。
- 若是先写入binlog，后写入redo log：binlog更新成功，redolog没有更新成功，那么主节点数据有延迟。

**做法：**

提交redolog后先标记一个prepared状态，然后提交binlog，成功后再将redolog的状态变为commited状态。

### 4.5 redo log 的刷盘策略

redo log的写入不是直接落到磁盘，而是在内存中设置了一片称之为 redo log buffer 的连续内存空间，也就是 redo 日志缓冲区 。

+ log buffer 空间不足时：当日志量占满了 log buffer 总容量的一半左右，就需要把这些日志刷新到磁盘上。 
+ 事务提交时
+ 后台线程：有一个后台线程，大约每秒都会刷新一次 log buffer 中的 redo log 到磁盘。 
+ 正常关闭服务器时
+ 触发checkpoint规则：当redo log 文件达到上限时，会触发 checkpoint 规则，以释放 redo log 文件的空间，并且刷盘。



## 五、MySQL 锁

### 5.1 锁的分类

**锁的粒度**

+ 全局锁：对整个数据库实例加锁，常用在数据迁移、全库数据备份时。加锁后整个数据库就处于只读状态。
+ 表锁：
  + 普通表锁：锁住整个表。
  + 元数据锁：元数据锁可以确保在执行 DDL（数据定义语言）语句和 DML（数据操作语言）语句时的互斥。不允许在修改表结构的时候，进行数据的访问。
  + 意向锁：加行锁的时候，会自动加上意向锁。如果没有意向锁，那么加表锁的时候，需要依次去判断每一行数据是否是加了行锁，效率很慢，所有引入了意向锁。
+ 行锁：开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，**并发度高**。

**兼容性**

+ 共享锁（S Lock）,也叫读锁（read lock），相互不阻塞，但是会阻塞排他锁。
+ 排他锁（X Lock），也叫写锁（write lock），排它锁是阻塞的，只有一个请求能执行写入，并阻止其它锁读取正在写入的数据。

### 5.2 InnoDB 里的行锁实现

注意这是在InnoDB里的行锁的实现：

+ **Record Lock 记录锁**：记录锁就是直接锁定某行记录。当我们使用唯一性的索引(包括唯一索引和聚簇索引) 进行等值查询且精准匹配到一条记录时，此时就会直接将这条记录锁定。
+ **Gap Lock 间隙锁**：间隙 锁(Gap Locks) 的间隙指的是两个记录之间逻辑上尚未填入数据的部分,是一个左开右开空间。当我们使用用等值查询或者范围查询，并且没有 命中任何一个 record ，此时就会将对应的间隙区间锁定。
+ **Next-key Lock 临键锁**：临键指的是间隙加上它右边的记录组成的左开右闭区间。比如(1,6]、(6,8] 等。临键锁就是记录锁(Record Locks)和间隙锁(Gap Locks)的结合。当我们使用范围查询，并且命中了部分 record 记 录，此时锁住的就是临键区间。==mysql默认行锁类型就是 临键锁(Next-Key Locks) 。 当使用唯一性索引，等值查询匹配到一条记录的时候，临键锁(Next-Key Locks)会退 化成记录锁；没有匹配到任何记录的时候，退化成间隙锁。==

间隙锁(Gap Locks) 和 临键锁(Next-Key Locks) 都是用来配合MVCC解决幻读问题的， 在已提交读（READ COMMITTED）RC 隔离级别下， 间隙锁(Gap Locks) 和 临键锁 (Next-Key Locks) 都会失效！

### 5.3 行级锁失效（行级锁退化为表锁）

InnoDB 的行锁是针对索引字段加的锁，当我们执行 sql语句时，如果没有命中索引或者==索引失效==的话，就 会导致全表扫描，这时候行锁会退化为表锁。

### 5.4 当前读 和 快照读

**快照读（一致性非锁定读）**：

快照读就是会去读取记录的历史版本，每行记录可能存在多个历史版本。当mysql数据库在事务隔离级别 RC(读取已提交) 和 RR（可重读）下，InnoDB 就会使用⼀致性非锁定读：

+ 在 RC 级别下，快照读总是读取最新的⼀份快照数据。 
+ 在 RR 级别下，快照读总是读取这个事务开始时的数据版本。

当我们使用单纯的 SELECT 语句的时候就是快照读，除了下面这两 SELECT 语句

```sql
SELECT ... FOR UPDATE
SELECT ... LOCK IN SHARE MOD
```

**当前读 （一致性锁定读）**就是给行记录加 X 锁或 S 锁。 

### 5.5 乐观锁和悲观锁

**悲观锁**

悲观锁认为被它保护的数据是极其不安全的，每时每刻都有可能被改动，一个事务 拿到悲观锁后，其他任何事务都不能对该数据进行修改，只能等待锁被释放才可以 执行。 数据库中的行锁，表锁，读锁，写锁均为悲观锁。

**乐观锁**

乐观锁认为数据的变动不会太频繁。 乐观锁通常是通过在表中增加一个版本(version)或时间戳(timestamp)来实现，比如CAS和MVCC。



## 六、MySQL 事务

### 6.1 事务的四大特性

1. 原子性（ Atomicity ） ： 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么全不完成； 
2. ⼀致性（ Consistency ）： 执行事务前后，数据保持⼀致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的； 
3. 隔离性（ Isolation ）： 并发访问数据库时，⼀个用户的事务不被其他事务所干扰，各并发事务之间是相互隔离的； 
4. 持久性（ Durabilily ）： ⼀个事务被提交之后。它对数据库中数据的改变是持久的，即使数据 库发生故障也不应该对其有任何影响。

只有保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。 也就是说 A、I、D 是手段，C 是目的。

### 6.2 MySQL 的 ACID 如何实现

1. 原子性：依靠 undo log 实现，如果事务发生错误，或者回滚事务，就用undo log 确保事务全部撤回。
2. 一致性：其他三个特性得到保证，即可实现一致性。这是目的。
3. 隔离性：通过锁和MVCC来实现。
4. 持久性：通过 redo log 来实现持久性，将发生宕机时未写入磁盘的数据，重新写入磁盘。

### 6.3 并发事务带来的问题 以及 事务的隔离级别

**并发事务带来的问题**

+ 脏读：事务 A、B 交替执行，事务 A 读取到事务 B 未提交的数据。
+ 不可重复读：在一个事务中，两次读取同一条记录，却返回了不同的数据。
+ 幻读：⼀个事务A读取了一个范围的结果集， 接着另⼀个事务B插入了一些数据。然后事务A再次查询相同的范围，两次的结果集不一样。

**隔离级别**

- **读未提交**：可能会读到其他没有提交事务的数据修改。可能会导致脏读、幻读或不可重复读。实现方式就是读最新的数据就行，当前读。
- **读已提交**：可以读到其他已经提交事务的数据修改。解决了脏读，有不可重复读和幻读风险。
- **可重复读**：解决了脏读和不可重读的风险。有幻读风险。mysql默认隔离级别。
- **串行化**：解决所有这三个风险。但是效率很低。通过加读写锁来实现。

读提交和可重复读通过MVCC和锁来实现。

### 6.4 MVCC（多版本并发控制）

三个概念

 - 隐藏字段和版本链

   - 每个表创建的时候，除了你创建的表字段外还有几个隐藏字段：包括事务id和上个版本数据的指针。
   - 每一次修改一行数据时，都会先把undo日志记录下来，当前行的版本指针就会指向旧的数据行，这样就形成了一个版本链。

 - 读视图（Read View）：四个字段的数据结构
   - 生成当前视图的事务id
   - 当前活跃事务中最小id（事务id是按照递增分配的，比最小id更小的就是已经提交的事务）
   - 表示生成 ReadView 时系统中应该分配给下一个事务的 id 值

   + 当前所有活跃事务集合。

 - 一致性算法：
   - 如果被访问版本的 事务id（DB_TRX_ID） 属性值与 ReadView 中的 创建读视图的事务id（creator_trx_id） 值相同， 意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
   - 若是被访问的版本的事务id小于读视图上最小事务id，表示该事物已经提交，数据可见。
   - 若是被访问的版本的事务id大于等于读视图上最大事务id，表示该事务还没提交，数据不可见。
   - 上面两种情况不满足，再依次对比是否存在于当前活跃事务id集合中：
     - 若存在集合中，表示当前事务还没提交，不可见。
     - 若是不存在，表示当前事务已经提交，可见（在生成读视图的时刻，该事务已经提交，所以不在该集合中）

**总结：**

 - 根据读视图和一致性算法判断版本链上的数据是否可读。
 - 如果是读已提交RC的隔离级别：事务中每次快照读都需要生成一个读视图；
 - 如果是可重复读RR的隔离级别，永远使用事务中第一次**快照读**生成的读视图。

### 6.5 RR下如何防止幻读

`InnoDB`存储引擎在 RR 级别下通过 `MVCC`和 `Next-key Lock` 来解决幻读问题：

**1、执行普通 `select`，此时会以 `MVCC` 快照读的方式读取数据**

在快照读的情况下，RR 隔离级别只会在事务开启后的第一次查询生成 `Read View` ，并使用至事务提交。所以在生成 `Read View` 之后其它事务所做的更新、插入记录版本对当前事务并不可见，实现了可重复读和防止快照读下的 “幻读”

**2、执行 select...for update/lock in share mode、insert、update、delete 等当前读**

在当前读下，读取的都是最新的数据，如果其它事务有插入新的记录，并且刚好在当前事务查询范围内，就会产生幻读！`InnoDB` 使用 **Next-key Lock** **临键锁**来防止这种情况。当执行当前读时，会锁定读取到的记录的同时，锁定它们的间隙，防止其它事务在查询范围内插入数据。只要我不让你插入，就不会发生幻读



## 七、MySQL 优化

### 7.1 定位慢查询

一般来说有两种方式：

1. 使用运维工具（例如 Skywalking、Prometheus 等）来监测接口的执行时间，并且可以分析接口中那部分执行较慢，来定位是否为sql执行过慢。
2. 还可以在测试环境下，开启mysql的慢日志查询，可以记录执行时间超过阈值的sql 到慢查询日志中。

其实在我们的项目中定位慢查询，主要就是测试的时候接口响应时间过长，然后把接口中的sql语句直接拿到数据库工具中执行一下，看看耗时，就可以定位了。不过我也了解过oracle定位慢查询的方式，主要是通过从动态性能视图(V$SQLAREA)中查询sql的执行时间、次数、语句信息来定位慢sql。但是主要的定位慢查询的方式还是运维检测工具。



### 7.2 MySQL 执行计划分析

当我们使用mysql的执行计划explain来去查看这条sql的执行情况时，我们一般重点关注几个字段：

+ **key 和 key_len 字段**：key 列表示 MySQL 实际使用到的索引。可以通过key和key_len检查是否命中了索引，如果是联合索引可以通过 key_len 判断命中了几个索引字段，也可以判断索引是否有失效的情况，
+ **type 字段**：type显示的是访问类型，主要有这几种类型：system > const > eq_ref > ref > range > index > all。阿里巴巴的java开发手册推荐，至少要达到 range 级别，要求是 ref 级别，如果可以是 const 最好。 说明： 
  + const：表示使用唯一索引扫描，并且只匹配一条数据。
  + eq_ref：唯一性索引扫描，并且也只匹配一条数据，与const不同的是eq_ref用于联表的查询。
  + ref：非唯一索引扫描，返回匹配某个单独值的所有行。
  + range：只检索给定范围的行，返回匹配指定区间的所有行。
  + index：全索引扫描，一般是因为没有where条件。
  + all：全表扫描。

+ **extra字段**：这列包含了 MySQL 解析查询的额外信息。
  + Using index：表示使用了覆盖索引，且不需要访问表的数据行
  + Using index condition：表示使用了索引条件下推
  + Using where：表明where语句中的条件无法完全使用索引过滤，MYSQL服务器层将在存储引擎层返回行以后再应用WHERE过滤条件。表示我们的sql语句依然有优化空间避免在服务器层应用where



### 7.3 深度分页场景

说明：MySQL 并不是跳过 offset 行，而是取 offset+N 行，然后返回放弃前 offset 行，返回 N 行，那当 offset 特别大的时候，效率就非常的低下，要么控制返回的总页数，要么对超过特定阈值的页数进行 SQL 改写。 

正例：先使用子查询快速定位需要获取的 id 段，然后再关联： 

```sql
SELECT a.* FROM 表 1 a, (select id from 表 1 where 条件 LIMIT 100000,20 ) b where a.id=b.id
```



### 7.4 优化经验

1. 不使用 select *，只查询需要的列
2. 数据量大的情况下，使用分页，使用子查询先查询id，优化深度分页
3. 查询时 严禁使用三个表以上的 join
4. 页面搜索严禁左模糊或者全模糊，如果需要请走搜索引擎来解决。
5. 利用覆盖索引来进行查询操作，避免回表。
6. 防止因字段类型不同造成的隐式转换，导致索引失效。
7. 避免列上的函数运算，可能导致索引失效
8. 不得使用外键与级联，一切外键概念必须在应用层解决。
9. 建组合索引的时候，区分度最高的在最左边。使用组合索引的时候，注意最左匹配原则。
10. 一定要使用union all，不要使用 union，可以在代码逻辑完成去重。
11. 当数据量 太大时，考虑分库分表



## 八、其他问题

### 8.1 MySQL 主从复制原理

+ master数据写入，更新binlog 
+ master创建一个dump线程向slave推送binlog 
+ slave连接到master的时候，会创建一个IO线程接收binlog，并记录到relay log中继 日志中 
+ slave再开启一个sql线程读取relay log事件并在slave执行，完成同步 



### 8.2 你们的数据量那么大，有没有分库分表

我们的业务时存在水平分表的，比如我们的ETC或者MTC数据，大约数据量一天在 20 到 40万之间，我们对这个数据的处理就是水平分表，因为它一个月的数据在一千万左右，我们当时是按照它的年月来分表的，一张表一个月的数据，表名中会带有年月，我们在代码中，或者数据同步中，都是用年月拼接表名，访问对应的数据。

还有一个数据就是 门架数据，这个数据量大概一天在四百万左右，这个表我们的处理方式是只保留一天的数据，每天凌晨之后首先用 oracle的 定时任务 完成对于这个表的清洗，先确保数据的正确性，因为我们的数据是接口数据，那边接口是每次只提供了五分钟的数据，并且可能会出现多提供了几十秒，或者少提供了几十秒的情况 ，可能出现重复数据。所以我们凌晨之后先oracle定时任务清洗数据，然后用springboot的定时任务，进行数据计算，包括计算每天的进津车流量，出津车流量，域内流量，每小时流量等，以提供给我们的业务功能使用。然后利用 dataX-Web 的定时任务将前一天的数据全部写到我们的hadoop的hdfs中，最后我们再删除表中前一天的所有数据。



> 服务器相关信息：
>
> + oracle数据库的硬盘空间：挂载的空间一共19T
> + hadoop集群空间： 一共十二台服务器，每个挂载空间都在5T
>
> oracle 存储信息：
>
> + 我们为每个业务都建立了独立的表空间，开启了分区机制（按天分区），并且开启了自动扩容机制
> + ETC和MTC的每月数据量：1-2GB
> + 门架数据每月数据量：15-20GB
