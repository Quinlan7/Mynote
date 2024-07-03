# Oracle报错

## ORA-12514, TNS:listener does not currently know of service requested in connect descriptor



### 问题排查

1. **数据库连接不上**，**我们直接服务器进入命令行**

先

```shell
sqlplus
```



![image-20231205192146251](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312051921383.png)

无法进入命令行，报错：Oracle 数据库尝试创建审计追踪文件，由于没有空间无法创建。

> 审计追踪文件的作用：
>
> Oracle 数据库审计追踪文件用于记录数据库中发生的安全相关事件和活动，以提供有关数据库的详细审计信息。这些文件有助于监视和追踪数据库的使用情况，以确保数据库的安全性、合规性和可追溯性。以下是审计追踪文件的一些主要作用：
>
> 1. **安全监控：** 审计追踪文件记录了数据库中的登录、注销、授权、权限更改等安全相关事件。通过审计文件，数据库管理员可以检查是否有未经授权的访问尝试，以及数据库用户的活动是否符合预期。
>
> 2. **合规性：** 许多行业和法规要求数据库系统实施严格的安全审计和监控。审计追踪文件可以用于满足这些合规性要求，例如，PCI DSS（支付卡行业数据安全标准）要求对数据库的访问和操作进行详细的审计记录。
>
> 3. **问题排查：** 当出现数据库性能或安全性问题时，审计追踪文件可用于进行问题排查。通过查看文件，管理员可以了解在特定时间数据库发生了什么，有助于快速定位和解决问题。
>
> 4. **用户行为分析：** 审计追踪文件记录了用户的活动，包括 SQL 语句的执行、表的访问等。这些信息对于分析用户行为、优化查询以及进行容量规划等方面都是有用的。
>
> 5. **数据库性能优化：** 通过审计追踪文件，可以了解数据库中哪些查询经常被执行，哪些表被频繁访问，从而帮助优化数据库性能。
>
> 在 Oracle 数据库中，可以通过配置审计选项和参数来控制审计追踪文件的生成。审计追踪文件通常包含详细的事件信息，如时间戳、事件类型、用户、主机信息、SQL 语句等。

2. **查询剩余空间**

![image-20231205192313922](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312051923000.png)

可以看到我们的根目录已经全部占用了，应该就是因为我们的审计追踪文件创建在这个目录之下，所以没有足够的空间继续创建了

### 解决问题

1. **查看我们的初始化配置文件，看一下审计追踪文件的默认创建位置**

一般是在这个路径下的 init.ora 文件

```
cd $ORACLE_HOME/dbs/
```

![image-20231205165432183](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312051654363.png)

可以看到我们的 audit_file_dest 路径就是我们的审计追踪文件的位置

audit_trail 是我们审计追踪文件的策略

- `NONE`（默认）：不进行审计。
- `OS`：使用操作系统级别的审计功能。这种情况下，审计追踪文件的管理由操作系统处理。
- `DB`：审计信息存储在数据库中，可以通过 SQL 查询来检索。数据库管理员需要定期清理过时的审计数据。

> 定期清理sql
>
> ```sql
> DBMS_AUDIT_MGMT.CLEAN_AUDIT_TRAIL(
>    audit_trail_type     => DBMS_AUDIT_MGMT.AUDIT_TRAIL_AUD_STD,
>    use_last_arch_timestamp => TRUE
> );
> ```

2. **找到审计追踪文件并删除**

查看环境变量

```
cat /etc/profile
```

![image-20231205194220565](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202312051942671.png)

进入 $ORACLE_BASE/admin/ORCLCDB/adump

直接删除这个文件夹下所有文件

```
rm -rf *
```

3. **进入oracle命令行重启服务器实例**

由于我的数据库实例使用的是PDB，所以我需要打开我指定的PDB

```
[root@oracledb dbs]# su oracle
[oracle@oracledb dbs]$ sqlplus /nolog

SQL*Plus: Release 19.0.0.0.0 - Production on Tue Dec 5 16:55:51 2023
Version 19.3.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

SQL> conn /as sysdba
Connected to an idle instance.

SQL> startup
ORACLE instance started.

Total System Global Area 4.0400E+10 bytes
Fixed Size		   30393616 bytes
Variable Size		 7381975040 bytes
Database Buffers	 3.2883E+10 bytes
Redo Buffers		  103821312 bytes
Database mounted.
Database opened.         " - rest of line ignored.

SQL> ALTER PLUGGABLE DATABASE ORCLPDB1 OPEN;

Pluggable database altered.

```



## oracle重启

首先进入oracle用户

```
启动 Oracle 监听服务

$ lsnrctl start

启动 Oracle 服务

$ sqlplus /nolog

SQL > conn / as sysdba

SQL > startup

验证：

SQL > select * from dual;

SQL > exit
————————————————
```

